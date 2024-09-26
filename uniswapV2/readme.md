#### uniswap包含的开源项目：
  - uniswap-interface
  - uniswap-v2-sdk
  - uniswap-sdk-core
  - uniswap-info
  - uniswap-v2-subgraph
  - uniswap-v2-core
  - uniswap-v2-periphery
  - uniswap-lib

  前三个是前端App项目，即[提供交易的项目](https://app.uniswap.org)，展示页面都写在`uniswap-interface`项目中，`uniswap-v2-sdk`
  和`uniswap-sdk-core`则是作为SDK而存在，`uniswap-interface`会引用到v2-sdk和sdk-core，通过 @uniswap/v2-sdk 和 @uniswap/sdk-core
  的方式引入到需要使用的TS文件中。`uniswap-interface`最新代码其实是跟线上同步的，即是集成了 V3 版本的。如果只想部署 V2 版本的前端，
  那可以找出历史版本的项目代码进行部署，如果是不带流动性挖矿功能，推荐2020年9月份的版本，如果是带挖矿功能，那可以试试2020年10月份的版本。
  `uniswap-info`则是Uniswap Analytics项目，对应于[官网页面](https://info.uniswap.org)，展示了一些统计分析数据，其数据主要是从 Subgraph 读取。
  `uniswap-v2-subgraph`则是Subgraph项目了    
  最后三个则是合约项目了，`uniswap-v2-core`就是核心合约的实现。`uniswap-v2-periphery`则提供了和UniswapV2进行交互的外围合约，主要就是路由合约。
  `uniswap-lib`则封装了一些工具合约  
  Uniswap还有一个流动性挖矿合约项目liquidity-staker
