---
locale: zh
---
# 交易元数据（标签）

可以将Arweave视为一个永久的追加写入硬盘，其中驱动器上的每个条目都是唯一的交易。交易具有唯一的ID、签名和拥有者地址，用于签名和支付该交易被发布的地址。除了这些头值之外，Arweave协议允许用户使用自定义标签标记交易。这些标签被指定为附加到交易的集合名值对。这些标签使得查询Arweave并找到包含特定标签或标签的所有交易变得可能。查询和筛选交易的能力对支持构建在Arweave上的应用程序至关重要。

## 什么是交易标签？

交易标签是一对键值对，其中base64URL键和值的组合必须小于2048字节的arweave本机交易的最大值。

::: tip
捆绑交易对更多的标签空间提供支持。通过bundler.network发布的交易具有多达4096字节的标签空间。
:::


一些常见的交易标签示例包括：

* `Content-Type`：用于指定永久网上渲染的内容的MIME类型。
* `App-Name`：描述写入数据的应用程序的标签。
* `App-Version`：该标签是与App-Name配对的应用程序的版本。
* `Unix-Time`：这个标签是一个Unix时间戳，自纪元以来的秒数。
* `Title`：用于为交易中存储的内容提供名称或简要描述。
* `Description`：用于提供内容的更长描述。

交易标签可以用于多种目的，例如索引交易以进行搜索、将交易组织到类别中或提供有关交易中存储的内容的元数据。

## 关于交易标签的一些重要信息

交易标签被编码为Base64URL编码的字符串，用于键和值。这使得可以安全地通过http传输字节数组作为键或值。虽然没有解码后不可读，但不应将其视为加密。

直接发布到Arweave的交易标签的最大总大小为2048字节。该大小由交易标签的所有键和所有值的串联确定。

可以在GraphQL查询中使用交易标签返回过滤后的交易项集合。

## 社区中常用的标签

| <div style="width:100px">标签名称</div>  | 描述 | 用例 |
| -------- | ----------- | --------- |
| App-Name | 最常用于SmartWeave标识符 | 常见的值包括SmartWeaveContract、SmartWeaveAction和SmartWeaveContractSource |
| App-Version | 这些数据的版本，可能代表使用这些信息的应用程序 | 0.3.0是当前的SmartWeave版本 |
| Content-Type | 用于标识交易中包含的数据的MIME类型 | text/html，application/json，image/png |
| Unix-Time | 这个标签是一个Unix时间戳，自纪元以来的秒数 | 交易提交的时间 |
| Title | ANS-110用于描述内容的标准名称 | 为Atomic Asset提供一个名称 |
| Type | ANS-110用于对数据进行分类的标准名称 | 类型可以对永久网上资产进行分类 |

## 示例

<CodeGroup>
  <CodeGroupItem title="arweave">

```ts
const tx = await arweave.createTransaction({ data: mydata })
tx.addTag('Content-Type', 'text/html')
tx.addTag('Title', '关于交易标签的精彩文章')
tx.addTag('Description', '这是一篇您不想错过的文章！')
tx.addTag('Topic:Amazing', 'Amazing')
tx.addTag('Type', 'blog-post')


await arweave.transactions.sign(tx, jwk)
await arweave.transactions.post(tx)
```

  </CodeGroupItem>
  <CodeGroupItem title="@bundlr-network/client">

```js
await bundlr.upload(mydata, [
  { name: 'Content-Type', value: 'text/html' },
  { name: 'Title', value: '关于交易标签的精彩文章' },
  { name: 'Description', value: '这是一篇您不想错过的文章！' },
  { name: 'Topic:Amazing', value: 'Amazing' },
  { name: 'Type', value: 'blog-post' }
])
```

  </CodeGroupItem>
</CodeGroup>

## 总结

了解交易标签如何影响Arweave技术栈可以为使用Permaweb作为应用程序平台解决问题提供背景。标签提供了使用常见数据标准和模式来消费和创建的工具，以鼓励Permaweb上非竞争性的数据体验。其结果使得生态系统的用户可以选择消费和创建内容的应用程序，因为他们的数据始终随用户而不是应用程序。