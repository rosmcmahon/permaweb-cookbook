---
locale: zh
---
# 使用 GraphQL 查询 Arweave
Arweave 提供了一种简单的方法来查询交易并通过[标签](../concepts/tags.md)筛选它们。Arweave 的与 GraphQL 兼容的索引服务提供了用户可以向其发布 GraphQL 查询的端点，并提供了一个用于尝试查询的游乐场。

[GraphQL](https://graphql.org)是一种灵活的查询语言，服务可以使用它来为客户端构建定制的数据模式以进行查询。GraphQL 还允许客户端指定他们希望在结果中看到哪些可用数据结构的元素。

## 公共索引服务

- [arweave.net graphql](https://arweave.net/graphql) - 由[ar.io](https://ar.io)管理的原始 graphql 端点
- [goldsky 搜索服务](https://arweave-search.goldsky.com/graphql) - 一个专门用于搜索的公共服务，使用了 graphql 语法的超集，由[goldsky](https://goldsky.com)管理
- [ar.io 分散索引](https://ar-io.dev/graphql) - 一个用于索引服务的分散网络。目前正在测试阶段，可以使用 L1 交易。

## 执行 GraphQL 查询
要查询 arweave，我们需要通过支持 GraphQL 的索引服务进行访问。请选择上面列出的其中一个 GraphQL 游乐场开始！

复制并粘贴以下查询
```graphql:no-line-numbers
query {
  transactions(tags: [{
    name: "App-Name",
    values: ["PublicSquare"]
  }]) 
  {
    edges {
      node {
        id
        tags {
          name
          value
        }
      }
    }
  }
}
```

如果你对 GraphQL 不熟悉的话，一开始可能会感到有些压力，但一旦您熟悉了其结构，就很容易阅读和理解了。

```text:no-line-numbers
query { <schema type> ( <filter criteria> ) { <data structure of the results> } }
```
在示例查询中，我们复制粘贴的 `<schema type>` 是 `transactions`，但我们也可以查询 `blocks`。Arweave 的 GraphQL 模式的完整描述写在了[Arweave GraphQL 指南](https://gql-guide.arweave.dev)中。该指南将“查询结构”称为“Query 结构”，将 `transactions` 和 `blocks` 的完整数据结构定义称为“数据结构”。

当涉及到 `<data structure of the results>` 时，需要注意的是，您可以指定您感兴趣的完整数据结构的子集。例如，一个交易模式的完整数据结构在[此处列出](https://gql-guide.arweave.dev/#full-data)。

在我们的例子中，我们对与我们的筛选条件匹配的任何交易感兴趣的 `id` 和完整的 `tags` 列表。

点击游乐场中间的大“Play”按钮来运行该查询。

![image](https://arweave.net/rYfVvFVKLFmmtXmf8KeTvsG8avUXMQ4qOBBTZRHqVU0)

你会注意到我们在原始查询中指定的结果数据结构中返回了交易列表。

如果你对区块链不熟悉，这似乎有些出乎意料，我们还没有构建任何东西，为什么会有这些结果呢？事实证明，我们筛选的`“PublicSquare”: “App-Name”`标签已经使用了一段时间。

Arweave 协议的创始人 Sam Williams 几年前在[一个 github 代码片段](https://gist.github.com/samcamwilliams/811537f0a52b39057af1def9e61756b2)中提出了交易格式。此后，生态系统中的开发人员一直在其上进行建设和改进，进行实验，并使用这些标签发布交易。

回到查询 Arweave。你会注意到在 GraphQL 结果中，没有可读的帖子消息，只有标签和有关帖子的信息。

这是因为 GraphQL 索引服务只关心索引和检索交易和区块的标题数据，而不关心它们的关联数据。

要获取交易的数据，我们需要使用另一个 HTTP 端点进行查找。
```text:no-line-numbers
https://arweave.net/<transaction id>
```

复制并粘贴查询结果中的一个 id，并修改上面的链接，将 `id` 附加到链接中。它应该类似于这样...

https://arweave.net/eaUAvulzZPrdh6_cHwUYV473OhvCumqT3K7eWI8tArk

在浏览器中导航到该链接（HTTP GET）将检索到帖子的内容（存储在交易数据中）。在这个例子中，它是…
```text:no-line-numbers
Woah that's pretty cool 😎
```
（有关完整的 arweave HTTP 端点列表，请参阅[HTTP API](https://docs.arweave.org/developers/server/http-api)文档。）

## 从 JavaScript 发布查询
从 JavaScript 发布 GraphQL 查询与在游乐场中发布它并无太大区别。

首先安装 `arweave-js` 包以便轻松访问 GraphQL 端点。
```console:no-line-numbers
npm install --save arweave
```

然后输入一个稍微高级版本的上面示例查询，并 `await` 它的发布结果。

```js:no-line-numbers
import Arweave from 'arweave';

// 初始化一个 arweave 实例
const arweave = Arweave.init({});

// 创建一个查询，它选择具有特定标签的前 100 个 tx 数据
const queryObject = {
	query:
	`{
		transactions(
			first:100,
			tags: [
				{
					name: "App-Name",
					values: ["PublicSquare"]
				},
				{
					name: "Content-Type",
					values: ["text/plain"]
				}
			]
		) 
		{
			edges {
				node {
					id
					tags {
						name
						value
					}
				}
			}
		}
	}`
};
const results = await arweave.api.post('/graphql', queryObject);
```

## 多个查询
可以在单个往返到 GraphQL 端点中发布多个查询。这个示例为两个钱包地址的 `name` 交易（作为单独的查询）查询了 `name` 交易，使用了现在已过时（由 `ar-profile` 替代）但仍然是永久的 `arweave-id` 协议。
```graphql:no-line-numbers
query {
	account1: transactions(first: 1, owners:["89tR0-C1m3_sCWCoVCChg4gFYKdiH5_ZDyZpdJ2DDRw"],
		tags: [
			{
				name: "App-Name",
				values: ["arweave-id"]
			},
			{
				name: "Type",
				values: ["name"]
			}
		]
	) {
		edges {
			node {
				id
					owner {
					address
				}
			}
		}
	}
	account2: transactions(first: 1, owners:["kLx41ALBpTVpCAgymxPaooBgMyk9hsdijSF2T-lZ_Bg"],
		tags: [
			{
				name: "App-Name",
				values: ["arweave-id"]
			},
			{
				name: "Type",
				values: ["name"]
			}
		]
	) {
		edges {
			node {
				id
					owner {
					address
				}
			}
		}
	}
}
```


## 资源
* [Arweave GQL 参考](../../references/gql.md)
* [ArDB 包](./ardb.md)
* [ar-gql 包](./ar-gql.md)
* [搜索索引服务](./search-indexing-service.md)