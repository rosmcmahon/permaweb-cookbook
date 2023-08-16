---
locale: zh
---
使用arweave-js发布交易
通过`arweave-js`包，可以直接将Arweave原生交易发布到节点或网关。

::: info
Arweave通过使用交易束来进行扩展。这些束使得每个区块能够包含几乎无限数量的交易。如果不使用交易束，Arweave区块每个区块限制1000个交易（每2分钟产生一个新区块）。如果您的用例超过这个容量，可能会出现交易丢失的情况。在这种情况下，请考虑使用[bundlr.network](./bundlr.md)或类似的服务来捆绑您的交易。
:::

## 安装arweave-js包

要安装`arweave-js`，运行以下命令
<CodeGroup>
  <CodeGroupItem title="NPM">

```console:no-line-numbers
npm install --save arweave
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

```console:no-line-numbers
yarn add arweave
```

  </CodeGroupItem>
</CodeGroup>

## 初始化arweave-js
使用`arweave-js`库来发布直接的第一层交易。

```js:no-line-numbers
import Arweave from 'arweave';
import fs from "fs";

// 从磁盘加载JWK钱包密钥文件
let key = JSON.parse(fs.readFileSync("walletFile.txt").toString());

// 初始化arweave实例
const arweave = Arweave.init({});
```

## 发布钱包到钱包的交易
一个基本的交易，将AR代币从一个钱包地址转移到另一个钱包地址。
```js:no-line-numbers
// 创建一个向目标地址发送10.5AR的钱包到钱包交易
let transaction = await arweave.createTransaction({
  target: '1seRanklLU_1VTGkEk7P0xAwMJfA7owA1JHW5KyZKlY',
  quantity: arweave.ar.arToWinston('10.5')
}, key);

// 在发布之前，必须用您的密钥对交易进行签名
await arweave.transactions.sign(transaction, key);

// 发布交易
const response = await arweave.transactions.post(transaction);
```

## 发布数据交易
该示例演示了如何从磁盘加载文件并创建一个交易来将其数据存储在网络上。您可以在[https://ar-fees.arweave.dev](https://ar-fees.arweave.dev)找到网络当前的收费价格。
```js:no-line-numbers
// 从磁盘加载数据
const imageData = fs.readFileSync(`iamges/myImage.png`);

// 创建一个数据交易
let transaction = await arweave.createTransaction({
  data: imageData
}, key);

// 添加一个自定义标签，告诉网关如何将此数据提供给浏览器
transaction.addTag('Content-Type', 'image/png');

// 在发布之前，必须用您的密钥对交易进行签名
await arweave.transactions.sign(transaction, key);

// 创建一个上传器，将您的数据种子到网络中
let uploader = await arweave.transactions.getUploader(transaction);

// 运行上传器直到完成上传。
while (!uploader.isComplete) {
  await uploader.uploadChunk();
}
```
## 资源
* 要了解所有您可以发布交易的方法，请参阅食谱中的[发布交易](../../concepts/post-transactions.md)部分。

* 要详细了解`arweave-js`的所有功能，请参阅github上的文档[https://github.com/ArweaveTeam/arweave-js](https://github.com/ArweaveTeam/arweave-js)