## 基础知识类

### 1.difference between private, internal, public and external functions? 
### 私有、内部、公共和外部函数之间的区别？  

Private can only be used within the current contract, subcontract inheritance can not be called; internal can be called by the current contract and subcontracts; public contract internal 
and external and subcontracts can be called; external only provided to the external call, the contract can not be called within the contract, the contract interface functions should be 
declared as external.   
私有只能当前合约内部使⽤，⼦合约继承也不能调⽤；内部可以当前合约和⼦合约调⽤；公共合约内部外部和⼦合约都可以调⽤；外部只提供给外部调⽤，合约内部不能调⽤，合约接⼝的函数都要声明成external。 

### 2.Approximately how big can the smart contract size be? 
### 智能合约⼤⼩⼤约可以有多⼤?   

After Shanghai upgraded, the realisation of EIP-3860 is 48KB, the original 24KB, expanding the amount of code to solve the problem of high gas fees between contracts calling each other 
after splitting the previous complex contract.   
上海升级后，实现EIP-3860是48KB，原来24KB，扩⼤代码量解决以前复杂合约拆分后合约之间互相调⽤的⾼gas费问题。 

### 3.What is the difference between create and create2? 
### create 和 create2 之间有什么区别？ 

CREATE and CREATE2 are two important opcodes in Solidity that can be used to deploy smart contracts, but there are some differences between them. The CREATE opcode calculates 
the address of a new contract by hashing the sender's address and the nonce value, while the CREATE2 opcode uses a more complex formula that includes parameters such as the sender's 
address, a random number, and a bytecode to calculate the address of a new contract. The main advantage of the CREATE2 opcode is that it can predict the address of a contract before it is 
deployed, which is useful in scenarios where the address of the contract is known in advance.    
CREATE和CREATE2是Solidity中的两个重要的操作码，它们都可以⽤于部署智能合约，但是它们之间有⼀些区别. CREATE操作码通过对发送者地址和nonce值进⾏哈希运算来计算新合约的地址，⽽
CREATE2操作码则使⽤了更复杂的公式来计算新合约的地址，这个公式包括发送者地址、随机数、字节码等参数. CREATE2操作码的主要优点是，它可以在部署合约之前预测合约的地址，这对于需要提前
知道合约地址的场景⾮常有⽤。

### 4.What are the major changes to arithmetic operations in Solidity version 0.8.0? 
### Solidity 0.8.0 版本对算术运算有什么重⼤变化？ 
As of Solidity version 0.8.0, arithmetic operations are rolled back on underflow and overflow. This means that if the result of an arithmetic operation is out of range for its datatype, 
the operation will fail and roll back. Prior to Solidity 0.8.0, integer underflows and overflows were allowed and did not result in an error. To avoid this, Solidity 0.8.0 turned on arithmetic operation checking by default, and if you need to use the old way of doing arithmetic operations, you can use the unchecked{...} statement block 
从Solidity 0.8.0版本开始，算术运算会在下溢和上溢时回滚。这意味着，如果⼀个算术运算的结果超出了其数据类型的范围，那么这个运算将会失败并回滚。在Solidity 0.8.0之前，整数下溢和上溢
是允许的，不会导致错误，为了避免这种情况，Solidity 0.8.0默认开启了算术运算检查，如果您需要使⽤旧的算术运算⽅式，可以使⽤unchecked{R}语句块。 

### 5.Is it better to use a map or an array for an address allowlist? Why? 
### 对于地址 allowlist，使⽤映射还是数组更好？为什么？ 

