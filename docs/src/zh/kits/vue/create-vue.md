---
locale: zh
---
# 创建Vue起始工具包

本指南将提供逐步说明，以配置您的开发环境并构建永久网络（Permaweb）Vue应用程序。

## 先决条件

- 基础TypeScript知识（非强制性）- [了解TypeScript](https://www.typescriptlang.org/docs/)
- NodeJS v16.15.0或更高版本- [下载NodeJS](https://nodejs.org/en/download/)
- 了解Vue.js（最好是Vue 3）- [学习Vue.js](https://vuejs.org/)
- 了解git和常用的终端命令

## 开发依赖关系

- TypeScript（可选）
- NPM或Yarn包管理器

## 步骤

### 创建项目

以下命令将安装并启动create-vue，Vue项目的官方脚手架工具。

<CodeGroup>
  <CodeGroupItem title="NPM">

  ```console:no-line-numbers
  npm init vue@latest
  ```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

  ```console:no-line-numbers
  yarn create vue
  ```

  </CodeGroupItem>
</CodeGroup>

在该过程中，您将被提示选择可选功能，例如TypeScript和测试支持。我建议选择使用Vue Router，其他的可以根据您的喜好选择。

```console:no-line-numbers
✔ 项目名称：… <your-project-name>
✔ 添加TypeScript？… 否 / 是
✔ 添加JSX支持？… 否 / 是
✔ 添加Vue Router以进行单页应用程序开发？… 否 / *是*
✔ 添加Pinia用于状态管理？… 否 / 是
✔ 添加Vitest进行单元测试？… 否 / 是
✔ 添加Cypress进行单元和端到端测试？… 否 / 是
✔ 添加ESLint进行代码质量？… 否 / 是
✔ 添加Prettier进行代码格式化？… 否 / 是
```

### 切换到项目目录

```sh
cd <your-project-name>
```

### 安装依赖项

<CodeGroup>
  <CodeGroupItem title="NPM">

  ```console:no-line-numbers
  npm install
  ```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

  ```console:no-line-numbers
  yarn
  ```

  </CodeGroupItem>
</CodeGroup>

### 设置路由

Vue Router是Vue.js的官方路由器，并与Vue无缝集成。为了使其适用于Permaweb，将浏览器历史路由器切换为哈希路由器，因为URL无法发送到服务器。在`src/router/index.ts`或`src/router/index.js`文件中，将`createWebHistory`更改为`createWebHashHistory`。

```ts
import { createRouter, createWebHashHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('../views/AboutView.vue')
    }
  ]
})

export default router
```

### 设置构建

在`vite.config.ts`或`vite.config.js`文件中配置构建过程。要从子路径（https://[gateway]/[TX_ID]）中提供Permaweb应用程序，请将配置文件中的`base`属性更新为`./`。

```ts
export default defineConfig({
  base: './',
  ...
})
```

### 运行应用程序

在继续之前，十分重要的是验证一切是否正常工作。进行检查以确保进展顺利。

<CodeGroup>
  <CodeGroupItem title="NPM">

  ```console:no-line-numbers
  npm run dev
  ```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

  ```console:no-line-numbers
  yarn dev
  ```

  </CodeGroupItem>
</CodeGroup>
它将在本地启动一个新的开发服务器，默认情况下它使用`端口5173`，如果此端口已被使用，它可能会增加端口号1（`端口5174`）并再次尝试。

## 部署

### 生成钱包

生成钱包需要使用`arweave`包。

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

要生成您的钱包，请在终端中运行以下命令：
```sh
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

### 安装bundlr

需要使用Bundlr将应用程序部署到Permaweb，因为它提供了即时的数据上传和检索功能。

<CodeGroup>
  <CodeGroupItem title="NPM">

  ```console:no-line-numbers
  npm install --save-dev @bundlr-network/client
  ```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

  ```console:no-line-numbers
  yarn add -D @bundlr-network/client
  ```

  </CodeGroupItem>
</CodeGroup>

::: info Arweave钱包
要上传此应用程序，您可能需要添加AR并为您的Bundlr钱包充值。有关更多信息，请访问[https://bundlr.network](https://bundlr.network)和[https://www.arweave.org/](https://www.arweave.org/)。
:::

### 更新package.json

```json
{
  ...
  "scripts": {
    ...
    "deploy": "bundlr upload-dir dist -h https://node2.bundlr.network --wallet ./wallet.json -c arweave --index-file index.html --no-confirmation"
  }
}
```

### 更新.gitignore

为了保护您的资金，保持钱包文件的私密性是很重要的。将其上传到GitHub，可能会使其公开，这可能导致您的资金泄露。为了防止这种情况发生，将`wallet.json`文件添加到您的`.gitignore`文件中。不要忘记将其保存在安全的位置。

```sh
echo "wallet.json" >> .gitignore
```

### 运行构建

现在是生成构建的时候了。

<CodeGroup>
  <CodeGroupItem title="NPM">

  ```console:no-line-numbers
  npm run build
  ```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

  ```console:no-line-numbers
  yarn build
  ```

  </CodeGroupItem>
</CodeGroup>

### 运行部署
最后我们可以部署我们的第一个Permaweb应用程序

<CodeGroup>
  <CodeGroupItem title="NPM">

  ```console:no-line-numbers
  npm run deploy
  ```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

  ```console:no-line-numbers
  yarn deploy
  ```

  </CodeGroupItem>
</CodeGroup>

::: tip 成功
现在，您应该在Permaweb上拥有一个Vue应用程序！做得好！
:::

::: warning 充值钱包
如果您的应用程序大于120 kb或收到错误消息“Not enough funds to send data”，则需要为您的Bundlr钱包充值。有关更多信息，请访问[https://bundlr.network](https://bundlr.network)。
:::

## 代码库

您可以在以下位置找到JavaScript或TypeScript中的一个完全功能示例。

- 代码库：[https://github.com/ItsAnunesS/permaweb-create-vue-starter](https://github.com/ItsAnunesS/permaweb-create-vue-starter)

## 摘要

本指南提供了一种简单的逐步方法，使用Create Vue在Permaweb上发布Vue.js应用程序。如果您需要其他功能，例如Tailwind，请考虑浏览该指南中列出的其他起始工具包，以找到适合您需求的解决方案。