---
locale: zh
---
# Warp (SmartWeave) SDK - 部署合约

SmartWeave 合约是通过向网络发送两笔交易来创建的，源交易和初始状态交易。源交易包含合约将用于确定当前状态的源代码。初始状态交易提供了一个合约标识符，用于引用以及合约应该使用作为评估当前状态的起点的初始种子数据。通过访问作为交易写入到网络的包含输入参数的操作，可以计算当前状态，这些参数将使用评估和实例化的源代码执行。Warp 合约可以使用多种不同的语言创建，并且可以使用 Warp SDK 进行评估。本指南将展示您可以部署 Warp 合约的许多不同方式。

::: tip
如果您想了解有关编写 Warp SmartWeave 合约的更多信息，请查看 Warp 学院！[https://academy.warp.cc/](https://academy.warp.cc/)
:::

从 Warp 版本1.3.0开始，您将需要一个插件来使用 Warp 部署合约。该插件将使您能够添加不同的钱包签名。

```js
import { DeployPlugin, InjectedArweaveSigner } from 'warp-contracts-plugin-deploy'
import { WarpFactory } from 'warp-contracts'

const warp = WarpFactory.forMainnet().use(new DeployPlugin())

...

function deploy(initState, src) {
  if (window.arweaveWallet) {
    await window.arweaveWallet.connect(['ACCESS_ADDRESS', 'SIGN_TRANSACTION', 'ACCESS_PUBLIC_KEY', 'SIGNATURE']);
  }
  const userSigner = new InjectedArweaveSigner(window.arweaveWallet);
  await userSigner.setPublicKey();

  return warp.deploy({
    wallet: userSigner,
    src,
    initState: JSON.stringify(initState)
  })
}
```


## 部署 Warp SmartWeave 合约的四种方式

通过 Warp SDK，您可以通过4种方式部署 SmartWeave 合约，这些选项处理开发人员可能遇到的不同用例。

* 需要同时部署合约和源代码
* 需要部署在 permaweb 上已经存在源代码的合约
* 需要通过顺序器部署合约，并使用路径清单指定一些数据
* 需要通过 Bundlr 部署合约，并在顺序器上注册该合约

::: tip
要了解有关 Warp 部署的更多信息，请查看项目的 github Readme。 [https://github.com/warp-contracts/warp#deployment](https://github.com/warp-contracts/warp#deployment)。
:::

::: warning
该项目正在快速开发中，所以这里的文档可能很快过时，如果您发现它过时了，请在[Permaweb Cookbook Discord 频道](https://discord.gg/haCAX3shxF)中告诉我们。
:::

## 示例

::: tip
默认情况下，所有部署函数都通过 Bundlr-Network 发布到 Arweave，每个选项都有一个标志，可以设置为不使用 Bundlr，但是网络需要多个确认才能完全确认交易。
:::

**deploy**

将合约和源代码部署到 Warp 顺序器，到 Bundlr (L2)，到 Arweave。

```ts
const { contractTxId, srcTxId } = await warp.deploy({
  wallet,
  initState,
  data: { 'Content-Type': 'text/html', body: '<h1>Hello World</h1>' },
  src: contractSrc,
  tags: [{"name":"AppName", "value":"HelloWorld"}],
})
```

* wallet - 应该是 Arweave 密钥文件（wallet.json），解析为实现[JWK 接口](https://rfc-editor.org/rfc/rfc7517)的 JSON 对象或字符串'use_wallet'
* initState - 是一个字符串化的 JSON 对象
* data - 如果要在部署中写入数据，此参数是可选的
* src - 合约的源代码的字符串或 Uint8Array 值
* tags - 是一个由名称/值对象`{name: string, value: string}[]`组成的数组，[了解更多关于标签的信息](../../../concepts/tags.md)

**deployFromSourceTx**

已经在 permaweb 上有源代码了吗？那么 deployFromSourceTx 就是您的选择工具！通过 permaweb，您永远不必担心数据的更改，因此重用合约的源代码是再好不过的选择。

```ts
const { contractTxId, srcTxId } = await warp.deployFromSourceTx({
  wallet,
  initState,
  srcTxId: 'SRC_TX_ID'
})
```

**deployBundled**

使用 Warp 网关顺序器的端点将原始数据项上传到 Bundlr 并对其进行索引。

```ts
import { createData } from 'arbundles'

const dataItem = createData(
  JSON.stringify({
    "manifest": "arweave/paths",
    "version": "0.1.0",
    "index": {
      "path": "index.html"
    },
    "paths": {
      "index.html": {
        "id": "cG7Hdi_iTQPoEYgQJFqJ8NMpN4KoZ-vH_j7pG4iP7NI"
      }
    }
  })
  , { tags: [{'Content-Type': 'application/x.arweave-manifest+json' }]})
const { contractTxId } = await warp.deployBundled(dataItem.getRaw());
```


**register**

使用 Warp 网关顺序器的端点对使用 Bundlr 上传的合约进行索引。

```ts
import Bundlr from '@bundlr-network/client'

const bundlr = new Bundlr('https://node2.bundlr.network', 'arweave', wallet)
const { id } = await bundlr.upload('Some Awesome Atomic Asset',  { 
  tags: [{'Content-Type': 'text/plain' }]
})
const { contractTxId } = await warp.register(id, 'node2') 
```

## 摘要

为什么有这么多部署合约的选项？这些方法的存在是为了减少重复，实现高级合约交互，并且允许进行智能合约协议的测试和使用的灵活性。 permaweb 在其架构中非常独特，它提供了一个功能，您可以部署数字数据和用于管理该数据的合约，生成相同的交易标识符。结果是动态数据与一组不可变的数据配对。部署合约只是 Warp SDK 的一个组成部分，要了解更多内容，请继续阅读本指南！