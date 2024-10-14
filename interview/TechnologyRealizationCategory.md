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

### 11、在权益证明之前后，block.timestamp发⽣了什么变化？
### Whathappenedtoblock.timestampbeforeandafterproofofstake?

在PoW协议中，block.timestamp表⽰矿⼯开始挖掘新块的时间戳。在PoS协议中，block.timestamp表⽰验证器开始验证新块的时间戳。因此，block.timestamp的含义在两种协议中都
与块的创建时间有关，但在PoS协议中，它与验证器的⾏为有关，⽽不是矿⼯的⾏为有关。
InthePoWprotocol,block.timestamprepresentsthetimestampwhentheminerstartsminingthenewblock.InthePoSprotocol,block.timestamprepresentsthetimestampwhenthe
verifierstartsverifyingthenewblock.Thus,themeaningofblock.timestampisrelatedtothecreationtimeoftheblockinbothprotocols,butinthePoSprotocol,itisrelatedtothebehaviorofthevalidator,notthebehavioroftheminer.

### 12、代理中的函数选择器冲突是什么，它是如何发⽣的？
### Whatisafunctionselectorconflictinanagentandhowdoesitoccur?

函数选择器是⼀个⽤于标识函数的哈希值。在Solidity中，当您调⽤⼀个合约中的函数时，您需要提供该函数的选择器。函数选择器由函数名称和参数类型组成，并使⽤Keccak-256哈希算法进⾏哈希
处理。当您在代理合约中调⽤另⼀个合约的函数时，您需要将该函数的选择器传递给代理合约。如果您在代理合约中定义了具有相同名称和参数类型的函数，则会发⽣函数选择器冲突。这意味着当您调
⽤代理合约中的函数时，Solidity⽆法确定您要调⽤哪个函数，因为它们具有相同的函数选择器。为了避免函数选择器冲突，您可以使⽤不同的函数名称或参数类型来定义代理合约中的函数。或者使⽤管
理员校验来调⽤，只有管理员才调⽤代理合约定义的函数。
Afunctionselectorisahashvalueusedtoidentifyafunction.InSolidity,whenyoucallafunctioninacontract,youneedtoprovideaselectorforthatfunction.Thefunction
selectorconsistsofthenameofthefunctionandthetypeoftheargumentandishashedusingtheKeccak256hashalgorithm.Whenyoucallafunctionfromanothercontractinaproxy
contract,youneedtopasstheselectorforthatfunctiontotheproxycontract.Afunctionselectorconflictoccursifyoudefinefunctionswiththesamenameandargumenttypesin
theproxycontract.Thismeansthatwhenyoucallafunctionintheproxycontract,Soliditycannotdeterminewhichfunctionyouwanttocallbecausetheyhavethesamefunction
selector.Toavoidfunctionselectorconflicts,youcandefinefunctionsintheproxycontractwithdifferentfunctionnamesorparametertypes.Oryoucanuseadministrator
checksumstocallthem,sothatonlyadministratorscallthefunctionsdefinedintheproxycontract.

### 13、payable函数对gas的影响是什么？
### Howdoesthepayablefunctionaffectgas?

