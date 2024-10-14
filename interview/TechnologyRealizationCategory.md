### 1、代理需要哪种特殊的 CALL 才能⼯作？ 
### What kind of special CALL is needed for proxies to work? 

Solidity中有三种调⽤函数可以实现合约间的函数调⽤，它们分别是call、delegatecall和callcode。其中，delegatecall是⼀种特殊的调⽤⽅式，它允许我们在主合约的上下⽂中加载和调⽤另
⼀个合约的代码。被调⽤合约的代码被执⾏，但被调⽤合约所做的任何状态改变实际上是在主合约的存储中进⾏的，⽽不是在被调⽤合约的存储中。
There are three call functions in Solidity that enable function calls between contracts, they are call, delegatecall, and callcode. Delegatecall is a special call that allows us to load and call another contract's code in the context of the master contract. The called contract's code is 
executed, but any state changes made by the called contract are actually made in the master contract's storage, not in the called contract's storage. 

### 2、在 EIP-1559 之前，如何计算以太坊交易的成本？ 
### Before EIP-1559, how was the cost of an Ether transaction calculated? 

在EIP-1559之前，以太坊交易的成本由矿⼯通过拍卖机制来决定。矿⼯会选择最⾼出价的交易，并将其包含在下⼀个区块中。交易的成本由两个因素决定：Gas Price和Gas Limit。Gas Price是以太
坊⽹络中的⼀种计量单位，⽤于衡量交易的复杂性。Gas Limit是指交易可以使⽤的最⼤Gas数量。交易的成本等于Gas Price乘以Gas Limit。因此，交易的成本取决于Gas Price和Gas Limit的值，这些值
由交易的发送者设置。
Prior to EIP-1559, the cost of an Ether transaction was determined by miners through an auction mechanism. Miners would select the highest bidding transaction and include it in the 
next block. The cost of a transaction is determined by two factors: the Gas Price and the Gas Limit. Gas Priceis a unit of measurement in the Ether network that measures the complexity of a 
transaction.The Gas Limit is the maximum amount of Gas that can be used for a transaction. The cost of a transaction is equal to the Gas Price multiplied by the Gas Limit.Therefore, the cost of a 
transaction depends on the values of Gas Price and Gas Limit, which are set by the sender of the transaction. 

### 3、ETH转账的底层原理是什么？ 
### What is the underlying principle of ETH transfers? 

底层原理就是修改链上数据，可以回答详细⼀点就是交易过程发⽣了什么。
  以下是ETH转账的详细过程： 
  1.连接到以太坊⽹络并加载您的私钥。您可以使⽤go-ethereum客⼾端来完成此操作。 
  2.获取发送帐⼾的随机数（nonce）。每个新事务都需要⼀个nonce，这是⼀个仅使⽤⼀次的数字。如果是发送交易的新帐⼾，则该随机数将为“0”。 
  3.将ETH转换为wei，因为这是以太坊区块链所使⽤的货币单位。以太⽹⽀持最多18个⼩数位，因此1个ETH为1加18个零。 
  4.设置您将要转移的ETH数量，以及燃⽓限额和燃⽓价格。燃⽓限额应设上限为“21000”单位，⽽燃⽓价格必须以wei为单位设定。 
  5.需要确定您要将ETH发送给哪个地址。这是接收地址。 
  6.接下来，您需要⽣成未签名的以太坊事务。这个函数需要接收nonce，地址，值，燃⽓上限值，燃⽓价格和可选发的数据。发送ETH的数据字段为“nil”（nil是go语⾔的null）。 
  7.使⽤发件⼈的私钥对事务进⾏签名。为此，您可以使⽤go-ethereum客⼾端提供的SignTx⽅法，该⽅法接受⼀个未签名的事务和您之前构造的私钥。
  8.在客⼾端上调⽤“SendTransaction”将已签名的事务⼴播到整个⽹络。这将向⽹络中的所有节点发送交易，并等待矿⼯将其打包到区块中。
