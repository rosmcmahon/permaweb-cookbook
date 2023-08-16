---
locale: zh
---
# Warp WriteInteractions

要在 SmartWeave 合约上调用一个函数，您可以创建一个称为 SmartWeave action 的事务。这个 action 包括函数名称和 SmartWeave 合约上函数需要的输入参数。您可以使用 contract.writeInteraction 函数创建一个 SmartWeave action。

## 代码

```ts
import { WarpFactory } from 'warp-contracts'

const warp = WarpFactory.forMainnet()
const STAMP_PROTOCOL = 'FMRHYgSijiUNBrFy-XqyNNXenHsCV0ThR4lGAPO4chA'

async function doStamp() {
  const result = await warp.contract(STAMP_PROTOCOL)
    .connect('use_wallet')
    .writeInteraction({
      function: 'stamp',
      timestamp: Date.now(),
      transactionId: 'zQhANphTO0DOsaWXhExylUD5cBN3a6xWvfn5ZCpmCVY'
    })
  console.log(result)
}
```

在调用 writeInteraction 时，您需要传递输入参数，这些参数是合约期望接收的参数。

::: warning
由于 SmartWeave 合约在惰性流中评估，您无法知道您的交互是否成功执行，直到将合约评估到当前状态为止。使用 [Warp readState](./readstate.md) 来访问合约并确定交互是否成功应用。
:::

## Dry Write

`DryWrite` 允许您在当前状态上测试和验证交互，而不需要实际在永久网络上执行它。此功能允许您在本地模拟交互并确保在应用之前它将成功执行。

```ts
import { WarpFactory } from 'warp-contracts'

const warp = WarpFactory.forMainnet()
const STAMP_PROTOCOL = 'FMRHYgSijiUNBrFy-XqyNNXenHsCV0ThR4lGAPO4chA'

async function doStamp() {
  const result = await warp.contract(STAMP_PROTOCOL)
    .connect('use_wallet')
    .dryWrite({
      function: 'stamp',
      timestamp: Date.now(),
      transactionId: 'zQhANphTO0DOsaWXhExylUD5cBN3a6xWvfn5ZCpmCVY'
    })
  console.log(result)
}
```

::: warning
使用 dry writes 时需要注意的一点是，对于使用 readState 或 internalWrites 的合约，需要在本地评估整个状态。这可能导致执行过程缓慢。
:::

## 优化速度

默认情况下，writeInteractions 被提交到 Warp Sequencer，然后打包并发布到 Arweave。您可以通过禁用 Bundling 直接发布到 Arweave。

```ts
const result = await contract.writeInteraction({
  function: 'NAME_OF_YOUR_FUNCTION',
  ...
}, { disableBundling: true })
```

## 总结

SmartWeave 协议允许使用 writeInteractions 在不可变、仅追加的存储系统上修改动态数据。这些交互使得与 SmartWeave 合约进行无需信任和无需权限的通信成为可能。Warp SDK 为开发人员提供了与 SmartWeave 协议及其 writeInteractions 功能交互的用户友好的 API。

其他资源：

* Warp SDK [https://github.com/warp-contracts/warp](https://github.com/warp-contracts/warp)
* Warp 文档 [https://warp.cc](https://warp.cc)