payable函数是⼀种特殊类型的Solidity函数，它允许合约接受以太币作为⽀付。当您在Solidity中定义⼀个payable函数时，您可以在函数调⽤中包含以太币，并将其存储在合约的余额中。由于以太币
是⼀种有价值的加密货币，因此在调⽤payable函数时需要⽀付⼀定的gas费⽤。这是因为在以太坊⽹络中，每个操作都需要消耗⼀定数量的gas，以保证⽹络的安全性和可靠性。
当您在Solidity中使⽤payable函数时，需要注意以下⼏点：
1.确保您的合约具有⾜够的余额来处理以太币⽀付。
2.确保您的合约具有⾜够的gas来处理以太币⽀付。
3.确保您的合约具有⾜够的安全性来处理以太币⽀付。
ThepayablefunctionisaspecialtypeofSolidityfunctionthatallowscontractstoacceptEtheraspayment.WhenyoudefineapayablefunctioninSolidity,youcanincludeEtherin
thefunctioncallandstoreitinthecontract'sbalance.SinceEtherisavaluablecryptocurrency,thereisagasfeethatneedstobepaidwhencallingapayablefunction.Thisis
becauseintheEthernetwork,eachoperationconsumesacertainamountofgastokeepthenetworksecureandreliable.WhenyouusethepayablefunctioninSolidity,youneedtopay
attentiontothefollowingpoints:
 1.MakesureyourcontracthasenoughbalancetohandleEtherpayments.
 2.MakesureyourcontracthasenoughgastoprocessEtherpayments.
 3.Makesureyourcontracthasenoughsecuritytohandleethereumpayments.

### 14、函数参数中的memory和calldata有什么区别？
### Whatisthedifferencebetweenmemoryandcalldatainfunctionarguments?

memory：⽤于声明函数参数将被存储在内存中。内存中的数据只在函数执⾏期间存在，执⾏完毕后就被销毁。在函数内部，您可以使⽤memory关键字来创建临时变量，但是不能在函数之外使⽤
它们。在函数调⽤期间，函数参数的值将从调⽤⽅复制到内存中，并在函数执⾏完毕后被销毁。
calldata：⽤于声明函数参数将被存储在调⽤数据区域中。调⽤数据区域是⼀个不可修改的区域，⽤于保存函数参数。在函数内部，您可以使⽤calldata关键字来访问函数参数，但是不能在函数之外使
⽤它们。在函数调⽤期间，函数参数的值将从调⽤⽅复制到调⽤数据区域中，并在函数执⾏完毕后被销毁。
Memoryisusedtodeclarethatafunctionparameterwillbestoredinmemory.Thedatainmemoryexistsonlyforthedurationofthefunction'sexecutionandisdestroyedwhen
executioniscomplete.Youcanusethememorykeywordtocreatetemporaryvariablesinsideafunction,butyoucannotusethemoutsidethefunction.Duringafunctioncall,thevaluesofthefunctionargumentsarecopiedfromthecallerintomemoryanddestroyedwhenthefunctionisfinishedexecuting.
Thecalldataisusedtodeclarethatthefunctionparameterswillbestoredinthecalldataarea.Thecalldataareaisanonmodifiableareathatisusedtoholdfunctionparameters.
Youcanusethecalldatakeywordtoaccessfunctionargumentsinsideafunction,butyoucannotusethemoutsidethefunction.Duringafunctioncall,thevaluesofthefunction
parametersarecopiedfromthecallerintothecalldataareaanddestroyedwhenthefunctionisfinishedexecuting.

### 15、UUPS和TransparentUpgradeableProxy模式之间有什么区别？
### WhatisthedifferencebetweenUUPSandTransparentUpgradeableProxymode?

