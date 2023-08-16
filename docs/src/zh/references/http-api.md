---
locale: zh
---
# Arweave peer HTTP API
有关Arweave同级HTTP API的更完整参考，请参阅[链接指南](https://docs.arweave.org/developers/server/http-api)。

这里提供的端点是为了方便和/或因为它们在[链接指南](https://docs.arweave.org/developers/server/http-api)中被省略。

::: 信息
Permaweb网关服务通常由一个或多个完整的Arweave节点支持。因此，它们通常会在`/tx/`路径下公开节点端点，并直接路由请求到Arweave节点。这意味着这些方法通常可以在网关上调用，也可以直接在Arweave同级/节点上调用。
:::

<hr />

### 按字段获取
直接从Arweave节点检索与事务关联的标头字段。如果节点存储了块，并且数据足够小以便节点提供服务，则可用于检索事务数据。

`https://arweave.net/tx/TX_ID/FIELD`

可用字段：id | last_tx | owner | target | quantity | data | reward | signature
```js
const result = await fetch('https://arweave.net/tx/sHqUBKFeS42-CMCvNqPR31yEP63qSJG3ImshfwzJJF8/data')
// 字段以base64url格式返回，因此我们需要解码
const base64url = await result.text()
const jsonData = JSON.parse( Arweave.utils.b64UrlToString(base64url) )
console.log(jsonData)
```

<details>
<summary><b>点击查看示例结果</b></summary>

```json
{
  "ticker":"ANT-PENDING",
  "name":"pending",
  "owner":"NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0",
  "controller":"NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0",
  "evolve":null,
  "records": {
    "@":"As-g0fqvO_ALZpSI8yKfCZaFtnmuwWasY83BQ520Duw"
  },
  "balances":{"NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0":1}
}
```
</details>
<hr />

### 获取钱包余额
返回的余额以Winston为单位。要以$AR获取余额，请将余额除以1000000000000。
`https://arweave.net/wallet/ADDRESS/balance`
```js
const res = await axios.get(`https://arweave.net/wallet/NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0/balance`)
console.log(res)
console.log(res.data / 1000000000000)

6638463438702 // Winston
6.638463438702 // $AR
```
<hr />

### 获取事务状态
`https://arweave.net/tx/TX_ID/status`
::: 提示
此端点仅支持基本的Arweave事务，而不支持捆绑事务。事务必须在链上确认后，其状态才可用。
:::

```js
  const response = await fetch('https://arweave.net/tx/EiRSQExb5HvSynpn0S7_dDnwcws1AJMxoYx4x7nWoho/status')
  const result = await response.json()
  console.log(JSON.stringify(result))
```
<details>
<summary><b>点击查看示例结果</b></summary>

```json
{
  "block_height":1095552,"block_indep_hash":"hyhLEyOw5WcIhZxq-tlnxhnEFgKChKHFrMoUdgIg2Sw0WoBMbdx6uSJKjxnQWon3","number_of_confirmations":10669
}

```
</details>
<hr />

### 获取网络信息

```js
const res = await axios.get('https://arweave.net/info')
console.log(res.data)
```

<details>
<summary><b>点击查看示例结果</b></summary>

```json
{
    "network": "arweave.N.1",
    "version": 5,
    "release": 53,
    "height": 1106211,
    "current": "bqPU_7t-TdRIxgsja0ftgEMNnlGL6OX621LPJJzYP12w-uB_PN4F7qRYD-DpIuRu",
    "blocks": 1092577,
    "peers": 13922,
    "queue_length": 0,
    "node_state_latency": 0
}

```
</details>
<hr />