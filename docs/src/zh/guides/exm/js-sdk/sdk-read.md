---
locale: zh
---
# 通过执行机器 SDK 从无服务器函数中读取

从 EXM 无服务器函数中读取状态有两种方式。如 [简介](../intro.md#使用Arweave上的无服务器函数) 中所解释的那样，EXM 在缓存层存储函数的副本以快速服务应用程序，同时也将函数上传到 Arweave 以保持分散性和相关的好处。因此，函数状态可以从 EXM 的缓存层或直接从 Arweave 中读取。

1. 从 EXM 的缓存层读取：

read 调用读取存储在 EXM 缓存层上的最新状态。该层专门设计用于快速服务应用程序。它采用乐观的方法，并在接收到事务请求后立即更新函数状态。

<CodeGroup>
  <CodeGroupItem title="read.js">

```js
import { Exm } from '@execution-machine/sdk';
import { functionId } from './functionId.js';

// 初始化新的 EXM 实例
const exm = new Exm({ token: process.env.EXM_API_TOKEN });

// 从缓存层读取
const readResult = await exm.functions.read(functionId);
console.log(readResult);
```

  </CodeGroupItem>
</CodeGroup>

2. 直接从 Arweave 中读取（评估）：

evaluate 调用返回在 Arweave 上成功处理的最新状态。这个最新状态是通过[惰性求值](../intro.md#背后是怎样工作的)计算得出的，它按顺序计算初始状态和与函数交互，以得到最新状态。

<CodeGroup>
  <CodeGroupItem title="evaluate.js">

```js
import { Exm } from '@execution-machine/sdk';
import { functionId } from './functionId.js';

// 初始化新的 EXM 实例
const exm = new Exm({ token: process.env.EXM_API_TOKEN });

// 从 Arweave 评估
const evalResult = await exm.functions.evaluate(functionId);
console.log(evalResult);
```

  </CodeGroupItem>
</CodeGroup>

::: tip
仅建议用于验证目的从 Arweave 中读取。可以将 evaluate 调用返回的函数状态与缓存层返回的信息进行对比，以确保其真实性。在发布事务请求并在网络上更新之间可能会有轻微延迟。
:::