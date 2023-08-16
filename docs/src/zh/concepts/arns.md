---
locale: zh
---
# ArNS - Arweave命名系统
## 概述
Arweave命名系统（ArNS）是PermaWeb的Smartweave驱动的电话簿。

这是一个分散且抗审查的命名系统，由AR.IO网关启用，用于将友好的名称连接到PermaWeb应用程序、页面和数据。

该系统的运作方式类似于传统的DNS，用户可以在注册表中购买一个名称，DNS名称服务器将这些名称解析为IP地址。

使用ArNS，注册表是分散的、永久的，并存储在Arweave（使用Smartweave），每个AR.IO网关充当缓存和名称解析器。用户可以在ArNS注册表中注册一个名称，比如"my-name"，并设置一个指向任何Arweave交易ID的指针。AR.IO网关将将该名称解析为其自己的子域名之一，例如https://laserilla.arweave.net，并将所有请求代理到相关的Arweave交易ID。每个注册的名称还可以与其关联的其他名称进行关联，每个名称都指向一个Arweave交易ID，例如https://v1_laserilla.arweave.net，为其所有者提供更大的灵活性和控制权。

## ArNS注册表
<!-- // TODO: link to smartweave concept // -->

ArNS使用Smartweave协议管理其名称记录。每个记录或名称由用户租用，并与ANT令牌相关联。您可以将多个ArNS名称注册到单个ANT，但不能将多个ANT注册到单个ArNS名称——网关无法知道路由ID指向何处。

ArNS名称最多可以包含32个字符，包括数字[0-9]、字母[a-z]和破折号[-]。破折号不能位于末尾，例如-myname。

## ANTs（Arweave名称令牌）

ANTs是ArNS生态系统的重要组成部分——它们是拥有ArNS名称的实际关键。当您将ArNS名称注册到ANT时，ANT将成为该名称的传输方法。ArNS注册表并不关心谁拥有ANT，它只知道它属于哪个名称的ANT。

在ANTs中，您可以构建任何您希望的功能，范围包括NFT、PST、DAO或完整的应用程序。

## 亚名称

亚名称是由您的ANT（Arweave名称令牌）持有和管理的记录。这些记录可以在没有拥有ARNS名称的情况下创建和管理，并将随ANT一起传输给新所有者。同样，如果您的ArNS名称过期，并且您将您的ANT注册到新的ArNS名称，则您的所有亚名称将保持不变。

示例：您拥有oldName.arweave.net。

然后：您创建亚名称"my" - my_oldName.arweave.net。

然后：oldName.arweave.net过期了，并且您将您的ANT注册到新的ArNS名称newName.arweave.net。

现在：My_亚名称可以在newName上访问 - my_newName.arweave.net。

下面是一个ANT合约状态的示例：

```json
{
  balances:{ QGWqtJdLLgm2ehFWiiPzMaoFLD50CnGuzZIPEdoDRGQ : 1 },
  controller: "QGWqtJdLLgm2ehFWiiPzMaoFLD50CnGuzZIPEdoDRGQ",
  evolve: null,
  name: "ArDrive OG Logo",
  owner: "QGWqtJdLLgm2ehFWiiPzMaoFLD50CnGuzZIPEdoDRGQ",
  records:{
    @:{ transactionId: "xWQ7UmbP0ZHDY7OLCxJsuPCN3wSUk0jCTJvOG1etCRo" },
    undername1:{ transactionId: "usOLUmbP0ZHDY7OLCxJsuPCN3wSUk0jkdlvOG1etCRo" }
  },
  ticker:"ANT-ARDRIVE-OG-LOGO"
}
```
基本的"@"记录是ANT的初始路由ID。如果您将'my-name'注册到此ANT，并尝试通过my-name.arweave.net访问它，您将被重定向到@记录的transactionId。

如果您尝试访问undername1_my-name.arweave.net，您将获得'undername1'的transactionId。

理论上，ANT可以具有数量不限的亚名称。然而，能够提供多少个取决于与您的ArNS名称配套使用的层级。