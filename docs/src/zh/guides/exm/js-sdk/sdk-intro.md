---
locale: zh
---
# 执行机器 SDK

JavaScript SDK 可以在 JavaScript 和 TypeScript 应用程序中使用执行机器 (EXM)。要使用 SDK，请按照以下设置步骤进行操作。

## 安装

要在项目中安装 EXM，您可以使用 `npm` 或 `yarn`。

<CodeGroup>
  <CodeGroupItem title="npm">

```bash
npm install @execution-machine/sdk
```

  </CodeGroupItem>
  <CodeGroupItem title="yarn">

```bash
yarn add @execution-machine/sdk
```

  </CodeGroupItem>
</CodeGroup>

## 导入

在使用 EXM 与项目时，必须按照以下方式导入包。

<CodeGroup>
  <CodeGroupItem title="JavaScript">

```js
import { Exm } from '@execution-machine/sdk';
```
  </CodeGroupItem>
</CodeGroup>

## 创建实例

在安装和导入 EXM 后，需要创建一个实例来与其交互。

<CodeGroup>
  <CodeGroupItem title="JavaScript">

```js
const exmInstance = new Exm({ token: 'MY_EXM_TOKEN' });
```
  </CodeGroupItem>
</CodeGroup>

## 总结

以下指南将展示如何使用 EXM JS SDK 部署无服务器函数以及如何与其交互。