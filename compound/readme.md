### compound
---
&nbsp;&nbsp;&nbsp;&nbsp;compound为借贷defi协议，其为多种underlying（原生资产）提供资金池，包含eth和eip20 token如wbtc。每种underlying对应一种cToken，用户抵押
underlying后再按照规定的兑换率来获取，例eth对应cEth。compound采用超额抵押来实现借款，协议会根据用户抵押总市场价值和抵押因子（Collateral Factor）
来确定用户可借出多少份额。compound采用代还款清算，针对借款总价值大于抵押总价值的用户清算人可选择借款人抵押的一种资产来进行清算，清算人获得抵押代币和激励    
&nbsp;&nbsp;&nbsp;&nbsp;compound主要有两种借款利率模型,直线型和拐点型。直线型块借款利率计算公式为`y=kx+b`,其中x为资金利用率（未偿还的借款金额/可用于借款总金额），k为以块为时间区块的斜率，b为以块为时间区块的基准利率，块借款利率则为`资金使用率 * 借款利率 *（1 - 储备金率）`,其中储备金率为资金池中不用借款的备用金。拐点型块借款利率
计算公式为`k2*(x - p) + (k*p + b)`,即当资金池中资金利用率未到达拐点前仍为直线型，而到达拐点后借款利率为两段直线累积，拐点后的直线斜率更大，意味着利率随着资金利用率的增加而增加得更快
#### 主要合约
 - InterestRateModel：利率模型的抽象合约，JumpRateModelV2、JumpRateModel、WhitePaperInterestRateModel 为其实现合约
 - Comptroller：审计合约，封装了全局使用的数据，以及很多业务检验功能。其入口合约为 Unitroller，也是一个代理合约
 - CToken：cToken 代币的核心逻辑实现都在该合约中，属于抽象基类合约，没有构造函数，且定义了 3 个抽象函数。CEther 是 cETH 代币的入口合约，直接继承自 CToken。而 ERC20 的 cToken 入口则是 CErc20Delegator，这是一个代理合约，而实际实现合约为 CErc20Delegate、CDaiDelegate 等
