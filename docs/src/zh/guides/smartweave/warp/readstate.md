---
locale: zh
---
# Warp (SmartWeave) SDK - 读取状态

通过延迟评估来计算 SmartWeave 合约状态，这意味着状态评估发生在读取而不是写入时。在读取合约时，SDK 收集所有状态交互，对它们进行排序，并使用 reduce 或折叠模式对源合约执行它们。

## 基本读取状态

```ts
const warp = WarpFactory.forMainnet()
const CONTRACT_ID = '_z0ch80z_daDUFqC9jHjfOL8nekJcok4ZRkE_UesYsk'

const result = await warp.contract(CONTRACT_ID).readState()

// 记录当前状态
console.log(result.cachedValue.state)
```

## 高级读取状态

有些合约要么读取其他合约的状态，要么调用或写入其他合约的状态，当请求这些合约的状态时，需要设置评估选项。

```ts
const warp = WarpFactory.forMainnet()
const CONTRACT_ID = 'FMRHYgSijiUNBrFy-XqyNNXenHsCV0ThR4lGAPO4chA'

const result = await warp.contract(CONTRACT_ID)
  .setEvaluationOptions({
    internalWrites: true,
    allowBigInt: true
  })
  .readState()

// 记录当前状态
console.log(result.cachedValue.state)
```

### 常用评估选项

| 名称 | 描述 |
| ---- | ----------- |
| internalWrites | 评估包含对其他合约的内部写入的合约 |
| allowBigInt | 评估使用 BigInt 基元的合约，可了解更多关于 bigInt 的信息 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) |
| unsafeClient | 这个值可以是 `allow`，`skip` 或 `throw`。应避免在合约中使用 unsafeClient，它可能导致不确定的结果。 |

## 从特定的区块高度或排序键读取状态

如果您希望查看先前的状态而不是当前状态，可以通过提供 blockHeight 来读取特定区块高度下的合约状态

```ts
const { sortKey, cachedValue } = await contract.readState(1090111)
```

## 摘要

通过提取所有交互并通过折叠方法处理每个交互来读取 SmartWeave 合约的当前状态。这种方法是永久网络的独特方法，并需要对您的 SmartWeave 合约代码执行方式有独特的理解。