---
locale: zh
---
# 使用bundlr.network发布交易
使用`bundlr.network/client` JavaScript包可以将交易发布到bundlr.network。通过使用交易捆绑，捆绑服务可以确保已发布的交易得到确认，并且可以支持每个区块中的许多成千上万个交易。

## 安装bundlr.network/client
要安装`bundlr.network/client`，运行以下命令：

<CodeGroup>
  <CodeGroupItem title="NPM">

```console:no-line-numbers
npm install @bundlr-network/client
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

```console:no-line-numbers
yarn add @bundlr-network/client
```

  </CodeGroupItem>
</CodeGroup>

## 初始化Bundlr Network客户端
使用bundlr和发布Layer 1和捆绑的Layer 2交易之间的不同之处在于，当使用bundlr时，您必须提前在bundlr节点上存款。此存款可以使用AR令牌或多种其他加密货币进行。另一个区别是，bundlr服务保证您的数据将到达区块链。

```js:no-line-numbers
import Bundlr from '@bundlr-network/client';
import fs from "fs";

// 从磁盘加载JWK钱包密钥文件
let key = JSON.parse(fs.readFileSync("walletFile.txt").toString());

// 初始化bundlr SDK
const bundlr = new Bundlr("http://node1.bundlr.network", "arweave", key);
```

## 发布捆绑交易

```js:no-line-numbers
// 从磁盘加载数据
const imageData = fs.readFileSync(`images/myImage.png`);

// 添加一个自定义标签，告诉网关如何向浏览器提供此数据
const tags = [
  {name: "Content-Type", value: "image/png"},
];

// 创建捆绑交易并对其进行签名
const tx = bundlr.createTransaction(imageData, { tags });
await tx.sign();

// 将交易上传到bundlr以包含在要发布的捆绑中
await tx.upload();
```
## 资源
* 要了解可以发布交易的所有方法，请参阅Cookbook的[发布交易](../../concepts/post-transactions.md)部分。

* bundlr客户端的完整文档可在[bundlr.network网站](https://docs.bundlr.network/docs/overview)上找到。

* 使用bundlr上传NFT收藏的教程和工作坊 [上传NFT收藏](https://github.com/DanMacDonald/nft-uploader)。