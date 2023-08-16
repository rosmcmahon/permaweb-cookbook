---
locale: zh
---
# Warp（SmartWeave）SDK - Evolve

Evolve（进化）是一种功能，允许开发人员在不部署新合约的情况下更新智能合约的源代码。要使用此功能，您首先必须使用保存函数提交新的源代码。一旦更新的代码在Permaweb上得到确认，您可以使用进化函数将合约指向新的源代码ID。这样可以更新合约的行为，而无需创建新的合约实例。

## 为什么使用进化功能？

编写SmartWeave合约可能很困难，有时需要随着时间的推移增加更新或新功能。进化功能允许您对合约进行更改，而无需从头创建新的合约实例。要使用此功能，您的合约状态对象必须包含一个进化属性，该属性设置为新的合约源事务标识。这使您可以修改和改进现有的合约，而无需从头开始。

```json
{
  ...
  "evolve": "您的源代码 TX_ID"
}
```

## 将新的源代码发布到永久网上

在对现有合约进行进化之前，您需要将新的源代码发布到永久网上，可以使用`save`函数完成此操作。

```ts
import { WarpFactory } from 'warp-contracts'
import fs from 'fs'

const src = fs.readFileSync('./dist/contract.js', 'utf-8')
const jwk = JSON.parse(fs.readFileSync('./wallet.json', 'utf-8'))
const TX_ID = 'VFr3Bk-uM-motpNNkkFg4lNW1BMmSfzqsVO551Ho4hA'
const warp = WarpFactory.forMainnet()

async function main() {
  const newSrcTxId = await warp.contract(TX_ID).connect(jwk).save({src })
  console.log('NEW SRC ID', newSrcTxId)
}

main()
```

## 进化您的合约

::: warning
**验证** 确认您的新源 TX_ID，在[Sonar](https://sonar.warp.cc)上确认TX_ID。
:::

```ts
import { WarpFactory } from 'warp-contracts'
import fs from 'fs'

const src = fs.readFileSync('./dist/contract.js', 'utf-8')
const jwk = JSON.parse(fs.readFileSync('./wallet.json', 'utf-8'))
const TX_ID = 'VFr3Bk-uM-motpNNkkFg4lNW1BMmSfzqsVO551Ho4hA'
const warp = WarpFactory.forMainnet()

async function main() {
  const newSrcTxId = await warp.contract(TX_ID).connect(jwk).evolve('SRC TX ID')
  console.log(result)
}

main()

```

::: tip
值得注意的是，进化功能仅适用于未来的操作，意味着您无法使用它将新的源代码应用于合约进化之前发生的操作。
:::


## 摘要

进化是一个强大的功能，可以为您的合约提供可扩展性，但它也可能是一种**攻击**向量，因此在使用时请确保完全理解您所做的事情。以下是您的合约中进化函数的常见示例片段。

```js

export async function handle(state, action) {
  ...
  if (action.input.function === 'evolve') {
    if (action.caller === state.creator) {
      state.evolve = action.input.value 
    }
    return { state }
  }
  ...
}
```