An address allowlist is a list of stored addresses that restricts certain operations to only those addresses in the list. In Solidity, an address allowlist can be implemented as either a map 
or an array, with the advantage of using a map to quickly find out if an address is in the allowlist, and the advantage of using an array to more easily traverse the entire allowlist. 
If you need to quickly find out if an address is in an allowlist, then it may be better to use a mapping. Mapping is implemented using a hash table and can find out if an address is in an 
allowlist in O(1) time. This is especially useful for large allowlists, as it avoids having to traverse the entire list to find an address. 
If you need to traverse the entire allowlist, it may be better to use an array. Arrays make it easier to traverse the entire list because they are contiguous chunks of memory. This is 
especially useful for small allowlists, as it avoids the extra overhead of using a hash table. 
地址 allowlist 是⼀个存储地址的列表，⽤于限制只有在列表中的地址才能执⾏某些操作。在 Solidity 中，可以使⽤映射或数组来实现地址 allowlist。使⽤映射的优点是可以快速查找地址是否在 
allowlist 中，⽽使⽤数组的优点是可以更容易地遍历整个 allowlist。 如果您需要快速查找地址是否在 allowlist 中，那么使⽤映射可能更好。映射使⽤哈希表实现，可
以在 O(1) 的时间内查找地址是否在 allowlist 中。这对于⼤型 allowlist 尤其有⽤，因为它可以避免遍历整个列表来查找地址。
如果您需要遍历整个 allowlist，那么使⽤数组可能更好。数组可以更容易地遍历整个列表，因为它们是连续的内存块。这对于⼩型 allowlist 尤其有⽤，因为它可以避免使⽤哈希表的额外开销。 

### 6.What are the main hash functions used in Ether? 
### 以太坊主要使⽤什么哈希函数？

以太坊主要使⽤两种哈希函数，分别是 Keccak-256 和 SHA-3。Keccak-256 是以太坊最初采⽤的哈希函数，它是 SHA-3 标准的⼀个变种。在以太坊中，Keccak-256 ⽤于计算地址、交易哈希和区块哈
希等重要数据的哈希值。SHA-3 是⼀种新的哈希函数标准，它⽐ Keccak-256 更加安全和⾼效。在以太坊中，SHA-3 ⽤于计算合约代码的哈希值。 
There are two main hash functions used in Ether, Keccak-256 and SHA-3. Keccak-256 is the original hash function used in Ether, and it is a variant of the SHA-3 standard. In Ether, Keccak-
256 is used to calculate hashes of important data such as addresses, transaction hashes, and block hashes.SHA-3 is a new hash function standard that is more secure and efficient than 
Keccak-256. In Ether, SHA-3 is used to calculate hashes of contract codes. 

### 7、1 个Ether 相当于 多少个 gwei ？ 
### How many gwei does 1 ether equal? 

1 ether = 10^9 Gwei 

### 8、1 个Ether 相当于 多少个 wei ？ 
### How many wei are equivalent to 1 Ether? 

1 ether = 10^18 Wei 
9、assert 和 require 之间有什么区别？ 

### What is the difference between assert and require? 

在 Solidity 中，assert 和 require 都是⽤于检查条件的函数。当条件不满⾜时，它们会抛出错误或异常。它们之间的区别在于，require ⽤于在执⾏前验证输⼊和条件，⽽ assert ⽤于检查不应该为假的
代码，失败的断⾔可能意味着代码层⾯存在错误。当我们想在执⾏之前验证输⼊和条件时，我们使⽤ require。当我们想要检查可以⽽且永远不应该为 false 的代码时，我们使⽤ assert。如果失败，则意
味着存在错误，在这种情况下，我们应该使⽤ assert。 
In Solidity, both assert and require are functions that are used to check conditions. They throw errors or exceptions when conditions are not met. The difference between them is that 
require is used to validate inputs and conditions before execution, while assert is used to check for code that shouldn't be false, and failed assertions can mean that there is an error at the code 
level. We use require when we want to validate inputs and conditions before execution, and assert when we want to check for code that can and should never be false, and if it fails, it means 
there is an error, in which case we should use assert. 

### 10、当在选择 assert 和 require 之间做出决定时，需要考虑的⽅⾯： 
### Aspects to consider when deciding between assert and require: 

