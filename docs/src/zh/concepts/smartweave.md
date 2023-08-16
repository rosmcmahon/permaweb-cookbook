---
locale: zh
---
# SmartWeave

## 什么是SmartWeave？

SmartWeave是Arweave上占主导地位的SmartContract范例的名称。SmartWeave合约的一个独特特性是通过“惰性评估”过程提供的当前合约状态。这意味着，与Arweave挖矿节点不断评估所有合约的当前状态不同，阅读合约的客户端会自行评估状态。

## 为什么SmartWeave很重要？

去中心化应用程序的状态和逻辑需要与其余数据一样具有抗审查、永久性和可验证性。SmartWeave使开发人员能够编写智能合约，将其应用的状态和逻辑封装在链上并以无需信任的可验证方式执行。这并非易事，因为Arweave协议并未为节点评估网络上的智能合约状态提供激励机制。

SmartWeave为合约交互提供了一个不可变的追加模式，利用永久性存储保存其状态。结果就是一个完全去中心化的链上状态机，可以以无需许可和无需信任的方式为协议和应用程序提供动态功能。通过使用SmartWeave，开发人员可以创建存储在Arweave上并保证不会随时间改变的智能合约。这使得他们能够以无需许可和无需信任的方式构建[Permaweb应用程序](/concepts/permawebApplications.md)，并具备动态功能。

开发人员选择使用SmartWeave实现其Permaweb应用程序的逻辑可能有以下几个原因：

- **去中心化存储：** SmartWeave基于Arweave构建，这意味着使用SmartWeave创建的应用程序将存储在分布式节点网络上，而不是集中服务器上。这使得它们更抵御审查、篡改和其他形式的干扰。

- **惰性评估：** SmartWeave合约的惰性评估功能允许高效和可扩展的执行。它不同于Arweave节点不断评估合约状态，而是由读取合约的客户端负责评估状态，利用用户的处理能力而不是网络节点。

- **语言支持：** SmartWeave支持多种编程语言，包括JavaScript、TypeScript、Rust、Go、AssemblyScript和WASM（WebAssembly）。这使得开发人员在创建SmartWeave应用程序时可以使用他们最熟悉的语言。

- **数据耐久性：** Arweave的设计目标是以一种使数据高度耐用且持久的方式存储数据。这对于需要长期存储数据的应用程序（如历史记录或科学数据）非常有用。

- **经济模型：** Arweave采用基于永久存储概念的独特经济模型，激励矿工无限期存储数据。这有助于确保使用SmartWeave创建的Permaweb应用程序的长期可行性和耐久性。

## SmartWeave如何工作？

SmartWeave合约本质上由初始合约状态构建，使用事务标签进行编辑、添加和删除。

SmartWeave SDK（例如`Warp`，之前称为`RedStone`）用于查询这些事务以在本地构建合约状态，并在每个事务上修改合约状态。评估器（`Warp`）使用标签查询合约的事务；通过`App-Name`标签和`Contract`标签，它知道事务是合约的一部分。

以下是合约**交互**的示例：
- `App-Name`表示这是一个SmartWeave**操作**。
- `Contract`标签提供了初始合约状态的特定事务ID。
- `Input`标签提供了合约的执行函数和其他所需数据：

```json
[
    {
        name:"App-Name"
        value:"SmartWeaveAction"
    },
    {
        name:"App-Version"
        value:"0.3.0"
    },
    {
        name:"Contract"
        value:"pyM5amizQRN2VlcVBVaC7QzlguUB0p3O3xx9JmbNW48"
    },
    {
        name:"Input"
        value:"{
            "function":"setRecord",
            "subDomain": "@",
            "transactionId":"lfaFgcoBT8auBrFJepLV1hyiUjtlKwVwn5MTjPnTDcs"
        }"
    }
]
```
以下是一个**合约**的示例：
- `App-Name`表示这是一个SmartWeave**合约**。
- `Contract-Src`标签指向合约的源代码：

```json
[
    {
        key:"App-Name"
        value:"SmartWeaveContract"
    },
    {
        key:"App-Version"
        value:"0.3.0"
    },
    {
        key:"Contract-Src"
        value:"JIIB01pRbNK2-UyNxwQK-6eknrjENMTpTvQmB8ZDzQg"
    },
    {
        key:"SDK"
        value:"RedStone"
    },
    {
        key:"Content-Type"
        value:"application/json"
    }
]
```

生成的状态是当前合约状态，客户端端的SDK可以使用它来计算用户余额、合约所有者和其他合约特定细节。一旦调用方具有经过验证的合约状态，他们可以构建一个与用户进行交互、部署到链上的交互，该交互在下次有人构建合约状态时将被包括在[Gateway](/concepts/gateways.md)上进行挖掘或索引。

## SmartWeave生态系统项目

有许多生态系统项目利用SmartWeave智能合约，以下是其中一些值得注意的项目：

### 实现
- [Warp](https://warp.cc/) | 提供SmartWeave SDK、教程，并帮助维护SmartWeave协议的主要提供者。
- [EXM](https://docs.exm.dev/) | Execution Machine（EXM）是一个开发者平台，用于在分散环境中创建和使用高可用性、高性能的应用程序。

### 工具
- [SonAr](https://sonar.warp.cc/#/app/contracts)| SmartWeave合约浏览器，由Warp创建和托管。

### 应用程序
- [Permapages](https://permapages.app/) | 永久网页创建工具，ArNS购买门户，以及ANT创建门户。您在永久网络上的个人资料。
- [ArNS](arns.md) | Arweave命名系统<!-- // todo: 更新为arns门户，当门户发布时 -->
- [WeaveDB](https://weavedb.dev/) | 作为智能合约的NoSQL数据库。
- [KwilDB](https://docs.kwil.com/) | 作为智能合约的SQL数据库。
- [ArDrive Inferno](https://ardrive.io/inferno/) | 通过Ardrive上传文件的PST获取。