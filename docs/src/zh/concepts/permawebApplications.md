---
locale: zh
---
# 永久网络应用

永久网络应用是一种在浏览器中运行的网页或网络应用程序。它之所以被称为永久网络应用，是因为它部署在Arweave上并永久保存。即使开发它的团队离开，用户仍然可以放心，永久网络应用将保持在线和可用。永久网络应用的一个巨大优势是它将其数据保存在Arweave上，这意味着它可以很容易地导入到其他可能改进当前使用的应用中。

## 什么是永久网络？

::: info 信息
想要深入了解永久网络，请查看这篇文章：[永久网络](./permaweb.md)
:::

永久网络是建立在[Arweave的永久网络服务](./permaweb.md)之上的站点、应用程序和智能合约的集合。永久网络的核心部分包括：

* 网关服务（例如arweave.net、arweave.live、ar.io）
* 捆绑服务（例如bundlr.network）
* 排序服务（例如warp.cc）
* 索引服务（例如goldsky）

<img src="https://arweave.net/ycQzutVToTtVT_vT4811ByswtZ-KjqmifNSehSb1-eg" width="700">

### 网关服务

网关服务是连接Arweave上的数据和在浏览器中显示数据之间的桥梁。网关通常提供索引服务，通过访问GraphQL端点查询Arewave的事务数据。

### 捆绑服务

捆绑服务将事务聚合到事务捆绑中，并确保这些捆绑直接发布到Arweave上。通过使用像bundlr.network这样的捆绑服务，您可以在一个Arweave区块中发布数十万个事务。

### 排序服务

排序器使得SmartWeave合约能够在Arweave网络上计算存储的业务逻辑时实现高性能。

### 索引服务
索引服务监听Arweave上的所有事务，并将它们导入到适合快速查询的索引数据库中。然后，它们会暴露一个GraphQL端点，以便永久网络应用可以对Arweave数据进行优化查询。

这些服务共同组成了永久网络服务层，并赋予开发人员在永久网络上构建完全去中心化应用程序的能力。

## 应用程序开发

使用永久网络进行应用程序开发与`单页应用程序（SPA）`开发类似，应用程序由在网页浏览器中执行的前端功能组成，并使用GraphQL（读/查询）、Bundlr（写入）和SmartWeave（去中心化计算）来构成应用程序的业务逻辑和持久性层。

![常见的永久网络应用](https://arweave.net/UjbgAk8duudDc97lOYIt7rBVtRHp2Z9F6Ua5OcvwNCk/)

通过利用现代Web应用程序框架和[路径清单](./manifests.md)规范，开发人员可以将网站和应用程序部署到永久网络上。

要了解更多关于创建和部署永久网络应用的信息，请查看您最喜欢的框架中的入门套件：

* [React](../kits/react/index.md)
* [Svelte](../kits/svelte/index.md)
* [Vue](../kits/vue/index.md)

::: tip 找不到我使用的框架？
找不到你使用的框架？为什么不做出贡献呢？[如何为这本手册做出贡献](../getting-started/contributing.md)
:::