Gas 优化的效率：如果 assert 返回⼀个 false 语句，它会编译为 0xfe，这是⼀个⽆效的操作码，它会⽤完所有剩余的 gas，从⽽完全恢复更改。如果 require 返回错误语句，它会编译为 0xfd，这是 
REVERT 的操作码，这意味着它将返回剩余的 gas。 
Bytecode 分析：如果你使⽤ assert，你的代码将会更⼩，因为它不会返回任何东西。如果你使⽤ 
require，你的代码将会更⼤，因为它需要返回错误信息。 
Efficiency of Gas Optimization: If assert returns a false statement, it will compile to 0xfe, which is an invalid opcode, and it will use up all the remaining gas to fully revert the change. If require 
returns an error statement, it compiles to 0xfd, which is the opcode for REVERT, meaning it will return the remaining gas. 
Bytecode analysis: If you use assert, your code will be smaller because it won't return anything. If you use require, your code will be bigger because it needs to return error messages. 

### 11、什么是检查效果（ check-effects ）模式？ 
### What is check-effects pattern? 

检查效果（check-effects）模式是⼀种编程模式，⽤于确保函数在执⾏时不会对状态造成意外的影响。在这种模式下，函数应该只读取输⼊参数，并且不应该修改任何状态。如果函数需要修改状
态，它应该返回⼀个包含所有修改的对象，⽽不是直接修改状态。这种模式可以帮助开发⼈员编写更安全、更可靠的代码，因为它可以减少由于状态修改⽽导致的错误。  
Check-effects mode is a programming pattern that ensures that functions are executed without unintended effects on state. In this mode, the function should only read the input 
parameters and should not modify any state. If the function needs to modify the state, it should return an object containing all the modifications instead of modifying the state directly. This pattern helps developers write safer and more reliable code because it reduces the number of errors due to state modifications.

```
Checks-Effects-Interaction - 保证状态完整，再做外部调用

该设计模式是编码风格约束，可有效避免重放攻击。通常情况下，一个函数可能包含三个部分：

Checks：参数验证
Effects：修改合约状态
Interaction：外部交互

这个模式要求合约按照Checks-Effects-Interaction的顺序来组织代码。它的好处在于进行外部调用之前，Checks-Effects已完成合约自身状态所有相关工作，使得状态完整、逻辑自洽，这样外部调用就无法利用不完整的状态进行攻击了。
```

### 12、运⾏独⽴验证节点所需的最⼩以太数量是多少？ 
### What is the minimum amount of Ether required to run a standalone validation node? 

在以太坊⽹络中，要运⾏⼀个独⽴的验证节点，需要⾄少 32 个以太币作为抵押品。这是因为在以太坊的 Proof of Stake（PoS）共识机制中，验证节点需要抵押⼀定数量的以太币来证明他们对⽹络的
贡献，同时也会获得⼀定的奖励. 
To run a standalone validation node in the Ethernet network, a minimum of 32 Ether is required as collateral. This is because in Ethernet's Proof of Stake (PoS) consensus mechanism, 
validation nodes need to collateralize a certain amount of Ether to prove their contribution to the network, and also receive a certain amount of rewards. 

### 13、fallback 和 receive 之间有什么区别？ 
### What is the difference between fallback and receive? 

fallback和receive都是特殊的回调函数，⽤于处理合约中不存在的函数调⽤和接收以太币。它们之间的区别在于，fallback函数会在调⽤合约不存在的函数时被触发，⽽receive函数只⽤于处理接收
以太币。
Both fallback and receive are special callback functions for handling function calls and receiving Ether that do not exist in the contract. The difference between them is that the fallback 
function is triggered when a function that does not exist in the contract is called, while the receive function is only used to handle receiving Ether. 

### 14、上海升级后，每个区块的 gas 限制是多少？  
### What is the gas limit per block after Shanghai upgrade? 

