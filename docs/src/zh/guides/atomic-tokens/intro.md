---
locale: zh
---
# 原子代币

## 什么是原子代币？

[查看概念](../../concepts/atomic-tokens.md)

## 创建原子代币

::: info 信息
在此示例中，我们使用已经在网络上发布的SWT合约源代码。[x0ojRwrcHBmZP20Y4SY0mgusMRx-IYTjg5W8c3UFoNs](https://sonar.warp.cc/#/app/source/x0ojRwrcHBmZP20Y4SY0mgusMRx-IYTjg5W8c3UFoNs#) - 
:::

example.ts

```ts
import Bundlr from '@bundlr-network/client'
import { WarpFactory } from 'warp-contracts'

async function main() {
  const wallet = JSON.parse(await import('fs')
    .then(fs => fs.readFileSync('./wallet.json', 'utf-8')))

  const bundlr = new Bundlr('https://node2.bundlr.network', 'arweave', wallet)
  const warp = WarpFactory.forMainnet()

  const data = `<h1>Hello Permaweb!</h1>`
  const tags = [
    { name: 'Content-Type', value: 'text/html' },
    // ANS-110标签 
    { name: 'Type', value: 'web-page' },
    { name: 'Title', value: 'My first permaweb page' },
    { name: 'Description', value: 'First permaweb page by Anon' },
    { name: 'Topic:Noob', value: 'Noob' },
    // SmartWeave合约
    { name: 'App-Name', value: 'SmartWeaveContract' },
    { name: 'App-Version', value: '0.3.0' },
    { name: 'Contract-Src', value: 'x0ojRwrcHBmZP20Y4SY0mgusMRx-IYTjg5W8c3UFoNs' },
    {
      name: 'Init-State', value: JSON.stringify({
        balances: {
          'cHB6D8oNeXxbQCsKcmOyjUX3UkL8cc3FbJmzbaj3-Nc': 1000000
        },
        name: 'AtomicToken',
        ticker: 'ATOMIC-TOKEN',
        pairs: [],
        creator: 'cHB6D8oNeXxbQCsKcmOyjUX3UkL8cc3FbJmzbaj3-Nc',
        settings: [['isTradeable', true]]
      })
    }
  ]

  const { id } = await bundlr.upload(data, { tags })
  await warp.createContract.register(id, 'node2')
  console.log('原子代币：', id)
}

main()
```

在此示例中，我们正在创建一个数据项并将该项上传到组合器网络服务中。然后，我们使用Warp序列器注册我们的合约。通过使用组合器发布我们的数据项并与Warp序列器注册，我们的数据立即可在网关服务上使用，我们的合约也可以立即接受交互操作。

运行示例

```sh
npm install @bundlr-network/client warp-contracts 
npm install typescript ts-node
npx ts-node example.ts
```

::: info 信息
[ANS-110](https://github.com/ArweaveTeam/arweave-standards/blob/master/ans/ANS-110.md) 是一种资产发现规范，允许与永久网络应用生态系统相互搭配。
:::

## 总结

这是一个部署原子资产的简单示例，要了解更详细的示例，请访问：[https://atomic-assets.arweave.dev](https://atomic-assets.arweave.dev)


## 使用代币

SmartWeave合约无法持有AR，即Arweave网络的原生币。AR用于在Arweave网络上购买数据存储空间，并可在Arweave网络上的源钱包和目标钱包之间进行转移，但无法在SmartWeave合约中持有。