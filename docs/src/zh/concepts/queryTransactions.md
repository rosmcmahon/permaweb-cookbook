---
locale: zh
---
# 查询交易
仅仅将数据永久存储是不够的，对于Arweave来说，数据还需要被发现和检索才能发挥其作用。本指南总结了在Arweave上查询数据的不同方法。

## GraphQL
随着时间的推移，实现GraphQL接口的索引服务已成为在Arweave上查询交易数据的首选方法。索引服务会读取随着网络（通常来自服务操作的完整的Arweave节点）添加的事务和区块头信息。读取完成后，头信息将被插入到数据库中，然后可以进行索引和高效查询。索引服务使用该数据库提供GraphQL终端点供客户端查询。

GraphQL具有一些优势，使其成为检索查询数据集的理想方法。它使索引服务能够创建一个单一的终端点，然后用于查询所有类型的数据。与通过REST API为每个资源进行HTTP请求不同，该服务可以在单个请求中返回多个资源。通过GraphQL，客户端可以将多个请求批量处理为单个往返，并指定所需的确切数据，从而提高性能。

以下是一个GraphQL示例，它查询特定所有者钱包地址中具有"Type"标签且值为"manifest"的所有交易ID。有关标签的更多信息，请阅读关于[事务标签](tags.md)的指南。

```js:no-line-numbers
const queryObject = {
	query:
	`{
		transactions (
			owners:["${address}"],
			tags: [
			  {
					name: "Type",
					values: ["manifest"]
				}
			]
		) {
			edges {
				node {
					id
				}
			}
		}
	}`
};
const results = await arweave.api.post('/graphql', queryObject);
```

### 公共索引服务
[https://arweave.net/graphql](https://arweave.net/graphql)

[https://arweave-search.goldsky.com/graphql](https://arweave-search.goldsky.com/graphql)

## 检查区块
上传到Arweave的每个数据块都有其自己独特的事务ID，并包含在一个独特的区块中，然后添加到区块链中。与每个事务相关的数据被分割成256KB的块，并按顺序附加到Arweave的数据集中。可以回退，逐个区块地从[当前区块](https://arweave.net/block/current)开始检查，以查找所需的事务ID。找到后，可以从该区块中检索到块的偏移量，并用于直接从Arweave节点请求块。这是在网络上定位和读取数据的最低级方式。值得庆幸的是，还有其他减少工作量的方法，例如[GraphQL](#graphql)。

## ARQL
::: warning
ARQL已弃用，被网关或索引服务上的GraphQL查询所取代。一些节点可能仍会支持ARQL请求，但无法保证结果的可用性和准确性。
:::
Arweave查询语言（ARQL）是Arweave早期开发中使用的方法。除了区块和数据块之外，节点还维护了一个SQL数据库，其中索引了个别的事务。客户端可以使用ARQL查询节点并获取交易数据。以下是一个示例ARQL查询语法。

```js:no-line-numbers
let get_mail_query =
	{
		op: 'and',
		expr1: {
			op: 'equals',
			expr1: 'to',
			expr2: address
		},
		expr2: {
			op: 'equals',
			expr1: 'App-Name',
			expr2: 'permamail'
		}
	}

const res = await this.arweave.api.post(`arql`, get_mail_query)
```
这种查询方法足以在数据集很小且易于索引时使用。随着Arweave的采用加速，索引数据集并响应ARQL查询导致计算成本不断增加。随着挖矿的竞争越来越激烈，节点越来越难以负担得起提供ARQL服务。这最终促使了索引服务和如今在Arweave上常见的[GraphQL查询](#graphql)。

然而，仍然有一种方式可以直接从节点查询数据。[永久网络（P3）支付协议](https://arweave.net/UoDCeYYmamvnc0mrElUxr5rMKUYRaujo9nmci206WjQ)是由社区开发的一项规范，可以让客户端付费获取服务。使用P3，希望提供索引服务的节点可以收取费用以盈利。

## 资源
* [查询Arweave指南](../guides/querying-arweave/queryingArweave.md)
* [ArDB套件](../guides/querying-arweave/ardb.md)
* [ar-gql套件](../guides/querying-arweave/ar-gql.md)
* [GraphQL参考](../references/gql.md)