The underlying principle is the modification of the data on the chain, which can be answered in a little more detail is what happens during the transaction. 
Here is the detailed process of ETH transfer: 
1. Connect to the ethereum network and load your private key. You can use the go-
ethereum client to do this. 
2. Get a random number (nonce) of sending accounts. Each new transaction requires a nonce, which is a number that is only used once. If it is a new account sending the transaction, 
the random number will be "0". 
3. Convert ETH to wei, as this is the unit of currency used by the Ethernet blockchain. Ethernet supports up to 18 decimal places, so 1 ETH is 1 plus 18 zeros. 
4. Set the amount of ETH you want to transfer, as well as the gas limit and gas price. The gas limit should be capped at "21000" units and the gas price must be set in wei. 
5. You need to determine to which address you want to send the ETH. This is the receiving address. 
6. Next, you need to generate an unsigned Ether transaction. This function needs to receive the nonce, address, value, gas cap, gas price and optionally send data. The data field for sending 
ETH is "nil" (nil is the go language for null). 
7. Sign the transaction using the sender's private key. To do this, you can use the SignTx method provided by the go-ethereum client, which accepts an unsigned transaction and the 
private key you constructed earlier. 
8. Call "Send Transaction" on the client to broadcast the signed transaction to the entire network. This sends the transaction to all nodes in the network and waits for the miner to 
package it into a block. 

### 4、在区块链上创建随机数的挑战是什么？ 
### What is the challenge of creating random numbers on a blockchain? 

在区块链上创建随机数的挑战是确保⽣成的随机数是真正随机的。由于区块链是⼀个公开的分布式账本，因此任何⼈都可以查看和验证交易。这意味着，如果随机数⽣成算法不够随机，那么攻击者
可能会通过分析交易来预测随机数的值，从⽽破坏系统的安全性。为了解决这个问题，⼀些技术已经被提出，例如使⽤区块链上的随机数⽣成器，或者使⽤多个随机数⽣成器来⽣成随机数，以确保⽣成
的随机数是真正随机的。
The challenge of creating random numbers on a blockchain is to ensure that the random numbers generated are truly random. Since the blockchain is a public distributed ledger, anyone 
can view and verify transactions. This means that if the random number generation algorithm is not random enough, then an attacker may be able to predict the value of the random number by 
analyzing the transactions, thus compromising the security of the system. To address this problem, several techniques have been proposed, such as using a random number generator on 
the blockchain, or using multiple random number generators to generate random numbers to ensure that the random numbers generated are truly random. 

### 5、如何⽤solidity实现随机数⽣成器？ 
### How to implement a random number generator with solidity? 

在这个⽰例中，我们使⽤了Solidity中的keccak256哈希函数来⽣成随机数。我们将当前时间戳、发送⽅地址和nonce值作为输⼊，然后对它们进⾏哈希运算，得到⼀个随机数。nonce值是⼀个递增的
计数器，⽤于确保每次调⽤random函数时都会⽣成⼀个新的随机数。请注意，这种⽅法并不是完全随机的，因为它仍然依赖于输⼊参数的值。但是，它可以提供⾜够的随机性，以满⾜⼤多数应⽤程序的
需求。
In this example, we have used the keccak256 hash function in Solidity to generate random numbers. We take as input the current timestamp, the sender address, and the nonce value, and 
then hash them to get a random number. The nonce value is an incremental counter that is used to make sure that a new random number is generated each time the random function is called. 
Note that this method is not completely random, since it still depends on the value of the input argument. However, it can provide enough randomness to meet the needs of most applications. 

### 6、什么是EIP-1559？ 
### What is EIP-1559? 

Ethereum Improvement Proposal (EIP) 1559是以太坊的⼀个升级，旨在改变以太坊计算和处理⽹络交易费⽤（称为“gas费⽤”）的⽅式。该升级通过使⽤基于区块的基础费⽤和发送⽅指定的最⼤
费⽤，⽽不是对gas价格进⾏竞价，来更加平衡地激励矿⼯在⾼或低⽹络拥塞期间进⾏挖矿，从⽽使以太坊交易更加⾼效。EIP-1559是⼀个提案，它改变了gas费⽤的结构和矿⼯的奖励⽅式。该提案于2021
年8⽉5⽇作为以太坊伦敦硬分叉的⼀部分实施。gas费 = 基础费（Basefee）+ 矿⼯费（Tip）。基础费会根据区块的Gas利⽤率动态调整，如每个区块的Gas费利⽤率低于50%，就降低⼿续费，⾼于50%，
就提⾼⼿续费。
Ethereum Improvement Proposal (EIP) 1559 is an upgrade to Ether that aims to change the way Ether calculates and processes network transaction fees (called "gas fees"). The upgrade 
makes Ether transactions more efficient by using a block-based base fee and a sender-specified maximum fee, rather than bidding on the price of the gas, to provide a more balanced incentive 
for miners to mine during periods of high or low network congestion. EIP-1559 is a proposal to change the way the gas fee is structured and how miners are rewarded. The proposal was 
implemented on August 5, 2021 as part of the London Hard Fork of Ether. gas fee = base fee (Basefee) + miner fee (Tip). The base fee will be dynamically adjusted based on the Gas 
utilization of the blocks, such that if the Gas fee utilization of each block is below 50%, the fee will be reduced, and if it is above 50%, the fee will be increased. 

