---
locale: zh
---
# 获取交易数据
虽然索引服务允许查询交易元数据，但它们不能提供对交易数据本身的访问。这是因为缓存交易数据和索引元数据具有不同的资源需求。索引服务主要依赖计算资源在数据库上执行查询，而交易数据更适合部署在内容传送网络（CDN）上以优化存储和带宽。

大多数网关都通过一组HTTP端点提供交易数据缓存服务。可以使用任何HTTP客户端/包从这些端点请求交易数据。例如，JavaScript中的Axios或Fetch，PHP中的Guzzle等。

<img src="https://ar-io.net/VZs292M6mq8LqvjLMdoHGD45qZKDnITQVAmiM9O2KSI" width="700">

如果您想绕过交易数据缓存服务并直接从Arweave节点处获取数据，虽然可以实现，但需要做很多工作！

Arweave将交易数据以256KB的连续序列形式存储，从网络的开始到当前的区块。这种格式经过了优化，以支持矿工参与SPoRA挖矿机制来证明他们存储Arweave数据。

::: info
1. 从已知的对等点检索对等点列表。
1. 向对等点请求包含您的交易数据的分块偏移。
1. 向对等点请求分块数据。
    1. 如果对等点提供了分块数据，则将其组合成原始格式。
1. （如果对等点没有分块）遍历对等点列表，向其请求分块数据。
1. 访问每个对等点时，请检查其对等点列表，并添加尚未在您的列表中的对等点。
1. 重复从步骤3开始，直到获取所有分块。
:::

每当您想要从Arweave网络中检索数据时，需要执行相当大量的工作。想象一下，如果您想要显示类似于[https://public-square.g8way.io](https://public-square.g8way.io)这样的推文时间线，用户体验将非常差，加载时间长且有旋转标记。因为Arweave上的数据是永久性的，可以安全地以其原始形式进行缓存，以使交易数据的检索更加快速和容易。

以下HTTP端点是如何访问arweave.net交易数据缓存服务中的缓存交易数据。

<hr />

### 获取缓存的交易数据
此方法从缓存中检索与指定事务ID（TX_ID）相关联的事务数据。

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

### 获取原始交易数据
某些[事务类型](manifests.md)的数据遵循不同的呈现规则，此端点将返回原始未转换的数据。

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

每个Arweave对等点/节点还暴露一些HTTP端点，这些端点通常是复制的网关。您可以在此处阅读有关Arweave对等点的HTTP端点的更多信息[/references/http-api.md](/references/http-api.md)。