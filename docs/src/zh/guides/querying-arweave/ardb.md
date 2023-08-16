---
locale: zh
---
# ArDB
这是一个构建在GraphQL之上的库，它使我们能够查询Arweave的交易和区块数据，无需记住GraphQL参数的名称。只需在您喜爱的代码编辑器中使用自动补全来构建查询。

## 安装
```console:no-line-numbers
yarn add ardb
```

## 示例
```js:no-line-numbers
import Arweave from 'arweave';
import ArDB from 'ardb';

// 初始化一个arweave实例
const arweave = Arweave.init({});

// arweave 是 Arweave Client 实例
const ardb = new ArDB(arweave);

// 通过ID获取单个交易
const tx = await ardb.search('transaction')
	.id('A235HBk5p4nEWfjBEGsAo56kYsmq7mCCyc5UZq5sgjY')
	.findOne();

// 获取一个交易数组，并仅包括第一个结果
const txs = await ardb.search('transactions')
	.appName('SmartWeaveAction')
	.findOne();

// 这与以下操作相同：
const txs = await ardb.search('transactions')
	.tag('App-Name', 'SmartWeaveAction')
	.limit(1)
	.find();

// 搜索特定所有者/钱包地址的多个交易
const txs = await ardb.search('transactions')
	.from('BPr7vrFduuQqqVMu_tftxsScTKUq9ke0rx4q5C9ieQU')
	.find();

// 进行下一页的结果查询...
const newTxs = await ardb.next();

// 或者您可以使用以下方法一次获取所有结果：
const txs = await ardb.search('blocks')
 .id('BkJ_h-GGIwfek-cJd-RaJrOXezAc0PmklItzzCLIF_aSk36FEjpOBuBDS27D2K_T')
 .findAll();

```

## 资源
* [ArDB NPM包](https://www.npmjs.com/package/ardb)