# USDY 合约参考手册

## 合约清单

### 核心合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **RWAHub** | `contracts/RWAHub.sol` | 抽象合约 | RWA 代币申购/赎回的核心逻辑 |
| **RWAHubOffChainRedemptions** | `contracts/RWAHubOffChainRedemptions.sol` | 抽象合约 | 扩展支持链下赎回功能 |
| **USDYManager** | `contracts/usdy/USDYManager.sol` | 具体合约 | USDY 的 RWAHub 实现 |

### 代币合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **USDY** | `contracts/usdy/USDY.sol` | 可升级 ERC20 | USDY 代币实现 |
| **USDYFactory** | `contracts/usdy/USDYFactory.sol` | 工厂合约 | 部署可升级 USDY 实例 |

### 定价合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **Pricer** | `contracts/Pricer.sol` | 具体合约 | 基础定价器，管理价格ID映射 |
| **PricerWithOracle** | `contracts/PricerWithOracle.sol` | 具体合约 | 带预言机同步的定价器 |

### 预言机合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **RWAOracleRateCheck** | `contracts/rwaOracles/RWAOracleRateCheck.sol` | 具体合约 | 基于速率限制的价格验证 |
| **RWAOracleExternalComparisonCheck** | `contracts/rwaOracles/RWAOracleExternalComparisonCheck.sol` | 具体合约 | 基于 Chainlink 对比的价格验证 |

### 合规合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **AllowlistUpgradeable** | `contracts/usdy/allowlist/AllowlistUpgradeable.sol` | 可升级合约 | 白名单管理 (可升级) |
| **AllowlistFactory** | `contracts/usdy/allowlist/AllowlistFactory.sol` | 工厂合约 | 部署白名单实例 |
| **Blocklist** | `contracts/usdy/blocklist/Blocklist.sol` | 不可升级合约 | 黑名单管理 |
| **SanctionsListClient** | `contracts/sanctions/SanctionsListClient.sol` | 抽象合约 | 制裁名单客户端 |
| **SanctionsListClientUpgradeable** | `contracts/sanctions/SanctionsListClientUpgradeable.sol` | 可升级合约 | 可升级制裁名单客户端 |

### 客户端合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **BlocklistClient** | `contracts/usdy/blocklist/BlocklistClient.sol` | 抽象合约 | 黑名单客户端 (不可升级) |
| **BlocklistClientUpgradeable** | `contracts/usdy/blocklist/BlocklistClientUpgradeable.sol` | 抽象合约 | 黑名单客户端 (可升级) |
| **AllowlistClient** | `contracts/usdy/allowlist/AllowlistClient.sol` | 抽象合约 | 白名单客户端 (不可升级) |
| **AllowlistClientUpgradeable** | `contracts/usdy/allowlist/AllowlistClientUpgradeable.sol` | 抽象合约 | 白名单客户端 (可升级) |

### 基础设施合约

| 合约 | 路径 | 类型 | 说明 |
|------|------|------|------|
| **Proxy** | `contracts/Proxy.sol` | 代理合约 | EIP-1967 透明代理 |
| **AllowlistProxy** | `contracts/usdy/allowlist/AllowlistProxy.sol` | 代理合约 | 白名单专用代理 |

---

## 接口清单

| 接口 | 路径 | 说明 |
|------|------|------|
| `IRWAHub` | `contracts/interfaces/IRWAHub.sol` | RWAHub 核心接口 |
| `IRWAHubOffChainRedemptions` | `contracts/interfaces/IRWAHubOffChainRedemptions.sol` | 链下赎回接口 |
| `IRWALike` | `contracts/interfaces/IRWALike.sol` | RWA 代币接口 |
| `IUSDYManager` | `contracts/interfaces/IUSDYManager.sol` | USDYManager 接口 |
| `IPricer` | `contracts/interfaces/IPricer.sol` | 定价器接口 |
| `IPricerReader` | `contracts/interfaces/IPricerReader.sol` | 定价器只读接口 |
| `IPricerWithOracle` | `contracts/interfaces/IPricerWithOracle.sol` | 带预言机定价器接口 |
| `IRWAOracle` | `contracts/rwaOracles/IRWAOracle.sol` | RWA 预言机接口 |
| `IRWAOracleSetter` | `contracts/rwaOracles/IRWAOracleSetter.sol` | 预言机设置接口 |
| `IRWAOracleExternalComparisonCheck` | `contracts/rwaOracles/IRWAOracleExternalComparisonCheck.sol` | 外部比较预言机接口 |
| `IAllowlist` | `contracts/interfaces/IAllowlist.sol` | 白名单接口 |
| `IAllowlistClient` | `contracts/interfaces/IAllowlistClient.sol` | 白名单客户端接口 |
| `IBlocklist` | `contracts/interfaces/IBlocklist.sol` | 黑名单接口 |
| `IBlocklistClient` | `contracts/interfaces/IBlocklistClient.sol` | 黑名单客户端接口 |
| `ISanctionsListClient` | `contracts/sanctions/ISanctionsListClient.sol` | 制裁名单客户端接口 |
| `IMulticall` | `contracts/interfaces/IMulticall.sol` | 批量调用接口 |

