---
locale: zh
---
# 你好世界（NodeJS）

本指南将引导您使用 `arweave-js` 和 `bundlr` 的最简单方法将数据上传到永久网络。

由于 Arweave 2.6 只允许每个区块1000个项目，直接通过网关（例如使用 `arweave-js`）发布的方式可能不常见。

## 要求

- [NodeJS](https://nodejs.org) 长期支持版本或更高版本

## 描述

在终端 / 控制台窗口中创建一个名为 `hw-nodejs` 的新文件夹。

## 设置

```sh
cd hw-nodejs
npm init -y
npm install arweave @bundlr-network/client
```

## 生成钱包

```sh
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

## 使用bundlr上传（免费交易）

```js:no-line-numbers
import Bundlr from "@bundlr-network/client";
import fs from "fs";

const jwk = JSON.parse(fs.readFileSync("wallet.json").toString());

const bundlr = new Bundlr(
  "http://node2.bundlr.network",
  "arweave",
  jwk
);

bundlr
  .upload("你好世界")
  .then((r) => console.log(`https://arweave.net/${r.id}`))
  .catch(console.log);
```

## 使用ArweaveJS上传

如果您正在运行最新版本的 `nodejs`，则此 `arweavejs` 脚本将按原样工作。对于其他版本，您可能需要使用 `--experimental-fetch` 标志。

```js:no-line-numbers
import Arweave from "arweave";
import fs from "fs";

// 从磁盘加载 JWK 钱包密钥文件
const jwk = JSON.parse(fs.readFileSync('./wallet.json').toString());

// 初始化 arweave
const arweave = Arweave.init({
  host: "arweave.net",
  port: 443,
  protocol: "https",
});

const tx = await arweave.createTransaction(
  {
    data: "你好世界！",
  },
  jwk
);

await arweave.transactions.sign(tx, jwk);

arweave.transactions.post(tx).then(console.log).catch(console.log);
console.log(`https://arweave.net/${tx.id}`);
```

## 资源

- [Bundlr SDK](https://github.com/Bundlr-Network/js-sdk)
- [Arweave JS SDK](https://github.com/ArweaveTeam/arweave-js)
- [Bundlr 文档：免费上传](https://docs.bundlr.network/FAQs/general-faq#does-bundlr-offer-free-uploads)