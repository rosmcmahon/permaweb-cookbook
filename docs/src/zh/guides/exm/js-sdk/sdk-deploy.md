---
locale: zh
---
# 使用 Execution Machine SDK 部署无服务器函数

要使用 JavaScript SDK 部署无服务器函数，我们在这里创建一个脚本，告诉计算机如何将我们的函数部署到网络中。

<details>
<summary><strong>函数逻辑示例</strong></summary>

安装包之后，我们需要在项目中定义函数逻辑的文件。

<CodeGroup>
  <CodeGroupItem title="function.js">

```js
export async function handle(state, action) {
    state.counter++;
    return { state };
}
```

  </CodeGroupItem>
</CodeGroup>

定义函数的语法基于 SmartWeave 在 JavaScript 中智能合约的实现。每个函数都有一个 `state`，它是存储在其中的值的 JSON 对象，以及与这些值交互的 `actions`。

上面的函数将名称添加到用户数组中，使用以下代码行完成：

```js
state.users.push(action.input.name);
```

在部署函数时，我们初始化一个名为 `users` 的空数组，它在读取和写入调用期间帮助函数标识此状态变量（存储在函数状态中的变量）。初始化时，`state` 的样子如下：

```js
{ users: [] }
```

另外，在向函数写入数据时，我们使用名为 `name` 的键来帮助函数识别我们正在向写入操作中输入的值。处理多个值时，这些定义变得更加重要。
</details>
<br/>

一旦定义了函数逻辑并正确设置了 API 令牌（请参见[此处](../api.md)），请按以下步骤创建部署文件：

<CodeGroup>
  <CodeGroupItem title="deploy.js">

```js
import { Exm, ContractType } from '@execution-machine/sdk';
import { readFileSync, writeFileSync } from 'fs';

// 创建新的 EXM 实例
const exm = new Exm({ token: process.env.EXM_API_TOKEN });

// 获取函数源代码
const functionSource = readFileSync('function.js');

// .deploy(source, initState, contractType)
const data = await exm.functions.deploy(functionSource, { users: [] }, ContractType.JS);

// 将函数 ID 写入本地文件
writeFileSync('./functionId.js', `export const functionId = "${data.id}"`)
```

  </CodeGroupItem>
</CodeGroup>

在部署时，我们需要将函数逻辑、函数的初始状态和函数定义的编程语言作为参数传递。要进行部署，请在项目的适当目录中的命令行中运行以下命令：

```bash
node deploy.js
```

部署后，我们会收到一些数据，我们将 `functionId` 存储在本地文件中。如其名称所示，`functionId` 是一个唯一标识符，用于进一步与无服务器函数进行读取和写入等操作。

以下部分将详细介绍使用 EXM 函数进行读取和写入的过程。