在以太坊上，每个区块的 gas limit 是由矿⼯决定的。在上海升级后，每个区块的 gas limit 已经从 15,000,000 增加到了 30,000,000。 
On Ether, the gas limit of each block is determined by the miners. After the Shanghai upgrade, the gas limit per block has been increased from 15,000,000 to 30,000,000. 

### 15、tx.origin 和 msg.sender 之间有什么区别？ 
### What is the difference between tx.origin and msg.sender? 

tx.orgin是这笔交易的发起者，msg.sender是前⼀个调⽤发起者，如交易流程：张三发起交易-合约A-合约B，在合约B中msg.sender = 合约A，tx.orgin = 张三。 
Tx.origin is the originator of the transaction and msg.sender is the previous call originator, e.g. transaction flow: Zhang San initiates the transaction - Contract A - Contract B, in Contract B 
msg.sender = Contract A , and tx.origin = the database. 

### 16、view 和 pure 之间有什么区别？ 
### What is the difference between view and pure? 

view和pure都是函数的修饰符，view可以访问合约中的状态变量，不能修改；pure不能访问也不能修改状态变量。
Both view and pure are modifiers of functions. View can access state variables in a contract and cannot modify them; pure cannot access or modify state variables. 

### 17、ERC721 中的 transfer From 和 safe Transfer From 之间有什么区别？ 
### What is the difference between transfer From and safe Transfer From in ERC721? 

Transfer From和safe Transfer From都是ERC721合约中的函数，⽤于将代币从⼀个地址转移到另⼀个地址。它们之间的区别在于，safe Transfer From函数会检查接收合约是否实现了on ERC721 
Received函数，并且该函数返回的值是否为特定的魔数。如果接收合约没有实现on ERC721 Received函数或者返回的值不是特定的魔数，safe Transfer From函数将抛出异常并回滚所有更改，以确保代币
不会永久丢失。⽽transfer From函数则不会进⾏这种检查，因此可能会导致代币永久丢失。 
Both transfer From and safe Transfer From are functions in the ERC721 contract that are used to transfer tokens from one address to another. The difference between them is that the 
safe Transfer From function checks if the receiving contract implements the on ERC721 Received function and if the value returned by the function is a specific magic number. If the receiving 
contract does not implement the on ERC721 Received function or returns a value that is not a specific magic number, the safe Transfer From function throws an exception and rolls back all 
changes to ensure that the tokens are not permanently lost. The transfer From function, on the other hand, does not perform this check and therefore may result in the permanent loss of 
tokens. 

### 18、如何将 ERC1155 代币转换为⾮同质化代币？ 
### How to convert ERC1155 tokens to non-homogenized tokens? 

ERC1155代币是⼀种可替代和不可替代的代币，可以在同⼀合约中管理多个资产。ERC721代币是⼀种⾮可替代代币，每个代币都是唯⼀的。因此，将ERC1155代币转换为ERC721代币需要将每个
ERC1155代币转换为⼀个唯⼀的ERC721代币。  
要将ERC1155代币转换为ERC721代币，您需要执⾏以下步骤：  
  1.创建⼀个新的ERC721合约。 
  2.为每个ERC1155代币创建⼀个新的ERC721代币。 
  3.将ERC1155代币的所有权转移到新的ERC721代币。 
  4.销毁原始ERC1155代币。 
ERC1155 tokens are fungible and non-fungible tokens that can be used to manage multiple assets in the same contract. ERC721 tokens are non-fungible tokens and each token is unique. 
Therefore, converting ERC1155 tokens to ERC721 tokens requires converting each ERC1155 token to a unique ERC721 token.  
To convert ERC1155 tokens to ERC721 tokens, you need to perform the following steps:  
  1. Create a new ERC721 contract. 
  2. Create a new ERC721 token for each ERC1155 token. 
  3. Transfer ownership of the ERC1155 tokens to the new ERC721 tokens.
  4. Destroy the original ERC1155 tokens. 

### 19、修饰符（modifier）的作⽤是什么？ 
### What is the purpose of a modifier? 

