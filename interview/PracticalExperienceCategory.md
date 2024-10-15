### 1、什么是签名重放攻击？ 
### What is Signature Replay Attack? 

签名重放攻击是⼀种⽹络攻击，攻击者通过重复使⽤已经被验证的数字签名来欺骗系统。数字签名是⼀种⽤于验证数据完整性和⾝份验证的技术，它使⽤公钥密码学来⽣成和验证签名。在签名重放
攻击中，攻击者截获了⼀个数字签名并将其重复使⽤，以便在未经授权的情况下执⾏某些操作。例如，攻击者可以使⽤重放攻击来多次执⾏某个交易，从⽽导致资⾦损失。为了防⽌签名重放攻击，您可以使⽤以下⽅法：
  使⽤时间戳或随机数来确保每个数字签名只能使⽤⼀次。 
  使⽤序列号来确保数字签名按顺序使⽤。 
  使⽤加密协议来保护数字签名，例如TLS或SSL。 
A signature replay attack is a network attack in which an attacker spoofs a system by reusing digital signatures that have already been verified. Digital signature is a technique used to verify 
data integrity and authentication, which uses public key cryptography to generate and verify signatures. In a signature replay attack, an attacker intercepts a digital signature and reuses it to 
perform certain actions without authorization. For example, an attacker could use a replay attack to execute a transaction multiple times, resulting in a loss of funds. 
To prevent signature replay attacks, you can use the following methods: 
 Use timestamps or random numbers to ensure that each digital signature can be used only once. 
 Use serial numbers to ensure that digital signatures are used sequentially. 
 Use a cryptographic protocol to protect digital signatures, such as TLS or SSL. 

### 2、描述三种存储 gas 成本类型。 
### Describe the three storage gas cost types. 

  1.内存变量：内存变量是指在函数执⾏期间分配的临时变量。它们的gas成本取决于它们的⼤⼩，通常⽐存储变量更便宜。在函数执⾏结束后，内存变量将被销毁。
  2.存储变量：存储变量是指在合约存储器中永久存储的变量。它们的gas成本取决于它们的⼤⼩，通常⽐内存变量更昂贵。存储变量的gas成本还取决于它们的位置，例如，如果它们位于映射中，则访
问映射中的元素的成本更⾼。
  3.状态变量：状态变量是指在合约存储器中永久存储的变量，但它们是在合约创建时定义的。与存储变量相⽐，它们的gas成本更便宜，因为它们只需要在合约创建时初始化⼀次。 

 1.Memory variables: Memory variables are temporary variables allocated during function execution. Their gas cost depends on their size and are usually cheaper than memory variables. 
At the end of function execution, memory variables are destroyed. 
 2.Stored variables: Stored variables are variables that are permanently stored in contract memory. Their gas cost depends on their size and is usually more expensive than memory 
variables. The gas cost of stored variables also depends on their location, e.g., if they are located in a mapping, it is more expensive to access the elements of the mapping. 
 3.State variables: State variables are variables that are permanently stored in the contract memory, but they are defined when the contract is created. They have a cheaper gas cost 
compared to stored variables because they only need to be initialized once at contract creation time. 

### 3、为什么构造函数不应该使⽤在可升级合约⾥？ 
### Why should constructors not be used in scalable contracts? 

使⽤构造函数来初始化合约状态变量是⼀种常⻅的做法，但是这会导致合约的状态变量⽆法升级。因为在升级合约时，新的合约代码将会被部署到⼀个新的地址，⽽旧的合约状态变量将⽆法被传
递到新的合约中。因此，为了使合约状态变量能够升级，应该使⽤初始化函数来初始化合约状态变量，⽽不是构造函数。初始化函数可以在合约部署后随时调⽤，因此可以在升级合约时重新初始化合
约状态变量。
It is common practice to use constructors to initialize contract state variables, but this makes it impossible to upgrade a contract's state variables. This is because when upgrading a 
contract, the new contract code will be deployed to a new address and the old contract state variables will not be able to be passed to the new contract. Therefore, in order to enable 
contract state variables to be upgraded, an initialization function should be used to initialize the contract state variables instead of a constructor. The initialization function can be called 
anytime after the contract has been deployed, so it is possible to reinitialize the contract state variables when upgrading the contract. 

### 4、如果合约通过 delegate call 调⽤⼀个空地址或之前已⾃毁的合约，会发⽣什么？如果是常规调⽤⽽不是 delegatecall 会发⽣什么? 
### What happens if a contract calls a null address or a previously self-destructed contract via delegatecall? What happens if it is a regular call instead of a delegate call? 

如果合约通过delegatecall调⽤⼀个空地址或之前已⾃毁的合约，会导致delegatecall返回false，并且不会发⽣任何状态更改。如果是常规调⽤⽽不是delegatecall，则会导致交易失败并回滚，因为您
不能调⽤⼀个不存在的合约或已⾃毁的合约。
If a contract calls a null address or a previously self-destructed contract via delegatecall, this causes delegatecall to return false and no state change occurs. A regular call instead of a 
delegate call will cause the transaction to fail and be rolled back because you cannot call a non-existent or self-destructing contract. 

