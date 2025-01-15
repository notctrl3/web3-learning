## Swap
```
function swap(
    address recipient,          // 接收人
    bool zeroForOne,            // token0换token1还是token1换token0 token0->token1
    int256 amountSpecified,     // 为正数则为输入token的值，负数则为需要输出的值
    uint160 sqrtPriceLimitX96,  // 价格限制，用平方根表示的价格限制。
    bytes calldata data
) external override noDelegateCall returns (int256 amount0, int256 amount1)
```
### 主要流程
#### 1. 获取当前价格(tick)和可用流动性，初始化 Swap Cache 和 Swap State
```
        SwapCache memory cache =
            SwapCache({
                liquidityStart: liquidity,
                blockTimestamp: _blockTimestamp(),
                feeProtocol: zeroForOne ? (slot0Start.feeProtocol % 16) : (slot0Start.feeProtocol >> 4),
                secondsPerLiquidityCumulativeX128: 0,
                tickCumulative: 0,
                computedLatestObservation: false
            });

        bool exactInput = amountSpecified > 0;

        SwapState memory state =
            SwapState({
                amountSpecifiedRemaining: amountSpecified,
                amountCalculated: 0,
                sqrtPriceX96: slot0Start.sqrtPriceX96,
                tick: slot0Start.tick,
                feeGrowthGlobalX128: zeroForOne ? feeGrowthGlobal0X128 : feeGrowthGlobal1X128,
                protocolFee: 0,
                liquidity: cache.liquidityStart
            });
```
#### 2. 根据`zeroForOne`和`sqrtPriceX96`找到最近要穿过的tick及价格
在交换量未耗尽或未达到价格限制需要不断进行交易。根据`zeroForOne`和`sqrtPriceX96`找到最近要穿过的tick。如用token0换
token1，最近的tick为比当前价格大的最近的tick
```
step.sqrtPriceStartX96 = state.sqrtPriceX96;

(step.tickNext, step.initialized) = tickBitmap.nextInitializedTickWithinOneWord(
     state.tick,
     tickSpacing,
     zeroForOne
);
// ensure that we do not overshoot the min/max tick, as the tick bitmap is not aware of these bounds
   if (step.tickNext < TickMath.MIN_TICK) {
      step.tickNext = TickMath.MIN_TICK;
   } else if (step.tickNext > TickMath.MAX_TICK) {
      step.tickNext = TickMath.MAX_TICK;
   }

   // get the price for the next tick
   step.sqrtPriceNextX96 = TickMath.getSqrtRatioAtTick(step.tickNext);         
```
#### 3. 计算到最近一个已初始化的tick所需token量
因为每穿过一个tick，合约里可用的流动性会发生变化。所以先计算到最近的tick是否能支持所有交易，如果不能则剩下的交易留给下一个tick。
获取sqrtRatioNextX96：下一个价格。amountIn：本步骤需要输入的 Token0 数量。amountOut：本步骤将输出的 Token1 数量。
feeAmount:本步骤收取的费用金额。
```
// compute values to swap to the target tick, price limit, or point where input/output amount is exhausted
   (state.sqrtPriceX96, step.amountIn, step.amountOut, step.feeAmount) = SwapMath.computeSwapStep(
                state.sqrtPriceX96,
                (zeroForOne ? step.sqrtPriceNextX96 < sqrtPriceLimitX96 : step.sqrtPriceNextX96 > sqrtPriceLimitX96)
                    ? sqrtPriceLimitX96
                    : step.sqrtPriceNextX96,
                state.liquidity,
                state.amountSpecifiedRemaining,
                fee
            );
   // 本过程结束后还需交易的数额
   if (exactInput) {
       state.amountSpecifiedRemaining -= (step.amountIn + step.feeAmount).toInt256();
       state.amountCalculated = state.amountCalculated.sub(step.amountOut.toInt256());
   } else {
       state.amountSpecifiedRemaining += step.amountOut.toInt256();
       state.amountCalculated = state.amountCalculated.add((step.amountIn + step.feeAmount).toInt256());
   }
```
从一个价格到另一个价格所需的token公式`liquidity / sqrt(lower) -liquidity / sqrt(upper)`
#### 4. 更新当前价格和 Tick
```
if (state.sqrtPriceX96 == step.sqrtPriceNextX96) {
    if (step.initialized) {
        // 更新观察记录
        if (!cache.computedLatestObservation) {
            (cache.tickCumulative, cache.secondsPerLiquidityCumulativeX128) = observations.observeSingle(
                cache.blockTimestamp,
                0,
                slot0Start.tick,
                slot0Start.observationIndex,
                cache.liquidityStart,
                slot0Start.observationCardinality
            );
            cache.computedLatestObservation = true;
        }
        // 调整流动性
        int128 liquidityNet = ticks.cross(
            step.tickNext,
            (zeroForOne ? state.feeGrowthGlobalX128 : feeGrowthGlobal0X128),
            (zeroForOne ? feeGrowthGlobal1X128 : state.feeGrowthGlobalX128),
            cache.secondsPerLiquidityCumulativeX128,
            cache.tickCumulative,
            cache.blockTimestamp
        );
        if (zeroForOne) liquidityNet = -liquidityNet;

        state.liquidity = LiquidityMath.addDelta(state.liquidity, liquidityNet);
    }

    state.tick = zeroForOne ? step.tickNext - 1 : step.tickNext;
} 
else if (state.sqrtPriceX96 != step.sqrtPriceStartX96) {
    state.tick = TickMath.getTickAtSqrtRatio(state.sqrtPriceX96);
}
```
#### 5. 更新 Tick 和记录 Oracle 观察值
如果 Tick 发生了变化，记录新的观察值并更新池子的状态。
```
if (state.tick != slot0Start.tick) {
    (uint16 observationIndex, uint16 observationCardinality) = observations.write(
        slot0Start.observationIndex,
        cache.blockTimestamp,
        slot0Start.tick,
        cache.liquidityStart,
        slot0Start.observationCardinality,
        slot0Start.observationCardinalityNext
    );
    (slot0.sqrtPriceX96, slot0.tick, slot0.observationIndex, slot0.observationCardinality) = (
        state.sqrtPriceX96,
        state.tick,
        observationIndex,
        observationCardinality
    );
} else {
    slot0.sqrtPriceX96 = state.sqrtPriceX96;
}
```
#### 6. 更新流动性
```
    // update liquidity if it changed
     if (cache.liquidityStart != state.liquidity) liquidity = state.liquidity;
```
#### 7. 执行代币转账和回调
