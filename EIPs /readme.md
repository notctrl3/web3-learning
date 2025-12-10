## 1️⃣ 代币标准
| ERC/EIP     | 功能                | 备注                      |
| ----------- | ----------------- | ----------------------- |
| **ERC20**   | Fungible Token 标准 | 基础代币                    |
| **ERC721**  | NFT 标准            | 非同质化代币                  |
| **ERC1155** | Multi-token 标准    | 支持 NFT + FT 混合          |
| **ERC777**  | Token 标准升级        | 支持 hooks / operators    |
| **ERC4626** | Tokenized Vault   | Vault 规范|

## 2️⃣ 签名 / MetaTransaction / Permit
| EIP/ERC               | 功能                          | 常用场景                     |
| ---------------------------------- | ------------------- | ------------------------------- |
| **EIP712**            | Typed Data 签名               | MetaTx、Gasless TX、Permit |
| **ERC2612**           | ERC20 Permit                | 无需 approve，直接用签名授权       |
| **ERC3009**           | TransferWithAuthorization   | 允许链下签名 + 链上执行            |
| **ERC2771**           | MetaTx / Trusted Forwarder  | Gasless TX，Biconomy 等用   |
| **ERC1077**           | Gasless TX / Key Management | Smart wallet / meta tx   |
## 3️⃣ 升级 / Proxy / 可升级合约
| EIP/ERC                            | 功能                  | 常用场景                            |
| ---------------------------------- | ------------------- | ------------------------------- |
| **EIP1822 (UUPS)**                 | UUPS 可升级 Proxy      | 轻量 Proxy，逻辑合约控制升级               |
| **EIP1967**                        | Proxy Storage Slots | Proxy 地址/Implementation slot 标准 |
| **EIP2535 (Diamond)**              | 多逻辑合约组合             | 大型模块化合约，Facet 可升级               |
| **OpenZeppelin Transparent Proxy** | Proxy + admin       | 传统治理升级，广泛用在 DeFi                |
## 4️⃣ 其他
| EIP/ERC             | 功能                   |  常用场景               |
| ---------------------------------- | ------------------- | ------------------------------- |
| **EIP-1271**        | 合约签名验证               | 合约钱包签名验证       |
| **ERC1820**         | Interface registry   | 合约接口注册         |
| **ERC223** | Token transfer hooks |      |
| **ERC165**          | Interface detection  | ERC721/1155 必备 |