---

## 角色权限速查

### RWAHub / USDYManager

| 角色 | 权限 |
|------|------|
| `DEFAULT_ADMIN_ROLE` | 系统默认管理员 |
| `MANAGER_ADMIN` | 设置参数、管理其他角色、覆盖存款/赎回记录 |
| `PAUSER_ADMIN` | 暂停申购/赎回 |
| `PRICE_ID_SETTER_ROLE` | 为存款/赎回设置价格ID |
| `RELAYER_ROLE` | 添加链外存款证明 |
| `TIMESTAMP_SETTER_ROLE` | (USDYManager) 设置可认领时间戳 |

### USDY

| 角色 | 权限 |
|------|------|
| `DEFAULT_ADMIN_ROLE` | 代币管理员 |
| `MINTER_ROLE` | 铸造代币 |
| `PAUSER_ROLE` | 暂停转账 |
| `BURNER_ROLE` | 销毁指定地址的代币 |
| `LIST_CONFIGURER_ROLE` | 设置白名单/黑名单/制裁名单地址 |

### Pricer / PricerWithOracle

| 角色 | 权限 |
|------|------|
| `DEFAULT_ADMIN_ROLE` | 定价器管理员 |
| `PRICE_UPDATE_ROLE` | 添加/更新价格 |

### RWAOracle

| 角色 | 权限 |
|------|------|
| `DEFAULT_ADMIN_ROLE` | 预言机管理员 |
| `SETTER_ROLE` | 设置价格 |

### Allowlist

| 角色 | 权限 |
|------|------|
| `DEFAULT_ADMIN_ROLE` | 白名单管理员 |
| `ALLOWLIST_ADMIN` | 管理条款、设置有效条款索引 |
| `ALLOWLIST_SETTER` | 直接设置账户状态 |

---

## 关键函数速查

### 用户操作

| 函数 | 合约 | 说明 |
|------|------|------|
| `requestSubscription(amount)` | USDYManager | 发起申购 |
| `claimMint(depositIds)` | USDYManager | 认领铸造的 USDY |
| `requestRedemption(amount)` | USDYManager | 发起链上赎回 |
| `claimRedemption(redemptionIds)` | USDYManager | 认领赎回的 USDC |
| `requestRedemptionServicedOffchain(amount, dest)` | USDYManager | 发起链下赎回 |

### 管理员操作

| 函数 | 合约 | 说明 |
|------|------|------|
| `setPriceIdForDeposits(depositIds, priceIds)` | USDYManager | 设置存款价格ID |
| `setPriceIdForRedemptions(redemptionIds, priceIds)` | USDYManager | 设置赎回价格ID |
| `setClaimableTimestamp(timestamp, depositIds)` | USDYManager | 设置可认领时间 |
| `addPrice(price, timestamp)` | Pricer | 添加新价格 |
| `setPrice(newPrice)` | RWAOracle | 设置预言机价格 |

### 白名单操作

| 函数 | 合约 | 说明 |
|------|------|------|
| `addSelfToAllowlist(termIndex)` | Allowlist | 用户自注册 |
| `addAccountToAllowlist(termIndex, account, v, r, s)` | Allowlist | 签名验证入表 |
| `setAccountStatus(account, termIndex, status)` | Allowlist | 管理员设置状态 |

---

## 事件速查

### 申购/赎回事件

| 事件 | 说明 |
|------|------|
| `MintRequested` | 申购请求已提交 |
| `MintCompleted` | 铸造完成 |
| `RedemptionRequested` | 链上赎回请求已提交 |
| `RedemptionCompleted` | 链上赎回完成 |
| `RedemptionRequestedServicedOffChain` | 链下赎回请求已提交 |
| `DepositProofAdded` | 链外存款证明已添加 |

### 价格事件

| 事件 | 说明 |
|------|------|
| `PriceAdded` | 新价格已添加 |
| `PriceUpdated` | 价格已更新 |
| `RWAPriceSet` | 预言机价格已设置 |
| `PriceIdSetForDeposit` | 存款价格ID已设置 |
| `PriceIdSetForRedemption` | 赎回价格ID已设置 |

### 合规事件