在TransparentUpgradeableProxy模式中，代理合约只负责将所有调⽤转发到实现合约，并将实现合约的地址存储在代理合约的状态变量中。在升级合约时，新的实现合约将被部署到新的地址，然
后将新的实现合约的地址存储在代理合约的状态变量中。这种⽅法的缺点是，每次升级合约时都需要部署⼀个新的代理合约。
相⽐之下，UUPS代理模式使⽤了更加智能的⽅法。在UUPS代理模式中，代理合约只负责将所有调⽤转发到实现合约，并将实现合约的地址存储在代理合约的状态变量中。在升级合约时，只需要将
新的实现合约的代码上传到现有的代理合约地址，⽽⽆需部署新的代理合约。这种⽅法的优点是，可以在不更改代理合约地址的情况下升级合约，从⽽避免了每次升级合约时都需要部署⼀个新的代理合
约的问题。
InTransparentUpgradeableProxymode,theproxycontractisonlyresponsibleforforwardingallcallstotheimplementationcontractandstoringtheaddressofthe
implementationcontractintheproxycontract'sstatevariable.Atthetimeofupgradingthecontract,thenewimplementationcontractwillbedeployedtothenewaddressandthentheaddressofthenewimplementationcontractwillbestoredinthestatevariableoftheproxycontract.Thedisadvantageofthisapproachisthatanewagentcontractneedstobe
deployedeachtimethecontractisupgraded.
Incontrast,theUUPSproxymodelusesasmarterapproach.IntheUUPSproxymodel,theproxycontractisonlyresponsibleforforwardingallcallstotheimplementationcontractandzstoringtheaddressoftheimplementationcontractintheproxycontract'sstatevariable.Whenupgradingacontract,onlythecodeofthenewimplementationcontractneedstobe
uploadedtotheaddressoftheexistingproxycontractwithoutdeployinganewproxycontract.Theadvantageofthisapproachisthatthecontractcanbeupgradedwithoutchangingtheproxycontractaddress,thusavoidingtheproblemofdeployinganewproxycontracteverytimethecontractisupgraded.

### 16、ERC777代币存在什么危险?
### WhatarethedangersofERC777tokens?

ERC777代币是⼀种功能型代币，它在ERC20标准的基础上进⾏了改进，解决了⼀些ERC20标准存在的问题。ERC777代币的主要优势是它⽀持发送代币时携带额外的信息，同时也⽀持代币的操作员
功能。此外，ERC777代币还可以通过ERC1820接⼝注册表合约来实现代币转账的监听，增强了代币的安全性.虽然ERC777代币有很多优点，但是它也存在⼀些潜在的危险。由于ERC777代币是⼀种相
对较新的代币标准，因此它的⽣态系统相对较⼩，可能存在⼀些安全漏洞。此外，ERC777代币的操作员功能也可能会被滥⽤，导致代币被盗或者其他安全问题。因此，在使⽤ERC777代币时，需要谨慎
选择代币合约，同时也需要注意代币的安全性.
TheERC777TokenisafunctionaltokenthatimprovesontheERC20standardbyaddressingsomeoftheproblemsoftheERC20standard.ThemainadvantageoftheERC777Tokenisthatit
supportstheabilitytosendtokenswithadditionalinformation,aswellassupportfortokenoperatorfunctions.Inaddition,ERC777tokenscanlistentotokentransfersthroughthe
ERC1820interfaceregistrycontract,whichenhancesthesecurityoftokens.AlthoughtheERC777tokenhasmanyadvantages,italsohassomepotentialdangers.Since
theERC777tokenisarelativelynewtokenstandard,ithasarelativelysmallecosystemandmayhavesomesecurityvulnerabilities.Inaddition,theoperatorfunctionofERC777
tokenscanbeabused,leadingtotokentheftorothersecurityissues.Therefore,whenusingERC777tokens,youneedtochoosethetokencontractcarefullyandalsopayattentionto
thesecurityofthetokens.

### 17、OpenZeppelinERC721实现中的safeMint与mint有何不同？
### WhatisthedifferencebetweensafeMintandmintintheOpenZeppelinERC721implementation?

在OpenZeppelinERC721实现中，safeMint和mint都是⽤于创建新的ERC721代币的函数，但它们之间有⼀些区别。mint函数只是简单地创建⼀个新的ERC721代币，并将其分配给指定的地址。⽽
safeMint函数则会在创建新的ERC721代币之前检查⽬标地址是否⽀持ERC721转移。如果⽬标地址不⽀持ERC721转移，则safeMint函数会抛出异常并阻⽌创建新的ERC721代币。因此，safeMint函数⽐
mint函数更安全，因为它可以防⽌ERC721代币被锁定在不⽀持ERC721转移的合约中。 
IntheOpenZeppelinERC721implementation,bothsafeMintandmintarefunctionsusedtocreatenewERC721tokens,buttherearesomedifferencesbetweenthem.Mintfunctionsimply
createsanewERC721tokenandassignsittothespecifiedaddress.ThesafeMintfunction,ontheotherhand,checksifthetargetaddresssupportsERC721transfersbeforecreatinga
newERC721token.IfthetargetaddressdoesnotsupportERC721transfers,thesafeMintfunctionthrowsanexceptionandpreventsthecreationofnewERC721tokens.Therefore,the
safeMintfunctionissaferthanthemintfunctionbecauseitpreventsERC721tokensfrombeinglockedincontractsthatdonotsupportERC721transfers.