modifier是⼀种函数修饰符，⽤于声明⼀个函数修改器。函数修改器的作⽤与Spring中的切⾯功能很相似，当它作⽤于⼀个函数上，可以在函数执⾏前或后（依赖于具体实现）预先执⾏modifier中
的逻辑，以增强其功能。使⽤modifier可以将⼀些通⽤的操作提取出来，提⾼编码效率，降低代码耦合性。  
modifier可以⽤于控制函数的访问权限和⾏为。例如，如果您希望只有合约的所有者才能访问某个函数，您可以将该函数标记为private，并使⽤modifier来检查调⽤者是否为合约的所有者。如果调
⽤者不是合约的所有者，则函数将停⽌执⾏并回滚所有更改。这可以帮助保护合约免受未经授权的访问和攻击。  
A modifier is a function modifier used to declare a function modifier. The role of function modifiers is very similar to that of Spring's faceted functionality. When it is applied to a function, 
the logic in the modifier can be pre-executed before or after the function is executed (depending on the specific implementation) to enhance its functionality. The use of modifier can be 
extracted out of some common operations to improve coding efficiency and reduce code coupling. 
Modifiers can be used to control the access rights and behavior of functions. For example, if you want only the owner of a contract to have access to a function, you can mark the function as 
private and use a modifier to check if the caller is the owner of the contract. If the caller is not the owner of the contract, the function will stop executing and roll back all changes. This can 
help protect the contract from unauthorized access and attacks. 

### 20、uint256 可以存储的最⼤值是多少？ 
### What is the maximum value that uint256 can store? 

最⼤2^256-1 。为什么减1？0占⽤了⼀个空间，256位的表⽰的数是从000……00到111……11 。例如：如果有uint2 , 那么2^2可存储的数可以是 00 = 0 ，01 = 1, 10 = 2, 11 = 3，这⾥最⼤是3，也就是
2^-1,⽽不是4。 
Maximum 2^256-1. Why minus 1? 0 takes up a space, and the 256-bit representation of the number is from 000……00 to 111……11. For example, if we have uint2, then 2^2 can store 00 = 0, 
01 = 1, 10 = 2, 11 = 3, here the maximum is 3, which is 2^-1, not 4. 

### 21、什么是浮动利率和固定利率？ 
### What are variable and fixed interest rates? 

固定利率是指在贷款期间内，贷款利率保持不变。⽽浮动利率是指贷款利率会随着市场利率的变化⽽变化。浮动利率通常基于某个基准利率，例如央⾏基准利率或LIBOR（伦敦银⾏间同业拆借利
率）。如果基准利率上升，贷款利率也会上升，反之亦然。send函数的gas限制也为2300 gas，但它返回⼀个布尔值，指⽰转账是否成功。如果转账失败，它将返回false。但是，如果接收⽅合约没有实现fallback函数，或者fallback函数消耗的gas超过了2300，那么转账将失败并回滚所有更改。  
A fixed interest rate means that the interest rate on the loan remains the same for the duration of the loan. A floating rate is when the interest rate on the loan changes in response to 
changes in market interest rates. Floating interest rates are usually based on a benchmark rate, such as the central bank's prime rate or LIBOR (London Interbank Offered Rate). If the prime rate rises, the lending rate will also rise, and vice versa. The send function also has a gas limit of 2300 gas, but it returns a boolean indicating whether the transfer was successful. If the transfer fails, it returns false, but if the recipient contract does not implement a fallback function, or if the fallback function consumes more than 2300 gas, then the transfer fails and all changes are rolled back. 

### 22、transfer 和 send 之间有什么区别？为什么不应该使⽤它们？ 
### What is the difference between transfer and send? Why should they not be used? 

