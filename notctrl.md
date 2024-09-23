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
#### 	WTF Academy
1. wtf solidty01-05  
   补充下solidity基础知识
2. javascript异步  
    #### `Promise`   
     js提供promise对象做异步，Promise 对象有三种状态：pending（进行中）、fulfilled（已成功）和 rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。  
```js
const p = new Promise((resolve, reject) => {
  resolve('Hello, WTF JavaScript!')
  throw new Error('发生错误！')
})

p.then(
  (value) => {
    console.log(value)
  },
  (error) => {
    console.log(error)
  }
)

// Hello, WTF JavaScript!
```
   #### Promise api
   ##### Promise.all  
   `Promise.all` 方法用于将多个 `Promise` 实例，包装成一个新的 `Promise` 实例。等待所有 Promise 完成（fulfilled）或其中任何一个 Promise 拒绝（rejected）。如果所有 Promise 都 fulfilled，Promise.all 返回一个新的 Promise，该 Promise resolve 为一个包含所有 Promise resolve 值的数组，按照传入 Promise.all 的顺序排列。如果任何一个 Promise rejected，Promise.all 返回一个新的 Promise，该 Promise 立即 rejected，rejected 的原因是第一个 rejected 的 Promise 的 reason。 这意味着，即使其他 Promise 后来 fulfilled，Promise.all 也不会考虑它们的结果。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello, WTF JavaScript!')
  }, 1000)
})

const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello, WTF HTML!')
  }, 3000)
})

const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Hello, WTF CSS!')
  }, 2000)
})

Promise.all([p1, p2, p3]).then((res) => {
  console.log(res)
})

// ['Hello, WTF JavaScript!', 'Hello, WTF HTML!', 'Hello, WTF CSS!'] (3 秒后输出)
```
##### Promise.allSettled  
  等待所有 Promise 都完成（settled），无论它们是 fulfilled 还是 rejected。返回一个新的 Promise，该 Promise resolve 为一个数组，数组的每一项都是一个对象，表示每个 Promise 的最终状态。 每个对象都有一个
##### Promise.race
  返回最先完成的，无论它们是 fulfilled 还是 rejected。
##### Promise.any
  传递最先fulfilled的promise实例返回值，若都rejected则返回reject对象
##### Promise.resolve
  用来创建一个 `resolved` 的 `Promise`  
```js
Promise.resolve('Hello, WTF JavaScript!')
```
##### Promise.reject
  用来创建一个 `rejected` 的 `Promise`  
```js
Promise.reject(new Error('发生错误！'))
```  
#### async/await
一个函数前面如果加上 `async` 关键字 ，那么该函数就会返回一个 `Promise`  
`await` 会暂停 `async` 函数的执行，直到它右边的 Promise resolve 或 reject  
`await` 只能用于 `Promise`。如果你尝试 `await` 一个非 `Promise` 值，它会直接返回该值  
`await` 使异步代码看起来更像同步代码，更容易理解和维护。 它避免了 .then() 链式调用的嵌套，使代码更简洁清晰  
可以使用 try...catch 块来捕获 await 抛出的异常，从而处理 `Promise` 的 reject 情况  
   
<!-- Content_END -->
