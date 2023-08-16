---
locale: zh
---
# 创建 React 应用程序起始包

本指南将逐步指导您配置开发环境，以构建和部署一个永久网络 React 应用程序。

## 先决条件

- 基本的 TypeScript 知识（非强制性）- [https://www.typescriptlang.org/docs/](学习 TypeScript)
- NodeJS v16.15.0 或更高版本- [https://nodejs.org/en/download/](下载 NodeJS)
- ReactJS 的了解- [https://reactjs.org/](学习 ReactJS)
- 熟悉 git 和常见的终端命令

## 开发依赖项

- TypeScript
- NPM 或 Yarn 包管理器

## 步骤

### 创建项目

如果您对 TypeScript 不熟悉，可以排除额外的检查`--template typescript`

<CodeGroup>
  <CodeGroupItem title="NPM">
  
```console:no-line-numbers
npx create-react-app permaweb-create-react-app --template typescript
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn create react-app permaweb-create-react-app --template typescript
```

  </CodeGroupItem>
</CodeGroup>

### 进入项目目录

```sh
cd permaweb-create-react-app
```


### 安装 react-router-dom

您必须安装此软件包以管理不同页面之间的路由

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

现在我们需要检查一切是否正常工作，然后再进行下一步，运行以下命令
<CodeGroup>
<CodeGroupItem title="NPM">

```console:no-line-numbers
npm start
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
yarn start
```

  </CodeGroupItem>
</CodeGroup>
这将在您的计算机本地启动一个新的开发服务器。默认情况下，它使用 `PORT 3000`，如果此端口已被使用，它可能会要求您切换到终端中的另一个可用端口


### 修改 package.json 文件，包含以下配置

```json
{
  ...
  "homepage": ".",
}
```


### 设置路由

现在修改应用程序并添加新的路由，比如关于页面，首先创建另外两个 `.tsx` 文件。（如果您已经排除了额外的检查 `--template typescript`，那么您的组件文件扩展名应该是 `.jsx` 或 `.js`）

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
      欢迎来到 Permaweb！
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
        <div>首页</div>
      </Link>
    </div>
  );
}

export default About;
```

#### 修改 App.tsx

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
请注意，我们在 HashRouter 中包装路由，并使用 react-router-dom Link 组件构建链接。
这在当前状态的永久网络上很重要，它将确保路由正常工作，因为应用程序在 `https://[gateway]/[TX]` 这样的路径上提供服务
:::

## 永久部署

### 生成钱包

我们需要 `arweave` 软件包来生成钱包

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

然后在终端中运行以下命令

```sh
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

### 设置 Bundlr

我们需要 Bundlr 来将应用程序部署到 Permaweb 上，它提供了即时的数据上传和检索

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
您需要将 AR 添加到此钱包中，并为您的 bundlr 钱包添加资金，以便能够上传此应用程序。请参阅 [https://bundlr.network](https://bundlr.network) 和 [https://www.arweave.org/](https://www.arweave.org/) 了解更多信息。
:::

### 更新 package.json

```json
{
  ...
  "scripts": {
    ...
    "deploy": "bundlr upload-dir ./build -h https://node2.bundlr.network --wallet ./wallet.json -c arweave --index-file index.html --no-confirmation"
  }
  ...
}
```

### 运行 build

现在是生成构建的时候了，运行

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

### 运行 deploy
最后我们可以部署我们的第一个 Permaweb 应用程序

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
现在您应该在Permaweb 上拥有一个 React 应用程序！干得漂亮！
:::

::: info 错误
如果您收到此错误 `Not enough funds to send data`，您需要向您的 Bundlr 钱包充入一些 AR，并尝试再次部署，运行以下命令
:::

<CodeGroup>
  <CodeGroupItem title="NPM">
  
```console:no-line-numbers
bundlr fund 1479016 -h https://node1.bundlr.network -w wallet.json -c arweave
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">
  
```console:no-line-numbers
bundlr fund 1479016 -h https://node1.bundlr.network -w wallet.json -c arweave
```

  </CodeGroupItem>
</CodeGroup>

::: info
上述数字 1479016 是以 winston 表示的 AR 金额，AR 的最小单位。这将需要一些时间才能传播到您的 Bundlr 钱包。请等待10-20分钟后再尝试运行部署。
:::

## 仓库

本示例的完成版本可在此处找到：[https://github.com/VinceJuliano/permaweb-create-react-app](https://github.com/VinceJuliano/permaweb-create-react-app)

## 总结

这是一个基于 Create React App 的版本，用于将 React 应用程序发布到 Permaweb 上。您可以发现在 Permaweb 上部署应用程序的新方法，或者在本指南中查看其他起始包！