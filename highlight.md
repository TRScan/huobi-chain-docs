# Highlight

## v0.5.0-beta.4 -> v0.5.0-beta.rc
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
