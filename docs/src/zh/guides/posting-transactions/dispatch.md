---
locale: zh
---
# 通过Dispatch发布交易
Arweave浏览器钱包具有分派交易的概念。如果交易大小在100KB以下，可以免费发布！
## 发布交易
这可以在客户端应用程序中不依赖于任何包依赖项的情况下完成。只要用户有一个活动的浏览器钱包，并且数据少于100KB，分派的交易是免费的，并且保证在网络上得到确认。

```js:no-line-numbers
// 使用arweave-js创建交易
let tx = await arweave.createTransaction({ data:"Hello World!" })

// 将一些自定义标签添加到交易中
tx.addTag('App-Name', 'PublicSquare')
tx.addTag('Content-Type', 'text/plain')
tx.addTag('Version', '1.0.1')
tx.addTag('Type', 'post')

// 使用浏览器钱包将交易分派()
let result = await window.arweaveWallet.dispatch(tx);

// 打印出交易ID
console.log(result.id);
```

## 资源
* 有关所有的提交交易方式的概述，请参阅[提交交易](../../concepts/post-transactions.md)部分的手册。