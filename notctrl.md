## Notes

<!-- Content_START -->

### 2024.09.19
#### solidity
1. denial of service攻击，例如在循环退款中，用户在receive()fallback()中写入导致合约回退的代码
2. airdrop合约，ERC20，ERC721   
#### Defi mooc
##### 去中心化金融体系和中心化金融体系
##### 区块链中密码学应用
1. collision resistance. 即使hash函数domain远大于输出，也很难找到一对x,y => H(x) = H(y)
2. 在merkle tree数据结构可以通过merkle proof高效地验证某个特定的commit是否存在
3. 数字签名。传统的签名不适用于网络世界，于是通过产生公钥和私钥，利用私钥进行签名，通过公钥，元信息，签名验证信息的合法性。
##### sacling
1. payment channel.两个参与方通过预先将一笔资金锁定在通道中，其他交易线下进行来增加区块链吞吐量

### 2024.09.20
#### Defi mooc
##### sacling
1. payment channel
   两个参与方通过预先将一笔资金锁定在通道中，其他交易线下进行来增加区块链吞吐量
2. rollups
   - zk rollup
     协调者在L2上将roll up批量交易生成zk-SNARK证明，layer1不需要再次执行交易
     - 减少了 Layer 1 上的计算量： Layer 1 节点只需要验证证明，而不需要执行交易
     - 减少了 Layer 1 上的存储量： Layer 1 只需要存储证明和状态根，而不需要存储所有交易数据
     - 提高了交易处理的并行度： Layer 2 可以并行处理多个交易，而 Layer 1 只能串行处理交易
   - optimistic rollup
     采用"乐观"的方式,即假设交易都是正确的,不需要事先验证
     - 交易在 Layer 2 上执行： 用户在 OP Rollup 的 Layer 2 网络上发送交易
     - 协调者 (Sequencer) 收集和排序交易： 一个或多个协调者负责收集这些交易，并将它们排序并打包成批次
     - 状态根提交到 Layer 1： 协调者将每个批次的交易数据和新的状态根 (state root) 提交到以太坊 Layer 1 上的智能合约
     - 挑战期 (Dispute Period)： Layer 1 上会设置一个挑战期 (通常为 7 天)，任何人都可以对 Layer 2 提交的状态根提出质疑
     - 欺诈证明： 如果有人认为 Layer 2 提交的状态根是错误的，他们可以提交一个欺诈证明，证明 Layer 2 上的交易执行或状态转换是无效的
     - 验证欺诈证明： Layer 1 上的智能合约会验证欺诈证明，如果证明有效，就会回滚 Layer 2 提交的状态根，并惩罚提交错误状态根的协调者
### 2024.09.21
#### Defi mooc
##### 智能合约
##### 传统金融
### 2024.09.23
1. wtf solidty01-05
   补充下solidity基础知识 
<!-- Content_END -->
