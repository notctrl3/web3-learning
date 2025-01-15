`NonfungiblePositionManager`是一个专门管理 LP positions 的合约，遵循 ERC-721 标准（NFT）。它负责处理用户的 mint 和 burn 请求，并将流动性凭证（NFT）发送给用户。
### mint
#### 1. 调用`NonfungiblePositionManager`,传入相应参数
```
      (liquidity, amount0, amount1, pool) = addLiquidity(
            AddLiquidityParams({
                token0: poolKey.token0,               // token0地址
                token1: poolKey.token1,               // token1地址
                fee: poolKey.fee,                     // 费率
                tickLower: position.tickLower,        
                tickUpper: position.tickUpper,        // 价格区间，通过两个tick indexes确定
                amount0Desired: params.amount0Desired,// 希望存入的 token0 的数量
                amount1Desired: params.amount1Desired,// 希望存入的 token1 的数量
                amount0Min: params.amount0Min,        // 愿意接受的最小 token0 存入量
                amount1Min: params.amount1Min,        // 愿意接受的最小 token1 存入量
                recipient: address(this)              // 接收者地址
            })
        );
```
#### 2. 计算流动性
```
       // 获取当前价格，通过tick indexes获取价格区间
       (uint160 sqrtPriceX96, , , , , , ) = pool.slot0();
        uint160 sqrtRatioAX96 = TickMath.getSqrtRatioAtTick(params.tickLower);
        uint160 sqrtRatioBX96 = TickMath.getSqrtRatioAtTick(params.tickUpper);

        liquidity = LiquidityAmounts.getLiquidityForAmounts(
            sqrtPriceX96,
            sqrtRatioAX96,
            sqrtRatioBX96,
            params.amount0Desired,
            params.amount1Desired
       );
    }
```
```
  由于每个价格区间符合恒定乘积公式，设 token0，token1 数量为 x, y
  x * y = K
  token0 的价格 : p = y / x
  x = √￣（k/p）
  y = √￣kp
```
则计算流动性 L 分三种情况 (a为tick低点，b为tick高点) ：  
1. 当前价格低于PriceA，此时按 token0 的数量计算
   ```
      L = (token0数量 / 价格区间所有token0总数)* 当前流动性
      L = ( Δx / （√￣（k/Pa）- √￣（k/Pb）) *  √￣k
      L =  Δx / （√￣（1/Pa）- √￣（1/Pb）
   ```
2. 当前价格高于PriceB，此时按 token1 的数量计算
   ```
      L = (token1数量 / 价格区间所有token1总数)* 当前流动性
      L = ( Δy / （√￣（kPb）- √￣（kPa）) *  √￣k
      L =  Δy / （√￣Pb - √￣Pa)
   ```