transfer函数的gas限制为2300 gas，这意味着如果接收⽅合约没有实现fallback函数，或者fallback函数消耗的gas超过了2300，那么转账将失败并回滚所有更改。这可以防⽌重⼊攻击，但也可
能导致⼀些问题，例如⽆法向某些合约发送以太币。send函数的gas限制也为2300 gas，但它返回⼀个布尔值，指⽰转账是否成功。如果转账失败，它将返回false。但是，如果接收⽅合约没有实现fallback
函数，或者fallback函数消耗的gas超过了2300，那么转账将失败并回滚所有更改。 由于这些限制，transfer和send函数已经被认为是不安全的，因此不应该使⽤它们。相反，您应
该使⽤call函数来转移以太币。call函数没有gas限制，可以向任何地址发送以太币，并且可以指定要发送的gas数量。但是，您应该⼩⼼使⽤call函数，因为它可能会导致⼀些安全问题，例如重⼊攻击。 
The transfer function has a gas limit of 2300 gas, which means that if the recipient contract does not implement a fallback function, or if the fallback function consumes more than 2300 
gas, the transfer will fail and all changes will be rolled back. This prevents reentry attacks, but can also lead to problems such as not being able to send Ether to some contracts. the send 
function also has a gas limit of 2300 gas, but it returns a boolean indicating whether the transfer was successful or not. If the transfer fails, it returns false. however, if the recipient contract does not implement the fallback function, or if the fallback function consumes more than 2300 gas, the transfer will fail and all changes will be rolled back. 
Due to these limitations, the transfer and send functions have been deemed unsafe and therefore they should not be used. Instead, you should use the call function to transfer Ether. 
call functions do not have a gas limit, can send Ether to any address, and can specify the amount of gas to send. However, you should be careful with the call function as it can lead to some 
security issues, such as reentry attacks.  

### 23、如何在 Solidity 中编写节省gas的⾼效循环？ 
### How to write efficient loops that save gas in Solidity?   

1.避免⽆限循环。 
2.避免重复计算：多次计算相同的值，应该将该值存储在变量中，并在需要时重复使⽤该变量。 
3.避免使⽤昂贵的操作：例如除法和取模。尝试使⽤更便宜的操作来代替它们。 
4.避免使⽤⼤型数组：如果可能的话，您应该使⽤映射或其他数据结构来代替数组。 
5.避免使⽤复杂的嵌套循环。 
6.使⽤constant和immutable关键字：如果您需要在循环中使⽤常量或不可变变量，请使⽤constant和immutable关键字来声明它们。 
1. Avoid infinite loops. 
2. Avoid repeated calculations: to calculate the same value multiple times, you should store the value in a variable and reuse the variable when needed. 
3. Avoid using expensive operations: such as division and modulo. Try using cheaper operations instead of them. 
4. Avoid large arrays: If possible, you should use maps or other data structures instead of arrays. 
5. Avoid complex nested loops. 
6. Use the constant and immutable keywords: if you need to use constants or immutable variables in a loop, declare them using the constant and immutable keywords. 

### 24、uint8、uint32、uint64、uint128、uint256 都是有效的 uint ⼤⼩。还有其他的吗？ 
### uint8, uint32, uint64, uint128, uint256 are all valid uint sizes. Are there others? 

这些类型分别表⽰8位、32位、64位、128位和256位的⽆符号整数。除了这些类型之外，Solidity还⽀持其他整数类型，例如int8、int16、int32、int64、int128和int256。这些类型分别表⽰8位、16
位、32位、64位、128位和256位的有符号整数。 
These types represent 8-bit, 32-bit, 64-bit, 128-bit, and 256-bit unsigned integers, respectively. In addition to these types, Solidity supports other integer types such as int8, int16, 
int32, int64, int128, and int256, which represent 8-bit, 16-bit, 32-bit, 64-bit, 128-bit, and 256-bit signed integers. 

### 25、以太坊如何确定 EIP-1559 中的 BASEFEE？ 
### How does Ethernet determine BASEFEE in EIP-1559? 

