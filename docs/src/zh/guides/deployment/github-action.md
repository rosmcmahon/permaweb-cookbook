---
locale: zh
---
# Github动作

::: danger
此指南仅供教育目的，并且您应该使用它来学习部署应用程序的选项。在本指南中，我们信任由`microsoft`拥有的第三方资源`github`来保护我们的秘密信息，在它们的文档中，他们使用`libsodium密封盒`对存储的秘密信息进行加密，您可以在此处找到更多关于他们的安全实践的信息. https://docs.github.com/en/actions/security-guides/encrypted-secrets
:::


Github Actions是一种CI/CD流水线，允许开发人员通过生成自github工作流系统的事件来触发自动化任务。这些任务可以是任何事情，在本指南中，我们将展示如何使用github actions将您的永久Web应用程序部署到永久Web，使用bundlr和ArNS。

::: tip
此指南需要了解github actions，并且您必须拥有一些ArNS测试代币，请访问https://ar.io/arns/以获取更多详细信息。
:::

::: warning
此指南不包括测试或任何其他您可能希望添加到生产工作流的检查。
:::

## 创建部署脚本

部署脚本是执行部署应用程序的重要脚本，我们将使用`@bundlr-network/client`和`warp-contracts`来发布我们的应用程序，并将新发布的应用程序注册到ArNS上。

安装部署依赖项

```console
npm install --save-dev @bundlr-network/client
npm install --save-dev warp-contracts
npm install --save-dev arweave
```

创建 `deploy.mjs` 文件

```js
import Bundlr from '@bundlr-network/client'
import { WarpFactory, defaultCacheOptions } from 'warp-contracts'
import Arweave from 'arweave'

const ANT = '[YOUR ANT CONTRACT]'
const DEPLOY_FOLDER = './dist'
const BUNDLR_NODE = 'https://node2.bundlr.network'

const arweave = Arweave.init({ host: 'arweave.net', port: 443, protocol: 'https' })
const jwk = JSON.parse(Buffer.from(process.env.PERMAWEB_KEY, 'base64').toString('utf-8'))

const bundlr = new Bundlr.default(BUNDLR_NODE, 'arweave', jwk)
const warp = WarpFactory.custom(
  arweave,
  defaultCacheOptions,
  'mainnet'
).useArweaveGateway().build()

const contract = warp.contract(ANT).connect(jwk)
// 上传文件夹
const result = await bundlr.uploadFolder(DEPLOY_FOLDER, {
  indexFile: 'index.html'
})

// 更新 ANT
await contract.writeInteraction({
  function: 'setRecord',
  subDomain: '@',
  transactionId: result.id
})

console.log('已部署Cookbook，请等待20 - 30分钟来更新ArNS！')
```

## 将脚本添加到 package.json

创建一个名为 `deploy` 的新脚本属性，调用构建脚本，然后在脚本部署属性的值中调用 `node deploy.mjs`。

package.json

```json
  ...
  "scripts": {
    "dev": "vuepress dev src",
    "build": "vuepress build src",
    "deploy": "yarn build && node deploy.mjs"
  },
  ...
```

## 创建github action

在 `.github/workflows` 文件夹中创建一个 `deploy.yml` 文件，此文件指示github actions在`main`分支上触发推送事件时进行部署。

```yml
name: publish 

on:
  push:
    branches:
      - "main"

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v1
        with:
          node-version: 18.x
      - run: yarn
      - run: yarn deploy
        env:
          KEY: ${{ secrets.PERMAWEB_KEY }}
```

## 概要

在项目存储库中，转到设置和密钥，添加一个新的密钥到存储库中，此密钥称为PERMAWEB_KEY。密钥的值应为部署钱包的base64编码字符串。

```console
base64 -i wallet.json | pbcopy
```

为了使此部署工作，请确保为此钱包的bundlr帐户提供资金，在您将要使用的钱包中确保有一些$AR，不多，可能只需0.5个AR，然后使用bundlr cli提供资金。

```console
npx bundlr 250000000000 -h https://node2.bundlr.network -w wallet.json -c arweave
```

::: warning
保持此钱包资金较低，仅用于此项目。
:::

:tada: 您已经设置了一个github action，完全自动化了您对永久Web的部署！