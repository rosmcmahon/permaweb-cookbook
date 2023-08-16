---
locale: zh
---
# arlocal
`arlocal`是一个快速设置和运行本地Arweave测试环境的工具。它允许您在类似Arweave网关的服务器上测试交易。开发者可以在模拟环境中测试他们的应用程序，然后再将其部署到Arweave网络中。

使用`arlocal`不需要$AR代币，并且交易是即时的。

## CLI
在使用arlocal CLI之前，您的机器上必须安装了node和npm。

要启动本地网关，请运行`npx arlocal`。

::: tip
通过将端口作为参数传递，您可以指定在哪个端口上运行slim网关。
`npx arlocal 8080`
:::

要隐藏日志，请在运行网关时添加`--hidelogs`标志。
`npx arlocal --hidelogs`

## Node 
通过运行以下命令将该程序包安装为开发环境的依赖项：
`yarn add arlocal -D` or `npm install arlocal --save-dev`

```js
import ArLocal from 'arlocal';

(async () => {
  const arLocal = new ArLocal();

  // 创建本地测试环境
  await arLocal.start();

  // 运行您的测试代码

  // 关闭测试环境
  await arLocal.stop();
})();
```

可以使用选项创建`ArLocal`实例
| 选项 | 描述 |
| ---- | ----------- |
| port | 使用的端口 |
| showLogs | 显示日志 |
| dbPath | 临时数据库的目录  |
| persist | 在服务器重启时保留数据

### 示例
为了使此示例正常工作，代码需要使用一个生成的测试钱包。为此，必须将`arweave`包与`arlocal`一起安装到项目中。

`yarn add arweave arlocal -D` 或 `npm install --save-dev arweave arlocal`

以下是一个基本的JavaScript测试示例，用于使用arlocal创建数据交易并将其发布到Arweave：

```js
import ArLocal from 'arlocal'
import Arweave from 'arweave'

test('test transaction', async () => {
    // 创建和启动ArLocal实例
    const arLocal = new ArLocal()
    await arLocal.start()
    // 创建本地Arweave网关
    const arweave = Arweave.init({
    host: 'localhost',
    port: 1984,
    protocol: 'http'
  })
  // 生成钱包
  const wallet = await arweave.wallets.generate()
  // 向钱包空投一定数量（以winston为单位）的代币
  await arweave.api.get(`mint/${addr}/10000000000000000`)
  // 创建mine函数
  const mine = () => arweave.api.get('mine')
  try {
    // 创建交易
    let transaction = await arweave.createTransaction({
      data: '<html><head><meta charset="UTF-8"><title>Hello world!</title></head><body></body></html>'
    }, wallet);
    // 签名并发布交易
    await arweave.transactions.sign(transaction, wallet);
    const response = await arweave.transactions.post(transaction);
    // 挖掘交易
    await mine()
    // 测试响应
  } catch(err) {
    console.error('ERROR: ', err.message)
  }
  // 关闭测试环境
  await arLocal.stop()
})
```

::: warning
L1交易的测试结果可能与L2交易的结果不同。
:::

## 资源
[arlocal文档](https://github.com/textury/arlocal)