在以太坊的EIP-1559协议中，BASEFEE是由以太坊⽹络根据交易需求和区块⼤⼩动态调整的。BASEFEE的计算⽅式是通过⼀个名为“基础费⽤追踪器”的算法来实现的。该算法会根据当前区块的
交易需求和区块⼤⼩来动态调整BASEFEE，以确保交易能够在合理的时间内得到确认。 具体来说，基础费⽤追踪器会根据当前区块的交易需求和区块⼤⼩计算出⼀个⽬标基础费⽤。如
果当前的BASEFEE⾼于⽬标基础费⽤，那么BASEFEE将会下降。如果当前的BASEFEE低于⽬标基础费⽤，那么BASEFEE将会上升。这种动态调整机制可以确保BASEFEE始终能够反映当前的交易需求和区
块⼤⼩，从⽽提⾼以太坊的交易效率和可靠性。
In Ethernet's EIP-1559 protocol, BASEFEE is dynamically adjusted by the Ethernet network based on transaction demand and block size, and is calculated using an algorithm called the "Base Fee 
Tracker". The algorithm dynamically adjusts BASEFEE based on the current block's transaction demand and block size to ensure that transactions are recognized in a reasonable amount of 
time. Specifically, the BASEFEE tracker calculates a target BASEFEE based on the transaction demand and block size of the current block. If the current BASEFEE is higher than the target base fee, then the BASEFEE will decrease. If the current BASEFEE is lower than the target base fee, then the BASEFEE will increase. This dynamic adjustment mechanism ensures that BASEFEE always 
reflects the current transaction demand and block size, thus improving the efficiency and reliability of Ethernet transactions. 

### 26、冷读（cold read）和热读（warm read）之间有什么区别？ 
### What is the difference between cold read and warm read? 

冷读（cold read）和热读（warm read）是两种不同的访问存储变量的⽅式。冷读是指第⼀次读取存储变量时，需要从存储中读取变量的值，这需要较⾼的gas费⽤。⽽热读是指在第⼀次读取存储变
量之后，再次读取存储变量时，可以从缓存中读取变量的值，这需要较低的gas费⽤。热读和冷读是由Ethereum虚拟机（EVM）⾃动处理的。 
Cold read and warm read are two different ways of accessing stored variables. Cold read means that the first time you read a stored variable, you need to read the value of the variable 
from the storage, which requires a higher gas cost. Whereas, warm read means that after reading the storage variable for the first time, when reading the storage variable again, the value of the variable can be read from the cache, which requires a lower gas cost. Warm and cold reads are handled automatically by the Ethereum Virtual Machine (EVM). 

### 27、AMM 如何定价资产？ 
### How does AMM price assets? 

通过恒定乘积算法，a * b = k ,兑换 m个a 需要的b数量算法 = k/(a+m) - b ,这⾥没计算⼿续费。 
Through the constant product algorithm, a * b = k ,the number of b's needed to convert m a's algorithmically = k/(a+m) - b ,no commission is calculated here. 

### 28、接⼝中有效的函数修饰符有哪些？ 
### What are the valid function modifiers in an interface? 

在Solidity中，接⼝是⼀种抽象合约，它定义了合约应该实现的函数。接⼝中的函数没有实现，只有函数签名。函数签名包括函数名称、参数类型和返回类型。接⼝中的函数可以使⽤以下修饰符：
  external：指定函数只能从合约外部调⽤。 
  view：指定函数不会修改合约状态。 
  pure：指定函数既不会修改合约状态，也不会读取合约状态。 
  payable：指定函数可以接受以太币作为⽀付。 
In Solidity, an interface is an abstract contract that defines the functions that the contract should implement. Functions in an interface have no implementation, only a function signature. 
The function signature consists of the function name, parameter type and return type. Functions in interfaces can use the following modifiers:
 external: specifies that the function can only be called from outside the contract. 
 view: Specifies that the function does not modify the state of the contract. 
 pure: specifies that the function will neither modify nor read the contract state. 
 payable: Specifies that the function can accept Ether as payment. 

