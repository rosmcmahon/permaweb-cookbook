---
locale: zh
---
# 获取交易数据
尽管索引服务允许查询交易元数据，但它们并不提供对交易数据本身的访问权限。这是因为缓存交易数据和索引元数据具有不同的资源需求。索引服务主要依赖计算资源在数据库上执行查询，而交易数据更适合部署在内容分发网络（CDN）上，以优化存储和带宽。

大多数网关都提供了一组HTTP端点，用于提供事务数据的缓存服务。可以使用任何HTTP客户端/软件包来从这些端点请求事务数据。例如，JavaScript可以使用Axios或Fetch，PHP可以使用Guzzle，等等。

如果你想绕过事务数据缓存服务，直接从Arweave的节点/对等体获取数据，你也可以，但是需要付出很大的努力！

事务数据以256KB的连续块序列的形式存储在Arweave上，从网络的起始位置直到当前区块。这种格式被优化用于支持SPoRA挖掘机制，矿工通过参与其中来证明他们正在存储Arweave数据。

::: info
1. 从一个众所周知的对等体获取对等体列表。
1. 向对等体请求包含您事务数据的块偏移量。
1. 向对等体请求块。
    1. 如果对等体提供了块，请将它们组合成原始格式。
1. （如果对等体没有块）遍历对等体列表，并添加尚未在列表中的对等体。
1. 对每个访问的对等体，检查其对等体列表，并添加尚未在列表中的对等体。
1. 从步骤3开始重复，直到获得所有块。
:::

每次从Arweave网络中检索数据时，都需要执行相当大量的工作。想象一下，如果您想要显示一个类似于[https://public-square.g8way.io](https://public-square.g8way.io)的推文时间线，用户体验将会非常糟糕，加载时间会很长并且会出现加载图标。由于在Arweave上的数据是永久性的，可以将其以原始形式缓存，以使检索交易数据更快捷、更容易。

下面是如何访问arweave.net事务数据缓存服务中的已缓存事务数据。

### 获取已缓存的TX数据

`https://arweave.net/TX_ID`

```js
const res = await axios.get(`https://arweave.net/sHqUBKFeS42-CMCvNqPR31yEP63qSJG3ImshfwzJJF8`)
console.log(res)
```

<details>
<summary><b>点击查看示例结果</b></summary>

```json
{
    "data": {
        "ticker": "ANT-PENDING",
        "name": "pending",
        "owner": "NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0",
        "controller": "NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0",
        "evolve": null,
        "records": {
            "@": "As-g0fqvO_ALZpSI8yKfCZaFtnmuwWasY83BQ520Duw"
        },
        "balances": {
            "NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0": 1
        }
    },
    "status": 200,
    "statusText": "",
    "headers": {
        "cache-control": "public,must-revalidate,max-age=2592000",
        "content-length": "291",
        "content-type": "application/json; charset=utf-8"
    },
    "config": {
        "transitional": {
            "silentJSONParsing": true,
            "forcedJSONParsing": true,
            "clarifyTimeoutError": false
        },
        "adapter": [
            "xhr",
            "http"
        ],
        "transformRequest": [
            null
        ],
        "transformResponse": [
            null
        ],
        "timeout": 0,
        "xsrfCookieName": "XSRF-TOKEN",
        "xsrfHeaderName": "X-XSRF-TOKEN",
        "maxContentLength": -1,
        "maxBodyLength": -1,
        "env": {},
        "headers": {
            "Accept": "application/json, text/plain, */*"
        },
        "method": "get",
        "url": "https://arweave.net/sHqUBKFeS42-CMCvNqPR31yEP63qSJG3ImshfwzJJF8"
    },
    "request": {}
}

```
</details>
<hr />

每个Arweave对等体/节点还会暴露一些HTTP端点，这些端点通常是复制的网关。您可以在这里了解更多关于Arweave对等体的HTTP端点。

### 获取原始事务
`https://arweave.net/raw/TX_ID`
```js
const result = await fetch('https://arweave.net/raw/rLyni34aYMmliemI8OjqtkE_JHHbFMb24YTQHGe9geo')
  .then(res => res.json())
  console.log(JSON.stringify(result))
```

<details>
<summary><b>点击查看示例结果</b></summary>

```json
{
  "manifest": "arweave/paths",
  "version": "0.1.0",
  "index": {
    "path": "index.html"
  },
  "paths": {
    "index.html": {
      "id": "FOPrEoqqk184Bnk9KrnQ0MTZFOM1oXb0JZjJqhluv78"
    }
  }
}
```

</details>
<hr/>

### 根据字段获取
`https://arweave.net/tx/TX_ID/FIELD`

可用字段：id | last_tx | owner | target | quantity | data | reward | signature
```js
const result = await fetch('https://arweave.net/sHqUBKFeS42-CMCvNqPR31yEP63qSJG3ImshfwzJJF8/data')
  .then(res => res.json())
  console.log(JSON.stringify(result))
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
返回的余额是以Winston为单位。要获取$AR的余额，将余额除以1000000000000
`https://arweave.net/wallet/ADDRESS/balance`
```js
const res = await axios.get(`https://arweave.net/wallet/NlNd_PcajvxAkOweo7rZHJKiIJ7vW1WXt9vb6CzGmC0/balance`)
console.log(res)
console.log(res.data / 1000000000000)

6638463438702 // Winston
6.638463438702 // $AR
```

### 获取事务状态
`https://arweave.net/tx/TX_ID/status`
::: tip
此端点仅支持本地Arweave事务。必须在获得成功响应之前确认事务。
:::

```js
  const result = await fetch('https://arweave.net/tx/EiRSQExb5HvSynpn0S7_dDnwcws1AJMxoYx4x7nWoho/status').then(res => res.json())
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