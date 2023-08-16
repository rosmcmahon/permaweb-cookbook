---
locale: zh
---
# Vite入门套件

本指南将逐步引导您配置开发环境以构建和部署一个永久网络反应应用程序。

## 先决条件

- 基本的TypeScript知识（非强制）- [https://www.typescriptlang.org/docs/](学习TypeScript)
- NodeJS v16.15.0或更高版本- [https://nodejs.org/en/download/](下载NodeJS)
- 熟悉ReactJS- [https://reactjs.org/](学习ReactJS)
- 熟悉git和常见的终端命令

## 开发相关依赖

- TypeScript
- NPM或Yarn包管理器

## 步骤

### 创建项目

如果您不熟悉TypeScript，可以使用 "react" 模板 (`--template react`)

<CodeGroup>
  <CodeGroupItem title="NPM">
  
```console:no-line-numbers
npm create vite@latest my-arweave-app -- --template react-ts
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn create vite my-arweave-app --template react-ts
```

  </CodeGroupItem>
</CodeGroup>

### 进入项目目录

```sh
cd my-arweave-app
```


### 安装react-router-dom

您需要安装此软件包来管理不同页面之间的路由

<CodeGroup>
  <CodeGroupItem title="NPM">
  
```console:no-line-numbers
npm install react-router-dom --save
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn add react-router-dom -D
```

  </CodeGroupItem>
</CodeGroup>


### 运行应用程序

现在，我们需要检查一切是否正常进行，然后再进行下一步，运行
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
它将在您的本地机器上启动一个新的开发服务器，默认情况下使用 `PORT 3000`，如果此端口已经在使用中
它可能会要求您在终端中切换到另一个可用的端口


### 设置钱包类型

如果您想使用 [ArConnect](https://arconnect.io), [Arweave.app](https://arweave.app) 或其他基于浏览器的钱包，您可以安装 ArConnect 的   types 包以获得 `window.arweaveWallet` 的声明。
<CodeGroup>
<CodeGroupItem title="NPM">

```console:no-line-numbers
npm install arconnect -D
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn add arconnect -D
```

  </CodeGroupItem>
</CodeGroup>

安装完软件包之后，您需要将其添加到 `src/vite-env.d.ts` 文件中。

```ts
/// <reference types="arconnect" />
```

### 设置路由

现在修改应用程序并添加新路由，比如关于页面，首先创建2个 .tsx 文件。（如果您使用了原始 JS 的 React 模板，请确保您的组件文件扩展名应为 `.jsx` 或 `.js` ）

```sh
touch src/HomePage.tsx
touch src/About.tsx
```

#### HomePage.tsx

```ts
import { Link } from "react-router-dom";

function HomePage() {
  return (
    <div>
      欢迎来到永久网络！
      <Link to={"/about/"}>
        <div>关于</div>
      </Link>
    </div>
  );
}

export default HomePage;
```

#### About.tsx

```ts
import { Link } from "react-router-dom";

function About() {
  return (
    <div>
      欢迎来到关于页面！
      <Link to={"/"}>
        <div>主页</div>
      </Link>
    </div>
  );
}

export default About;
```

#### 修改App.tsx

我们需要更新 App.tsx 来管理不同的页面

```ts
import { HashRouter } from "react-router-dom";
import { Routes, Route } from "react-router-dom";

import HomePage from "./HomePage";
import About from "./About";

function App() {
  return (
    <HashRouter>
      <Routes>
        <Route path={"/"} element={<HomePage />} />
        <Route path={"/about/"} element={<About />} />
      </Routes>
    </HashRouter>
  );
}

export default App;
```

::: info 哈希路由
请注意，我们正在将路由包裹在 HashRouter 中，并使用 react-router-dom 的 Link 组件构建链接。
这在当前状态的永久网络中非常重要，它将确保路由正常工作，因为应用程序
以像 `https://[gateway]/[TX]` 这样的路径提供
:::

## 永久部署

### 生成钱包

我们需要 `arweave` 包来生成钱包

<CodeGroup>
<CodeGroupItem title="NPM">

```console:no-line-numbers
npm install --save arweave
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn add arweave -D
```

  </CodeGroupItem>
</CodeGroup>

然后在终端中运行此命令

```sh
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

### 设置bundlr

我们需要 Bundlr 将应用程序部署到永久商店，它提供了即时的数据上传和检索

<CodeGroup>
  <CodeGroupItem title="NPM">
  
```console:no-line-numbers
npm install --global @bundlr-network/client
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn global add @bundlr-network/client
```

  </CodeGroupItem>
</CodeGroup>

::: info
您需要向此钱包添加 AR 并为业务钱包提供资金才能上传此应用程序。有关更多信息，请参见 [https://bundlr.network](https://bundlr.network) 和 [https://www.arweave.org/](https://www.arweave.org/) 。
:::

### 更新package.json

```json
{
  ...
  "scripts": {
    ...
    "deploy": "bundlr upload-dir ./dist -h https://node2.bundlr.network --wallet ./wallet.json -c arweave --index-file index.html --no-confirmation"
  }
  ...
}
```

### 运行构建

现在到了生成构建的时候

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

最后，我们准备部署我们的第一个永久网络应用程序

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
现在，您应该在永久网络上拥有一个React应用程序！干得好！
:::

::: error
如果您收到 "Not enough funds to send data" 错误，请将一些 AR 资金存入您的钱包中，然后再次尝试部署。
:::