3. 当前价格在（Pa, Pb）, 则为上面两个公式的较小值
#### 3. 更新标识价格区间的tick
tick的主要有两个属性`liquidityGross`和`liquidityNet`。liquidityGross指包含这个tick所有的position的流动性总和，liquidityNet为以这个
tick做上界的position的流动性减去以这个tick做下界的position的流动性的净值。
```
        // 当为true表明该tick从未被任何position引用转变为被引用 或者
        // 从被引用转为未被任何position引用
        flipped = (liquidityGrossAfter == 0) != (liquidityGrossBefore == 0);

        if (liquidityGrossBefore == 0) {
            // by convention, we assume that all growth before a tick was initialized happened _below_ the tick
            if (tick <= tickCurrent) {
                info.feeGrowthOutside0X128 = feeGrowthGlobal0X128;
                info.feeGrowthOutside1X128 = feeGrowthGlobal1X128;
                info.secondsPerLiquidityOutsideX128 = secondsPerLiquidityCumulativeX128;
                info.tickCumulativeOutside = tickCumulative;
                info.secondsOutside = time;
            }
            info.initialized = true;
        }

        info.liquidityGross = liquidityGrossAfter;

        // when the lower (upper) tick is crossed left to right (right to left), liquidity must be added (removed)
        info.liquidityNet = upper
            ? int256(info.liquidityNet).sub(liquidityDelta).toInt128()
            : int256(info.liquidityNet).add(liquidityDelta).toInt128();
```
#### 4. 更新position，tick点位图
postion以`owner，lower tick， upper tick`来存储。更新其流动性以及手续费计算，根据 flipped 更新 tick 点位图。Pool 合约重新通过liquidity计算所需
token数量，确保了添加的流动性与池子的当前状态完全一致。
```
       (amount0, amount1) = pool.mint(
            params.recipient,
            params.tickLower,
            params.tickUpper,
            liquidity,
            abi.encode(MintCallbackData({poolKey: poolKey, payer: msg.sender}))
        );

        require(amount0 >= params.amount0Min && amount1 >= params.amount1Min, 'Price slippage check');
```
```
        position = _updatePosition(
            params.owner,
            params.tickLower,
            params.tickUpper,
            params.liquidityDelta,
            _slot0.tick
        );

        if (params.liquidityDelta != 0) {
            if (_slot0.tick < params.tickLower) {
                // current tick is below the passed range; liquidity can only become in range by crossing from left to
                // right, when we'll need _more_ token0 (it's becoming more valuable) so user must provide it
                amount0 = SqrtPriceMath.getAmount0Delta(
                    TickMath.getSqrtRatioAtTick(params.tickLower),
                    TickMath.getSqrtRatioAtTick(params.tickUpper),
                    params.liquidityDelta
                );
            } else if (_slot0.tick < params.tickUpper) {
                // current tick is inside the passed range
                uint128 liquidityBefore = liquidity; // SLOAD for gas optimization

                // write an oracle entry
                (slot0.observationIndex, slot0.observationCardinality) = observations.write(
                    _slot0.observationIndex,
                    _blockTimestamp(),
                    _slot0.tick,
                    liquidityBefore,
                    _slot0.observationCardinality,
                    _slot0.observationCardinalityNext
                );

                amount0 = SqrtPriceMath.getAmount0Delta(
                    _slot0.sqrtPriceX96,
                    TickMath.getSqrtRatioAtTick(params.tickUpper),
                    params.liquidityDelta
                );
                amount1 = SqrtPriceMath.getAmount1Delta(
                    TickMath.getSqrtRatioAtTick(params.tickLower),
                    _slot0.sqrtPriceX96,
                    params.liquidityDelta
                );

                liquidity = LiquidityMath.addDelta(liquidityBefore, params.liquidityDelta);
            } else {
                // current tick is above the passed range; liquidity can only become in range by crossing from right to
                // left, when we'll need _more_ token1 (it's becoming more valuable) so user must provide it
                amount1 = SqrtPriceMath.getAmount1Delta(
                    TickMath.getSqrtRatioAtTick(params.tickLower),
                    TickMath.getSqrtRatioAtTick(params.tickUpper),
                    params.liquidityDelta
                );
            }
```
#### 5. 转账，并校验余额
```
    if (amount0 > 0) balance0Before = balance0();
    if (amount1 > 0) balance1Before = balance1();
    IUniswapV3MintCallback(msg.sender).uniswapV3MintCallback(amount0, amount1, data);
    if (amount0 > 0) require(balance0Before.add(amount0) <= balance0(), 'M0');
    if (amount1 > 0) require(balance1Before.add(amount1) <= balance1(), 'M1');
```
#### 6. 铸造 ERC721 LP凭证给用户，并将position元信息保存起来
```
        _mint(params.recipient, (tokenId = _nextId++));

        bytes32 positionKey = PositionKey.compute(address(this), params.tickLower, params.tickUpper);
        (, uint256 feeGrowthInside0LastX128, uint256 feeGrowthInside1LastX128, , ) = pool.positions(positionKey);

        // idempotent set
        uint80 poolId =
            cachePoolKey(
                address(pool),
                PoolAddress.PoolKey({token0: params.token0, token1: params.token1, fee: params.fee})
            );

        _positions[tokenId] = Position({
            nonce: 0,
            operator: address(0),
            poolId: poolId,
            tickLower: params.tickLower,
            tickUpper: params.tickUpper,
            liquidity: liquidity,
            feeGrowthInside0LastX128: feeGrowthInside0LastX128,
            feeGrowthInside1LastX128: feeGrowthInside1LastX128,
            tokensOwed0: 0,
            tokensOwed1: 0
        });
```

### burn