### 18、ERC165作⽤于什么？
### WhatdoesERC165do?

ERC165是⼀个标准，⽤于检测和发布智能合约实现的接⼝。它标准化了如何识别接⼝，如何检测它们是否实现了ERC165或其他接⼝，以及合约将如何发布它们实现的接⼝。它可以帮助您查询合约实
现的特定接⼝，以及更重要的是，该智能合约实现的版本。
ERC165isastandardfordetectingandpublishinginterfacesimplementedbysmartcontracts.Itstandardizeshowinterfacesareidentified,howtodetectiftheyimplementERC165
orotherinterfaces,andhowcontractswillpublishtheinterfacestheyimplement.Ithelpsyoutoquerythespecificinterfacethatacontractimplementsand,moreimportantly,the
versionofthatsmartcontractimplementation.

### 19、ERC721A如何减少铸造成本?有什么权衡？
### HowdoesERC721Areducecastingcosts?Whatarethetradeoffs?

ERC721A是⼀个改进的ERC721标准，旨在通过减少铸造成本来提⾼NFT的可扩展性。ERC721A通过引⼊批量铸造API来实现这⼀点，从⽽将铸造成本降低到O(1)的时间复杂度。与OZ的单独铸造⽅式不
同，ERC721A的批量铸造API可以同时铸造多个NFT，⽽不需要循环调⽤单独的铸造⽅法。这种⽅法可以显著减少铸造成本，但需要权衡的是，它可能会降低NFT的安全性。
ERC721AisanimprovedERC721standarddesignedtoimprovethescalabilityofNFTsbyreducingcastingcosts.ERC721AaccomplishesthisbyintroducingabatchcastingAPIthat
reducescastingcoststoO(1)timecomplexity.UnlikeOZ'sindividualcastingapproach,ERC721A'sbatchcastingAPIallowsforthesimultaneouscastingofmultipleNFTswithoutthe
needforcycliccallstoindividualcastingmethods.Thisapproachcansignificantlyreducecastingcosts,butthetradeoffisthatitmayreducethesecurityoftheNFTs.

### 20、CompoundFinance如何计算利⽤率？
### HowdoesCompoundFinancecalculateutilizationrates?

CompoundFinance是⼀个算法化的、⾃治的利率协议，旨在为开发者解锁⼀系列开放式⾦融应⽤。该协议的利⽤率是指借款⼈从Compound借⼊资产的数量与抵押品价值的⽐率。具体地，它是通
过将所有借款⼈的借款总额除以所有抵押品的总价值来计算的。这个⽐率越⾼，代表Compound的借款⼈越多，市场上的资⾦也越紧张。（从compound中借款，可通过增加抵押品价值降低借款利率）。
CompoundFinanceisanalgorithmic,autonomousrateprotocoldesignedtounlockarangeofopenfinanceapplicationsfordevelopers.Theprotocol'sutilizationrateistheratioof
theamountofassetsaborrowerborrowsfromCompoundtothevalueofthecollateral.Specifically,itiscalculatedbydividingthetotalamountborrowedbyallborrowersbythetotalvalueofallcollateral.Thehigherthisratio,themoreborrowersCompoundhasandthetighterthemarketis.(BorrowingfromCompoundreducestheborrowingratebyincreasingthe
valueofthecollateral.)
