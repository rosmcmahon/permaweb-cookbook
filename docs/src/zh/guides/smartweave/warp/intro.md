---
locale: zh
---
# Warp（SmartWeave）SDK简介

Warp是一个流行的SmartWeave协议SDK。通过使用Warp和Bundlr，您的SmartWeave部署和交互可以变得非常快速。

## 介绍

本指南是Warp SDK和其一些API方法的简短介绍。如果您想更多了解SmartWeave合约的基本概念，请访问[核心概念: SmartWeave](/concepts/smartweave.html)。

::: 提示
您可以在[github](https://github.com/warp-contracts)上找到Warp SDK。要深入了解Warp SmartWeave，请访问[Warp网站](https://warp.cc)。
:::

要在服务器上使用SDK，您需要访问一个wallet.json文件；要在浏览器中使用SDK，您需要连接到一个支持arweave的钱包。

## 安装

要在项目中安装warp，您可以使用`npm`或`yarn`或其他npm客户端。

<CodeGroup>
  <CodeGroupItem title="NPM">

```console
npm install warp-contracts
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

```console
yarn add warp-contracts
```

  </CodeGroupItem>
</CodeGroup>

## 导入

在项目中使用Warp时，根据您的项目设置，有几种导入sdk的方式。

<CodeGroup>
  <CodeGroupItem title="Typescript">

```ts
import { WarpFactory } from 'warp-contracts'
```

  </CodeGroupItem>
  <CodeGroupItem title="ESM">

```js
import { WarpFactory } from 'warp-contracts/mjs'
```

  </CodeGroupItem>
  <CodeGroupItem title="CommonJS">

```js
const { WarpFactory } = require('warp-contracts')
```

  </CodeGroupItem>
</CodeGroup>


## 连接到环境

有几个环境可能与您进行交互，您可以使用`forXXXX`帮助程序连接到这些环境。

<CodeGroup>
  <CodeGroupItem title="Mainnet">

```ts
const warp = WarpFactory.forMainnet()
```

  </CodeGroupItem>
  <CodeGroupItem title="Testnet">

```js
const warp = WarpFactory.forTestnet()
```

  </CodeGroupItem>
  <CodeGroupItem title="Local">

```js
const warp = WarpFactory.forLocal()
```

  </CodeGroupItem>
  <CodeGroupItem title="Custom">

```js
const warp = WarpFactory.custom(
  arweave, // arweave-js
  cacheOptions, // { ...defaultCacheOptions, inMemory: true}
  environment // 'local', 'testnet', 'mainnet'
)
```

  </CodeGroupItem>
</CodeGroup>


::: 警告
当使用本地环境时，您需要在端口1984上运行arLocal。
:::


## 总结

这个简介指南旨在帮助您设置Warp。以下指南将向您展示如何使用Warp SDK部署SmartWeave合约，如何与这些合约进行交互，以及如何演进SmartWeave合约。