### 5、如果向⼀个会回滚的函数进⾏ delegate call，delegate call 会怎么做？ 
### What does a delegate call do if it is made to a function that rolls back? 

如果向⼀个会回滚的函数进⾏ delegate call，delegate call 会返回 false，并且不会发⽣任何状态更改。如果是常规调⽤⽽不是 delegate call，则会导致交易失败并回滚，因为您不能调⽤⼀个不存在
的合约或已⾃毁的合约。
If you make a delegate call to a function that rolls back, the delegate call returns false and no state changes occur. If it's a regular call instead of a delegate call, the transaction 
fails and rolls back because you can't call a non-existing contract or a self-destructing contract. 

### 6、乘以或除以⼆的倍数的 gas ⾼效替代⽅法是什么？ 
### What is the gas efficient alternative to multiplying or dividing by a multiple of two? 

在Solidity中，将⼀个数乘以或除以2的倍数可以通过移位运算来实现，这⽐使⽤乘法或除法运算更⾼效。具体来说，将⼀个数左移n位相当于将它乘以2的n次⽅，⽽将⼀个数右移n位相当于将它除以
2的n次⽅。例如，将⼀个数除以8可以通过将它右移3位来实现，⽽将⼀个数乘以16可以通过将它左移4位来实现。在Solidity 0.8.3及更⾼版本中，编译器会⾃动将乘法和除法运算转换为移位运算，从⽽提
⾼代码的效率。
In Solidity, multiplying or dividing a number by a multiple of two can be accomplished by shifting operations, which is more efficient than using multiply or divide operations. Specifically, 
shifting a number n places to the left is equivalent to multiplying it by 2 to the nth power, while shifting a number n places to the right is equivalent to dividing it by 2 to the nth power. For 
example, dividing a number by 8 can be accomplished by shifting it 3 places to the right, while multiplying a number by 16 can be accomplished by shifting it 4 places to the left. In Solidity 
0.8.3 and higher, the compiler automatically converts multiplication and division operations to shifts, thus improving the efficiency of the code. 

### 7、哪些操作会部分退还 gas？ 
### What operations partially refund gas? 

STORE 操作：如果将⼀个存储槽从⾮零值更改为零值，则会退还⼀部分gas。 
SLOAD 操作：如果从存储中读取⼀个⾮零值，则会退还⼀部分gas。 
CALL 操作：如果调⽤的合约执⾏成功，则会退还⼀部分gas。 
RETURN 操作：如果从函数中返回，则会退还⼀部分gas。 
SELFDESTRUCT 操作：如果⾃毁合约，则会退还其余的gas。 
需要注意的是，这些操作的退还gas的数量是固定的，具体取决于操作的类型和执⾏的环境。 

The STORE operation: a portion of gas is refunded if a storage slot is changed from a non-zero value to a zero value. 
SLOAD operation: if a non-zero value is read from storage, a portion of gas is refunded. 
CALL operation: a portion of gas is refunded if the called contract executes successfully. 
RETURN operation: a portion of gas is refunded if returned from a function. 
SELFDESTRUCT operation: the rest of the gas is refunded if the contract self-destructs. 
Note that the amount of returned gas for these operations is fixed, depending on the type of operation and the environment in which it is executed. 

### 8、如果代理对 A 进⾏ delegate call，⽽ A 执⾏ address(this).balance，返回的是代理的余额还是 A 的余额？
### If the agent makes a delegate call to A and A executes address(this).balance, is the balance returned for the agent or for A?  

返回的是代理的余额。因为在 delegate call 中，调⽤的A会共享代理合约的存储空间，因此 address(this) 将返回代理合约的地址，⽽不是被调⽤合约的地址。 
The balance of the proxy is returned. Because in a delegate call, the calling A shares the storage space of the proxy contract, address(this) will return the address of the proxy contract, 
not the address of the called contract. 

### 9、为什么 Solidity 不⽀持浮点数运算？ 
### Why does Solidity not support floating point arithmetic? 

Solidity不⽀持浮点数运算是因为浮点数运算需要⼤量的计算资源，⽽以太坊虚拟机的计算资源是有限的。此外，浮点数运算可能会导致精度丢失和舍⼊错误，这可能会影响智能合约的正确性和安全
性。为了避免这些问题，Solidity使⽤整数运算来代替浮点数运算。如果您需要进⾏浮点数运算，可以使⽤⼀些库，如FixedPoint.sol，来模拟浮点数运算。这些库使⽤整数运算来实现浮点数运算，从⽽避
免了精度丢失和舍⼊错误的问题。
Solidity does not support floating-point arithmetic because floating-point arithmetic requires a large amount of computational resources, which are limited in the Ethernet virtual 
machine. In addition, floating point arithmetic can lead to precision loss and rounding errors, which can affect the correctness and security of smart contracts. To avoid these problems, 
Solidity uses integer operations instead of floating-point operations. If you need to perform floating point operations, you can use libraries such as Fixed Point.sol to simulate floating point 
operations. These libraries use integer arithmetic to implement floating point operations, thus avoiding precision loss and rounding errors. 
