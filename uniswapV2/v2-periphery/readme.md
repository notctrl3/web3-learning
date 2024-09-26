## 项目结构
   - UniswapV2Migrator.sol：迁移合约，从 V1 迁移到 V2 的合约
   - UniswapV2Router01.sol：路由合约 01 版本
   - UniswapV2Router02.sol：路由合约 02 版本，相比 01 版本主要增加了几个支持交税费用的函数
   - interfaces：接口都统一放在该目录下
   - libraries：存放用到的几个库文件
   - test：里面有几个测试用的合约
   - examples：一些很有用的示例合约，包括 TWAP、闪电兑换等

---

## UniswapV2Library.sol
### 主要函数
- sortTokens：对两个 token 进行排序
- pairFor：计算出两个 token 的 pair 合约地址
- getReserves：获取两个 token 在池子里里的储备量
- quote：根据给定的两个 token 的储备量和其中一个 token 数量，计算得到另一个 token 等值的数值
- getAmountOut：根据给定的两个 token 的储备量和输入的 token 数量，计算得到输出的 token 数量，该计算会扣减掉 0.3% 的手续费
- getAmountIn：根据给定的两个 token 的储备量和输出的 token 数量，计算得到输入的 token 数量，该计算会扣减掉 0.3% 的手续费
- getAmountsOut：根据兑换路径和输入数量，计算得到兑换路径中每个交易对的输出数量
- getAmountsIn：根据兑换路径和输出数量，计算得到兑换路径中每个交易对的输入数量
#### pairFor()
``` solidity
    // calculates the CREATE2 address for a pair without making any external calls
    // create2获取地址 = hash("0xFF",创建者地址, salt, initcode) 
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f' // init code hash
            ))));
    }
```
`hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f'`为UniswapV2Pair的创建字节码
#### getAmountOut
``` solidity
    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'UniswapV2Library: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(997);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(1000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }
```
根据AMM乘积做市公式`x * y = k`，在swap之后k值不变
``` text
reserveIn * reserveOut = (amountIn + reserveIn) * (reserveOut - amountOut)
=> amountOut = (reserveOut * amountIn) / (reserveIn + amountIn)

// 加上3%fee
=> amountOut = (reserveOut * amountIn * 997) / (1000 reserveIn + amountIn * 997)
```

#### getAmountsOut
返回兑换路径上每次转换能得到的代币输出数量
``` solidity
// performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal
view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }    
```

#### 