### 29、根据 Solidity ⻛格指南，函数应该如何排序? 
### How should functions be sorted according to the Solidity style guide? 

函数应根据其可⻅性和顺序进⾏分组：
  构造函数 
  receive 函数（如果存在） 
  fallback 函数（如果存在） 
  外部函数(external) 
  公共函数(public) 
  内部函数(internal) 
  私有函数(private) 
在⼀个分组中，把 view 和 pure 函数放在最后。 
Functions should be grouped according to their visibility and order: 
 constructor 
 receive function (if it exists) 
 fallback function (if present) 
 external function (if present) 
 public function (public) 
 internal function (internal) 
 private function (private) 
In a grouping, put the view and pure functions last. 

### 30、根据 Solidity ⻛格指南，函数修饰符应该如何排序？ 
### How should function modifiers be ordered according to the Solidity style guide? 

函数修改器的顺序应该是: 
  可⻅性（Visibility） 
  可变性（Mutability） 
  虚拟（Virtual） 
  重载（Override） 
  ⾃定义修改器（Custom modifiers） 
The order of function modifiers should be. 
  Visibility 
  Mutability 
  Virtual 
  Override 
Custom modifiers (Custom modifiers) 

### 31、Solidity 提供哪些关键字来测量时间？ 
### What keywords does Solidity provide to measure time? 

now：返回当前区块的时间戳（以秒为单位）。 
时间单位：Solidity提供了⼏个时间单位，包括seconds、minutes、hours、days、weeks和years。这些单位可以与数字⼀起使⽤，例如5 minutes或1 hours。 
block.timestamp：与now关键字类似，返回当前区块的时间戳。 
block.number：返回当前区块的块号。 
  
now: returns the timestamp (in seconds) of the current block. 
time units: Solidity provides several units of time, including seconds, minutes, hours, days, weeks and years. These units can be used with numbers, such as 5 minutes or 1 hour. 
block.timestamp: similar to the now keyword, returns the timestamp of the current block. 
block.number: Returns the block number of the current block. 

### 32、多⼤ uint 可以与⼀个地址在⼀个槽中? 
### How big a uint can be in a slot with an address? 

⼀个地址是20个字节（160位），⽽⼀个uint256是32个字节（256位）, 256-160 = 96。uint96及以下的都可以。
An address is 20 bytes (160 bits) and a uint256 is 32 bytes (256 bits), 256-160 = 96. uint96 and below are fine. 
用户私钥签名信息证明⾝份，应⽤于Sovrin、uPort。 
2.数字证书和区块链 
原理是认证机构颁发数字证书，哈希值记录在区块链上。通过区块链确认证书真实性，应⽤于Blockcerts。 
3.零知识证明（ZKP） 
原理是⽤⼾⽣成零知识证明，证明⾝份属性。验证者验证证明有效性⽽不获取具体⾝份信息，应⽤于Zcash、zk-SNARKs。 
4.代币和访问控制 
原理是使⽤代币或智能合约控制资源访问。⽤⼾通过⾝份验证后获得代币，使⽤代币访问资源，应⽤于ERC-725、ERC-735。  
1.Decentralized Identity (DID) 
The principle is that the user generates a public-private key pair to create a unique DID, which is registered on blockchain. Users use the private key to sign information to prove their 
identity, applied to Sovrin, uPort. 
2.Digital certificate and blockchain 
The principle is that the certification authority issues a digital certificate and the hash value is recorded on blockchain. The authenticity of the certificate is confirmed through the 
blockchain, which is used in Blockcerts. 
3.Zero Knowledge Proof (ZKP) 
The principle is that the user generates zero-knowledge proof to prove identity attributes. The verifier verifies the validity of the proof without obtaining specific identity information, 
applied to Zcash, zk-SNARKs. 
4.Token and Access Control 
The principle is to use tokens or smart contracts to control access to resources. Users obtain tokens after authentication and use them to access resources, applied in ERC-725, ERC-735. 


