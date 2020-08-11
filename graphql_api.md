# Muta GraphQL API


>[GraphQL](https://graphql.org) is a query language for APIs and a runtime for fulfilling those queries with your existing data.
GraphQL provides a complete and understandable description of the data in your API,
gives clients the power to ask for exactly what they need and nothing more,
makes it easier to evolve APIs over time, and enables powerful developer tools.

Huobi-chain has embeded a [Graph*i*QL](https://github.com/graphql/graphiql) for checking and calling API. Started a the Huobi-chain
node, and then try open http://127.0.0.1:8000/graphiql in the browser.

Huobi-chain's graphql uses JSON to send request, please refer to [spec](https://spec.graphql.org/June2018/#sec-JSON-Serialization)

## Common usage

1.getBlock 请求的 request 样例

```json
{
   "operationName":"getBlock",
   "variables":{
      "height":"0x0"
   },
   "query":"query getBlock($height: Uint64) {\n  getBlock(height: $height) {\n    header {\n      chainId\n      confirmRoot\n      cyclesUsed\n      height\n      execHeight\n      orderRoot\n      prevHash\n      proposer\n      receiptRoot\n      stateRoot\n      timestamp\n      validatorVersion\n      proof {\n        bitmap\n        blockHash\n        height\n        round\n        signature\n      }\n      validators {\n        pubkey\n        proposeWeight\n        voteWeight\n      }\n    }\n    orderedTxHashes\n    hash\n  }\n}"
}
```

其中 query 内的内容，就是基于 graphql 的查询语句：
```graphql
query getBlock($height: Uint64) {
  getBlock(height: $height) {
    header {
      chainId
      confirmRoot
      cyclesUsed
      height
      execHeight
      orderRoot
      prevHash
      proposer
      receiptRoot
      stateRoot
      timestamp
      validatorVersion
      proof {
        bitmap
        blockHash
        height
        round
        signature
      }
      validators {
        pubkey
        proposeWeight
        voteWeight
      }
    }
    orderedTxHashes
    hash
  }
}
```

2.getTransaction 请求的 request 样例

```json
{
   "operationName":"getTransaction",
   "variables":{
      "txHash":null
   },
   "query":"query getTransaction($txHash: Hash!) {\n  getTransaction(txHash: $txHash) {\n    serviceName\n    method\n    payload\n    nonce\n    chainId\n    cyclesLimit\n    cyclesPrice\n    timeout\n    txHash\n    pubkey\n    sender\n    signature\n  }\n}"
}
```

其中 query 内的内容，就是基于 graphql 的查询语句：

```graphql
query getTransaction($txHash: Hash!) {
  getTransaction(txHash: $txHash) {
    serviceName
    method
    payload
    nonce
    chainId
    cyclesLimit
    cyclesPrice
    timeout
    txHash
    pubkey
    sender
    signature
  }
}
```
3.getReceipt 请求的 request 样例

```json
{
   "operationName":"getReceipt",
   "variables":{
      "txHash":null
   },
   "query":"query getReceipt($txHash: Hash!) {\n  getReceipt(txHash: $txHash) {\n    txHash\n    height\n    cyclesUsed\n    events {\n      data\n      name\n      service\n    }\n    stateRoot\n    response {\n      serviceName\n      method\n      response {\n        code\n        errorMessage\n        succeedData\n      }\n    }\n  }\n}"
}
```

其中 query 内的内容，就是基于 graphql 的查询语句：

```graphql
query getReceipt($txHash: Hash!) {
  getReceipt(txHash: $txHash) {
    txHash
    height
    cyclesUsed
    events {
      data
      name
      service
    }
    stateRoot
    response {
      serviceName
      method
      response {
        code
        errorMessage
        succeedData
      }
    }
  }
}
```

4.queryService 请求的 request 样例

```json
{
   "operationName":"queryService",
   "variables":{
      "serviceName":"asset",
      "method":"get_asset",
      "payload":"{\"id\":\"0x1efa8f3b1d0a4f173b44a0310ca83d781b1c343d9e4917da78b324bc4ede064d\"}",
      "height":null,
      "caller":"muta1qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqggfy0d",
      "cyclePrice":null,
      "cycleLimit":null
   },
   "query":"query queryService(\n  $serviceName: String!\n  $method: String!\n  $payload: String!\n  $height: Uint64\n  $caller: Address!\n  $cyclePrice: Uint64\n  $cycleLimit: Uint64\n) {\n  queryService(\n    height: $height\n    serviceName: $serviceName\n    method: $method\n    payload: $payload\n    caller: $caller\n    cyclesPrice: $cyclePrice\n    cyclesLimit: $cycleLimit\n  ) {\n    code\n    errorMessage\n    succeedData\n  }\n}"
}
````

其中 query 内的内容，就是基于 graphql 的查询语句：

```graphql
query queryService(
  $serviceName: String!
  $method: String!
  $payload: String!
  $height: Uint64
  $caller: Address!
  $cyclePrice: Uint64
  $cycleLimit: Uint64
) {
  queryService(
    height: $height
    serviceName: $serviceName
    method: $method
    payload: $payload
    caller: $caller
    cyclesPrice: $cyclePrice
    cyclesLimit: $cycleLimit
  ) {
    code
    errorMessage
    succeedData
  }
}
```

其中 $serviceName，$method 和 $payload 的定义见 sendTransaction。

5. sendTransaction

请求的 request 样例

```json
{
   "operationName":"sendTransaction",
   "variables":{
      "inputRaw":{
         "chainId":"0xb6a4d7da21443f5e816e8700eea87610e6d769657d6b8ec73028457bf2ca4036",
         "cyclesLimit":"0xffffffff",
         "cyclesPrice":"0x0",
         "nonce":"0x8a18039e251b4a6769da7dbf94fe6f47a3e66e4821c43055dccb4758edd1cbe0",
         "method":"create_asset",
         "serviceName":"asset",
         "payload":"{\"name\":\"Squirrel\",\"symbol\":\"SQU\",\"supply\":1314}",
         "timeout":"0x18c",
         "sender":"muta14tk7le96va4shfg7kcd4prhqqxmvgl8c9fp4n2"
      },
      "inputEncryption":{
         "pubkey":"0xe2a102ee34d1ce8270cd236e9455d4ab9e756c4478779b1a20d7ce1c247af61ec2be3b",
         "signature":"0xf842b8400a47a32189cc371ccff5181d1dfa36d5bb0c119e6e441673c9bf53acd80041bd5b93a7136820383c601b43f00867ba8d4dc4e1690b685763d61a31d178d1663d",
         "txHash":"0x3ea3eeeee5fc45670ad629698d744f05c1c85200544b3ffe710b4c83ba195038"
      }
   },
   "query":"mutation sendTransaction(\n  $inputRaw: InputRawTransaction!\n  $inputEncryption: InputTransactionEncryption!\n) {\n  sendTransaction(inputRaw: $inputRaw, inputEncryption: $inputEncryption)\n}"
}
```

其中的 query

```graphql
mutation sendTransaction(
  $inputRaw: InputRawTransaction!
  $inputEncryption: InputTransactionEncryption!
) {
  sendTransaction(inputRaw: $inputRaw, inputEncryption: $inputEncryption)
}
```
InputRawTransaction 和 InputTransactionEncryption 在样例里已给出

InputRawTransaction.serviceName 是需要调用的 service 的名字，service 的名字是由 service 在 ServiceMapping 中
注册时给定的。例如：

```rust
pub const AUTHORIZATION_SERVICE_NAME: &str = "authorization";
pub const ASSET_SERVICE_NAME: &str = "asset";
pub const METADATA_SERVICE_NAME: &str = "metadata";
pub const KYC_SERVICE_NAME: &str = "kyc";
pub const MULTI_SIG_SERVICE_NAME: &str = "multi_signature";
pub const GOVERNANCE_SERVICE_NAME: &str = "governance";
            ADMISSION_CONTROL_SERVICE_NAME.to_owned(),


impl ServiceMapping for DefaultServiceMapping {
    fn get_service<SDK: 'static + ServiceSDK, Factory: SDKFactory<SDK>>(
        &self,
        name: &str,
        factory: &Factory,
    ) -> ProtocolResult<Box<dyn Service>> {
        let service = match name {
            AUTHORIZATION_SERVICE_NAME => {
                Box::new(Self::new_authorization(factory)?) as Box<dyn Service>
            }
            GOVERNANCE_SERVICE_NAME => Box::new(Self::new_governance(factory)?) as Box<dyn Service>,
            ADMISSION_CONTROL_SERVICE_NAME => {
                Box::new(Self::new_admission_ctrl(factory)?) as Box<dyn Service>
            }
            ASSET_SERVICE_NAME => Box::new(Self::new_asset(factory)?) as Box<dyn Service>,
            METADATA_SERVICE_NAME => Box::new(Self::new_metadata(factory)?) as Box<dyn Service>,
            KYC_SERVICE_NAME => Box::new(Self::new_kyc(factory)?) as Box<dyn Service>,
            MULTI_SIG_SERVICE_NAME => Box::new(Self::new_multi_sig(factory)?) as Box<dyn Service>,
            _ => panic!("not found service"),
        };

        Ok(service)
    }

    fn list_service_name(&self) -> Vec<String> {
        vec![
            AUTHORIZATION_SERVICE_NAME.to_owned(),
            ASSET_SERVICE_NAME.to_owned(),
            METADATA_SERVICE_NAME.to_owned(),
            KYC_SERVICE_NAME.to_owned(),
            MULTI_SIG_SERVICE_NAME.to_owned(),
            GOVERNANCE_SERVICE_NAME.to_owned(),
            ADMISSION_CONTROL_SERVICE_NAME.to_owned(),
        ]
    }
}
```

其中 Admission Control Service 的名字是 "authorization"，在这里也就是常亮 AUTHORIZATION_SERVICE_NAME。

InputRawTransaction.serviceName 是调用 service 的方法名，必须通过 [read] 或 [write] 在 service 中定义：

```rust
#[service]
impl<SDK: ServiceSDK> AssetService<SDK> {

    #[cycles(100_00)]
    #[read]
    fn get_allowance(
        &self,
        _ctx: ServiceContext,
        payload: GetAllowancePayload,
    ) -> ServiceResponse<GetAllowanceResponse> {
        //省略方法..
    }

    #[cycles(210_00)]
    #[write]
    fn create_asset(
        &mut self,
        ctx: ServiceContext,
        payload: CreateAssetPayload,
    ) -> ServiceResponse<Asset> {
    //省略方法..
    }   

    //其他省略方法..
}
```

样例中定义了 read 方法 get_allowance，和 write 方法 create_asset。

InputRawTransaction.payload 是调用 method 方法是需要给出的参数。

在上面的 create_asset 方法中，他的入参 CreateAssetPayload 的定义如下。
```rust
#[derive(Deserialize, Serialize, Clone, Debug)]
pub struct CreateAssetPayload {
    pub name:       String,
    pub symbol:     String,
    pub admin:      Address,
    pub supply:     u64,
    pub init_mints: Vec<IssuerWithBalance>,
    pub precision:  u64,
    pub relayable:  bool,
}
```

我们根据入参，需要将 CreateAssetPayload 序列化成 JSON 字符串，然后交给 InputRawTransaction.payload 传递。
故而 InputRawTransaction.payload：

"payload":"{\"name\":\"Squirrel\",\"symbol\":\"SQU\",\"supply\":1314}"。
