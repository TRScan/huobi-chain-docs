# Highlights

## v0.5.0 -> v0.6.0
### Chain ：
#### 💥 Breaking Changes
* N/A

#### ✨ Other Highlights
* 删除了多余的数据 clone,提高了性能
* network 修复了网络可能的 hanging 的 bug
* cli 增加了自由设定默认 config path 和 genesis path 的方法
* 允许 asset 服务 mint 初始 supply 为0的 asset
* timestamp 的 feedtime 方法现在会在 stale timestamp 的情况下返回失败
* kyc 的 所有 payload 现在被重新校验
* riscv 添加了 setAdmin 方法,并且 contract_call 支持 bech32 格式

### SDK：
#### 💥 Breaking Changes
* N/A

#### Highlight
* 添加了支持 riscv 服务的 setAdmin


## v0.5.0-rc.2 -> v0.5.0
### Chain ：
#### 💥 Breaking Changes
* N/A

#### ✨ Other Highlights
* 现在日志会打印出 view change 的原因
* state 修复了一个读取空值时的 bug
* 添加网络数据包大小累计计算
* 采用最新的 overlord 共识
* 添加结构化日志 api
* 支持通过 http 请求 dump profile
* 网络将会同时广播数据的大小
* 修复网络错误的共识链接计数
* 添加 cli 工具,现在可以冷链操作区块链,提供基础的维护功能
* mempool wal 现在被重新基于文件系统实现
* 修复了 storeMap, storeArray 下读取空数据的错误
* 添加了自定义的 json! 宏,方便日志系统输出
* 修复了来自审计的bug
    - asset 服务未校验数据
    - riscv 服务可能造成的数据溢出
* 添加了 transfer_quota 服务以及 timestamp服务

### SDK：
#### 💥 Breaking Changes
* N/A

#### Highlight
* 添加了支持 transfer_quota 服务和 timestamp服务的接口


## v0.5.0-rc -> v0.5.0-rc.2
### Chain ：
#### 💥 Breaking Changes
* executor 现在会将执行失败的 tx 正确地回滚了

#### ✨ Other Highlights
* 添加了更多的 metric 的指标
* 当节点 p2p 握手超时,丢弃节点
* 现在 p2p 会尝试重新链接一些 unconnectable 的节点
* 现在 p2p 当遇到 secio io error 的时候,会放弃节点
* 添加了拜占庭测试
* 根据log4rs, 现在日志文件可以设定按大小拆分文件, [参考配置](https://github.com/nervosnetwork/muta/blob/9fa17d795dc2e27e9a59753d86b03805c7fe1c5e/devtools/chain/config.toml#L46)
* 现在共识可以设定执行 gap, 以及单次同步最大 tx 数量, [参考配置](https://github.com/nervosnetwork/muta/blob/9fa17d795dc2e27e9a59753d86b03805c7fe1c5e/devtools/chain/config.toml#L23)
* Overlord 更新
    - 修复了无法随机出块的 bug
    - 修复了应当在过滤 proposal 之前校验 proposer 的 bug
* 修复了一些 prometheus 指标的小错误
* 现在在节点重启时, 会正确地从 wal 中恢复合法的交易并传送如 mempool
* network reactor 修复, network 不会再因为 reactor 崩溃而无法处理更多的网络消息
* 规范了 log api,更好地适配监控
* 一些其他软件工程上的重构
* 为 Governance 服务添加了查询 miner 的接口
* Riscv 服务被添加回来

### SDK：
#### 💥 Breaking Changes
* N/A

#### Highlight
* 添加可以查询新的 Governance 接口
* js sdk
    - 修复了 riscv 测试
    - 更好的项目重构

## v0.5.0-beta.4 -> v0.5.0-rc
### Chain ：
#### 💥 Breaking Changes
* Change the kyc service logic.

#### ✨ Other Highlights
* 配合审计，修复潜在风险
    - p2p 握手时，检验 chain id。
    - p2p 传输数据包分片。
    
### SDK：
#### 💥 Breaking Changes
* N/A

#### ✨ Other Highlights
* java sdk 增添了一些小需求。
* js sdk 添加了 runtime type check

## v0.5.0-beta.2.1 -> v0.5.0-beta.4
### Chain ：
#### 💥 Breaking Changes
* 升级 muta 到 v0.2.0-beta.4
* event 下的 service 字段可以由 service 指定，不再是 ServiceContext.service_name。
* admisson control 给出了拒绝 tx 的明确理由。
* governance 现在可以正确地更新 metadata了。

#### ✨ Other Highlights
* 性能优化， trieDB 支持 cache。[参考配置](https://github.com/nervosnetwork/muta-docs/blame/v0.2.0-beta.4/docs/setup/node-config.md#L45)
* 修复错误，hrp 将在 genesis 之前被加载。
* 修复错误，apm 将在统计更精准的时间
* 性能优化，限制一次同步 block 数量上限。
* 配合审计，修复潜在风险
    - graphql service 支持 tls 链接，[参考配置](https://github.com/nervosnetwork/muta-docs/blame/v0.2.0-beta.4/docs/setup/node-config.md#L86)
    - 限制相同 ip 地址的连接数

### SDK：
#### 💥 Breaking Changes
* java sdk 修复了由于 graphql 查询语句缺少字段导致返回对象数据不完全的bug。
* js sdk 修复了 setDefaultVariables 方法的错误。
* js sdk 更新了 interface，补充了 graphql 查询语句。
* js sdk 暴露了 metadata service。


## v0.5.0-beta.1 -> v0.5.0-beta.2.1
### Chain ：
#### 💥 Breaking Changes
* bech32 的 hrp 将从 genesis.toml 配置，且不可修改。不需要再通过环境变量的方式在编译期确定 hrp 了。可以参考 [genesis 配置文档](/config#创世块)。
* Multisig 服务修改了接口参数，不会再产生歧义。
* 显式移除了 tx_fee_inlet_address 字段，他在之前的版本已经被废弃了，现在 genesis.toml 里面也将看不到它的身影。


#### ✨ Other Highlights
* 按权重随机出块已被加入到 huobi chain，可以通过给 muta 传递 features = ["random_leader"] 显示开启，默认开启。详细配置方式可以参考[出块方式配置文档](/config#出块方式配置)。
* 直接移除了 tx_hook 的 cycles 计费，原则上计算手续费的步骤不会失败。
* kyc 服务的 orgName 和 tagName 都支持到长度为32字节的 ASCII 字符， 且 tag 的 capacity 被扩充到16。
* rocksdb 升级到了 v0.14
* 根据审计需求
    - 添加了针对 timestamp 的检查。
    - 修正了 p2p 的多余检查

### SDK：
#### 💥 Breaking Changes
* Multisig 服务修改了接口参数，不会再产生歧义。
* 适配了 metadata 的接口参数，添加了 bech32_address_hrp，提供了 validate bech32 地址格式这样的功能性方法
