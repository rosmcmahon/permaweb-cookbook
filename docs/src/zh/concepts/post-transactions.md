---
locale: zh
---
# 发布交易

有几种方法可以将交易发布到Arweave。每种方法都有自己独特的功能和约束。下图说明了四种主要的发布交易方法。

`直接到Peer`，`直接到网关`，`打包`和`派遣`。

<img src="https://arweave.net/Z1eDDnz4kqxAkkzy6p5elMz-jKnlaVIletp-Tm6W8kQ" width="550">

::: tip <img src="https://arweave.net/blzzObMx8QvyrPTdLPGV3m-NsnJ-QqBzvQIQzzZEfIk" width="20"> 保证交易
在发布大量交易或需要快速结算的情况下，考虑使用打包服务。打包服务可以立即结算大量交易，并在毫秒级时间内提供交易数据。打包服务将发布的交易保留到它们在链上得到确认为止。如果交易未包含在最新区块中，打包服务将在每个新区块中重新发布交易，直到它们在链上以足够数量的确认被记录下来。
:::

## 直接交易

直接发布到Arweave的交易有两种类型：`钱包到钱包`交易和`数据`交易。第一种在钱包地址之间转移**AR**代币。第二种将数据发布到Arweave并支付相关的存储费用。

有趣的是，“数据”交易也可以同时将**AR**代币转移到一个钱包地址，并同时支付存储费用。

所有的交易都允许用户通过[自定义标签](./tags.md)的形式指定最多2KB的元数据。

### 直接到Peer

交易可以直接发布到Arweave的一个peer（挖矿节点）。这可能是发布交易的最分散的方法，因为客户端可以选择希望发布交易的peer。

这种方法并不没有缺点。Peer节点可能会不断更换，这使得从应用程序可靠地发布事务变得困难。虽然可能在发布之前查询一个活动peer列表并选择一个，但这会给整个过程带来额外的负担和摩擦。此外，发布到peer的交易只能在被添加到区块后通过网关进行查询。这在交易发布到peer后到在浏览器从网关读取之间引入了1-2分钟的延迟。

基于以上原因，开发人员倾向于在发布直接交易时将`arweave-js`配置为指向一个网关，因为网关的乐观缓存使得交易几乎立即可用。

### 直接到网关

网关位于客户端和Arweave的peer网络之间。网关的主要功能之一是索引交易，并在等待交易被包含到区块中时乐观地缓存已发布到网络的数据。这使得在"待处理"状态下交易可以几乎立即进行查询，从而使得构建在网关之上的应用更具有响应性。但如果交易未被peer包含在区块中，它们有可能从乐观缓存中消失。

使用`arweave-js`发布直接交易的示例可以在[本指南](../guides/posting-transactions/arweave-js.md)中找到。

## 打包交易

构建在Arweave上的附加工具服务有时被称为永久网服务（Permaweb Services）。打包器是其中的一种服务。打包器将多个单独的交易捆绑到一个直接发布到Arweave的单个交易中。通过这种方式，协议级别上的单个交易可以包含数以万计的捆绑交易。然而，有一个限制，只有**数据**交易才能包含在一个打包器中。**钱包到钱包**交易（在钱包地址之间转移**AR**代币）必须作为单独的交易直接发布到Arweave。

当使用类似[bundlr.network](https://bundlr.network)的打包服务时，您必须在发布交易前在打包器节点上进行一笔小额存款。这使得打包服务能够收费并承担许多小（或大）的上传，而每次上传都无需直接在Arweave上结算**AR**代币。

[bundlr.network](https://bundlr.network)允许客户在许多[支持的加密货币](https://docs.bundlr.network/docs/currencies)中进行存款。

::: info
当交易发布到bundlr.network时，它们也会出现在连接的网关的乐观缓存中，因此可以在几毫秒内查询到。
:::

使用`bundlr.network/client`发布打包交易的示例可以在[本指南](../guides/posting-transactions/bundlr.md)中找到。

## 派遣交易

发布打包交易的另一种方法是通过浏览器。尽管浏览器对可上传数据的大小执行了一些限制，但基于浏览器的钱包仍然能够将交易发布到打包器。Arweave浏览器钱包实现了一个`dispatch()` API方法。如果您要发布小型交易（100KB或更小），即使您的应用程序未捆绑`bundlr.network/client`，也可以使用钱包的`dispatch()`方法来利用打包交易的优势。

使用Arweave钱包的`dispatch()`方法发布大小为100KB或更小的打包交易的示例可以在[本指南](../guides/posting-transactions/dispatch.md)中找到。

## 资源

- [arweave-js](../guides/posting-transactions/arweave-js.md)示例
- [bundlr.network](../guides/posting-transactions/bundlr.md)示例
- [派遣](../guides/posting-transactions/dispatch.md)示例