### 7、ERC20 中的 transfer 和 transferFrom 之间有什么区别？ 
### What is the difference between transfer and transferFrom in ERC20? 

transfer是从当前合约转账给⽬标账⼾，transferFrom是可设置发送账⼾和⽬标账⼾ transfer(address recipient, uint256 amount) 
transferFrom(address sender, address recipient, uint256 amount) Transfer is to transfer money from the current contract to the target account, transferFrom 
is to set the sender account and the target account. transfer(address recipient, uint256 amount) 
transferFrom(address sender, address recipient, uint256 amount) 

### 8、如何向没有payable 函数、receive 或回退的合约发送以太坊？ 
### How to send eth to a contract that has no payable function, receive or fallback? 

①通过其他合约⾃毁将⾃毁的eth发送⽬标合约 
②将挖矿获取的区块奖励费⽤的接收者设置为⽬标合约地址
③将Beacon信标链上质押后的提现地址设置为⽬标合约地址 
这⾥补充⼀个：在solidity0.4之前的合约，也可以直接接收ETH。 
①Send the self-destructed eth to the target contract by self-destructing another contract. 
②Set the recipient of the block reward fee from mining to the target contract address. 
③Set the address of the Beacon chain that will be used for withdrawals after pledging as 
the target contract's address. Here is one more thing: the contracts before solidity0.4 can also receive ETH directly. 

### 9、代理合约中的存储冲突是什么？ 
### What are storage conflicts in proxy contracts? 

存储冲突是指多个合约尝试访问同⼀存储位置时发⽣的问题。当多个合约同时尝试更新同⼀存储位置时，可能会发⽣存储冲突，导致数据不⼀致或合约⽆法正常⼯作。为了避免这种情况，使⽤代理
合约来管理存储位置，并确保只有⼀个合约可以访问每个存储位置。代理合约可以充当存储位置的所有者，并使⽤访问控制机制来限制对存储位置的访问。这可以帮助确保数据的⼀致性，并提⾼智能合
约的安全性和可靠性。
A storage conflict is a problem that occurs when multiple contracts try to access the same storage location. When multiple contracts try to update the same storage location at the same 
time, a storage conflict may occur, resulting in inconsistent data or a contract that does not work properly. To avoid this, use proxy contracts to manage storage locations and ensure that only 
one contract can access each storage location. A proxy contract can act as the owner of a storage location and use access control mechanisms to restrict access to the storage location. This can 
help ensure data consistency and improve the security and reliability of smart contracts. 

### 10、abi.encode 和 abi.encodePacked 之间有什么区别？ 
### What is the difference between abi.encode and abi.encodePacked? 

abi.encode会在每个参数之间添加填充，以确保每个参数占⽤32个字节。这可以确保编码后的字节数组具有固定的⻓度，并且可以正确地解码回原始参数。但是，由于填充的存在，编码后的字节数
组可能会⽐实际需要的更⼤。
abi.encodePacked不会添加填充，⽽是将所有参数拼接在⼀起。这可以确保编码后的字节数组尽可能⼩，但是可能会导致解码时出现问题，因为⽆法确定每个参数的确切位置。
Abi.encode adds padding between each parameter to ensure that each parameter takes up 32 bytes. This ensures that the encoded byte array has a fixed length and can be correctly 
decoded back to the original parameters. However, due to padding, the encoded byte array may be larger than it actually needs to be. 
Instead of adding padding, abi.encodePacked stitches all the parameters together. This ensures that the encoded byte array is as small as possible, but may lead to problems when 
decoding because it is not possible to determine the exact position of each parameter. Instead of adding padding, abi.encodePacked stitches all the parameters together. This 
ensures that the encoded byte array is as small as possible, but may lead to problems when decoding because it is not possible to determine the exact position of each parameter. 