| 事件 | 说明 |
|------|------|
| `AccountAddedSelf` | 用户自注册白名单 |
| `AccountAddedFromSignature` | 签名验证入白名单 |
| `AccountStatusSetByAdmin` | 管理员设置账户状态 |
| `BlockedAddressesAdded` | 地址已加入黑名单 |
| `BlockedAddressesRemoved` | 地址已从黑名单移除 |

### 管理事件

| 事件 | 说明 |
|------|------|
| `SubscriptionPaused` / `SubscriptionUnpaused` | 申购暂停/恢复 |
| `RedemptionPaused` / `RedemptionUnpaused` | 赎回暂停/恢复 |
| `OffChainRedemptionPaused` / `OffChainRedemptionUnpaused` | 链下赎回暂停/恢复 |
| `MintFeeSet` | 铸造费率已设置 |
| `RedemptionFeeSet` | 赎回费率已设置 |
| `MinimumDepositAmountSet` | 最小存款金额已设置 |
| `MinimumRedemptionAmountSet` | 最小赎回金额已设置 |

---

## 错误码速查

### 通用错误

| 错误 | 说明 |
|------|------|
| `CollateralCannotBeZero()` | 抵押品地址不能为零 |
| `RWACannotBeZero()` | RWA 地址不能为零 |
| `FeaturePaused()` | 功能已暂停 |
| `DepositTooSmall()` | 存款金额过小 |
| `RedemptionTooSmall()` | 赎回金额过小 |
| `PriceIdNotSet()` | 价格ID未设置 |
| `ArraySizeMismatch()` | 数组长度不匹配 |

### USDYManager 错误

| 错误 | 说明 |
|------|------|
| `BlockedAccount()` | 账户被封锁 |
| `SanctionedAccount()` | 账户被制裁 |
| `ClaimableTimestampNotSet()` | 可认领时间戳未设置 |
| `MintNotYetClaimable()` | 铸造尚未到可认领时间 |
| `ClaimableTimestampInPast()` | 可认领时间戳在过去 |

### Pricer 错误

| 错误 | 说明 |
|------|------|
| `InvalidPrice()` | 无效价格 |
| `PriceIdDoesNotExist()` | 价格ID不存在 |
| `LatestPriceMismatch()` | 最新价格不匹配 |
| `PricesAlreadyMatch()` | 价格已匹配 |

### 预言机错误

| 错误 | 说明 |
|------|------|
| `PriceUpdateWindowViolation()` | 违反价格更新窗口限制 |
| `DeltaDifferenceConstraintViolated()` | 违反价格变化差异约束 |
| `AbsoluteDifferenceConstraintViolated()` | 违反绝对差异约束 |
| `ChainlinkOraclePriceStale()` | Chainlink 预言机价格过期 |
| `ChainlinkRoundNotUpdated()` | Chainlink 轮次未更新 |
| `CorruptedChainlinkResponse()` | Chainlink 响应损坏 |

### 白名单错误

| 错误 | 说明 |
|------|------|
| `AlreadyVerified()` | 已验证 |
| `InvalidTermIndex()` | 无效条款索引 |
| `InvalidVSignature()` | 无效签名 v 值 |
| `InvalidSigner()` | 无效签名者 |

---

## 常量速查

### RWAHub

| 常量 | 值 | 说明 |
|------|-----|------|
| `BPS_DENOMINATOR` | 10,000 | 基点分母 |
| `assetRecipient` | `0xbDa73A0F13958ee444e0782E1768aB4B76EdaE28` | 存款接收地址 |

### RWAOracleRateCheck

| 常量 | 值 | 说明 |
|------|-----|------|
| `MIN_PRICE_UPDATE_WINDOW` | 23 hours | 最小价格更新间隔 |
| `MAX_CHANGE_DIFF_BPS` | 100 | 最大价格变化 (1%) |

### RWAOracleExternalComparisonCheck

| 常量 | 值 | 说明 |
|------|-----|------|
| `MAX_CL_WINDOW` | 25 hours | Chainlink 数据最大有效期 |
| `MAX_CHANGE_DIFF_BPS` | 74 | 与 Chainlink 最大偏差 |
| `MAX_ABSOLUTE_DIFF_BPS` | 200 | 单次最大绝对变化 |
| `MIN_PRICE_UPDATE_WINDOW` | 23 hours | 最小价格更新间隔 |

---

## 外部依赖

| 依赖 | 地址/说明 |
|------|-----------|
| Chainalysis Sanctions Oracle | `0x40C57923924B5c5c5455c48D93317139ADDaC8fb` |
| OpenZeppelin Contracts | v4.x |
| Solidity | 0.8.16 |
