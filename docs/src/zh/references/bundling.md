---
locale: zh
---
# 打包

在参考以下内容之前，请确保您已经阅读了[Bundle和Bundling](/concepts/bundles.md)中的[核心概念](/concepts/)部分。

## 设置

我们将使用[arbundles](https://github.com/bundlr-Network/arbundles)库，这是一个JavaScript实现的[ANS-104规范](https://github.com/ArweaveTeam/arweave-standards/blob/master/ans/ANS-104.md)。ArBundles还支持TypeScript。

**注意：**此参考假定您正在使用NodeJS环境。虽然在浏览器上也可以兼容ArBundles，但目前需要使用`Buffer` polyfills。这个问题将在未来版本的ArBundles中得到解决。

<CodeGroup>
  <CodeGroupItem title="NPM">

```console
npm install arbundles
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

```console
yarn add arbundles
```

  </CodeGroupItem>
</CodeGroup>

## 创建`Signer`

为了创建数据项(Data Item)，我们需要先创建一个`Signer`。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
import { ArweaveSigner, JWKInterface } from 'arbundles'

const jwk: JWKInterface = { /* 您的Arweave jwk密钥文件 */ }
const signer = new ArweaveSigner(jwk)
```

  </CodeGroupItem>
</CodeGroup>

## 创建`DataItem`

要创建一个`DataItem`，我们需要将一些数据和一个`Signer`一起传递给`createData()`实用函数。

**注意：**虽然`createData()`实用函数需要一个`Signer`，但返回的`DataItem`**尚未签名**，其中包含一个占位符ID。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
import { createData } from 'arbundles'

// 使用字符串创建DataItem
const myStringData: string = 'Hello Permaweb!'
const myDataItem = createData(myStringData, signer)

// 使用Buffer或Uint8Array创建DataItem
const myBufferData: Buffer | Uint8Array = Buffer.from('Hello Permaweb!')
const myOtherDataItem = createData(myBufferData, signer)

/* !!!警告!!! DATA ITEM 尚未签名! */
```

  </CodeGroupItem>
</CodeGroup>

## 创建`Bundle`

要创建一个Bundle，我们将我们的`DataItem`传递给`bundleAndSignData`实用函数，并等待结果。

**注意：**传递给此实用函数的`DataItem`可以在后面的部分中按照详细说明进行预签名。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
import { bundleAndSignData } from 'arbundles'

const dataItems = [ myDataItem, myOtherDataItem ]
const bundle = await bundleAndSignData(dataItems, signer)
```

  </CodeGroupItem>
</CodeGroup>

## 从`Bundle`创建`Transaction`

为了将一个Bundle发布到Arweave，最终需要有一个包含Bundle的根Layer 1 `Transaction`。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
import Arweave from 'Arweave'

// 设置一个Arweave客户端
const arweave = new Arweave({
  protocol: 'https',
  host: 'arweave.net',
  port: 443
})

// 使用ArweaveJS进行创建
const tx = await arweave.createTransaction({ data: bundle.getRaw() }, jwk)

// 或者根据Bundle本身创建
const tx = await bundle.toTransaction({}, arweave, jwk)

// 签名交易
await arweave.transactions.sign(tx, jwk)

// 根据您的首选方法将tx发布到Arweave!
```

  </CodeGroupItem>
</CodeGroup>

## 签名`DataItem`

为了获取`DataItem`的ID（例如在同一个bundle中包含的manifest中使用），我们必须调用并等待其`.sign()`方法。如果签名成功，`DataItem`现在将具有其唯一ID和签名，并且准备添加到`Bundle`中。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
await myDataItem.sign(signer)
await myOtherDataItem.sign(signer)

const id1 = myDataItem.id
const id2 = myOtherDataItem.id
```

  </CodeGroupItem>
</CodeGroup>

## 为`DataItem`添加标签

`DataItem`本身可以像Layer 1的Arweave交易一样有标签。一旦Arweave Gateway取消打包并索引了`Bundle`，这些`DataItem`的标签就可以像Layer 1的Arweave交易标签一样进行检索。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
const myStringData: string = 'Hello Permaweb!'
const tags = [
  { name: 'Title', value: 'Hello Permaweb' },
  { name: 'Content-Type', value: 'text/plain' }
]
const myDataItem = createData(myStringData, signer, { tags })
```

  </CodeGroupItem>
</CodeGroup>

## 使用Bundle

**警告：**确保您传递给`new Bundle(buffer)`的`Buffer`包含了一个`Bundle`，否则，传递给它的非常小的`Buffer`将会导致线程崩溃。**请勿**在生产环境中使用`new Bundle(buffer)`。而是查看ArBundles存储库中的[可流式接口](https://github.com/Bundlr-Network/arbundles/blob/master/src/stream)。

<CodeGroup>
  <CodeGroupItem title="TS">

```ts:no-line-numbers
const bundle = new Bundle(Buffer.from(tx.data))
const myDataItem = bundle.get(0)
const myOtherDataItem = bundle.get(1)
```

  </CodeGroupItem>
</CodeGroup>