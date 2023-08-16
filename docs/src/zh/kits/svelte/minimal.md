---
locale: zh
---
# 最小的Svelte起始工具包

本指南将逐步指导您配置开发环境，以构建和部署永久网络应用程序。

## 先决条件

* 熟悉TypeScript
* NodeJS v18或更高版本
* 熟悉Svelte - [https://svelte.dev](https://svelte.dev)
* 熟悉git和常见的终端命令

## 开发依赖

* TypeScript
* esbuild
* w3
* yarn `npm i -g yarn`

## 步骤

### 创建项目

```sh
mkdir myproject
cd myproject
yarn init -y
yarn add -D svelte esbuild typescript esbuild-svelte tinro svelte-preprocess
```

## 创建buildscript.js

```js
import fs from "fs";
import esbuild from "esbuild";
import esbuildSvelte from "esbuild-svelte";
import sveltePreprocess from "svelte-preprocess";

//确保目录在放入内容前存在
if (!fs.existsSync("./dist/")) {
  fs.mkdirSync("./dist/");
}
esbuild
  .build({
    entryPoints: [`./src/main.ts`],
    bundle: true,
    outdir: `./dist`,
    mainFields: ["svelte", "browser", "module", "main"],
    // logLevel: `info`,
    splitting: true,
    write: true,
    format: `esm`,
    plugins: [
      esbuildSvelte({
        preprocess: sveltePreprocess(),
      }),
    ],
  })
  .catch((error, location) => {
    console.warn(`Errors: `, error, location);
    process.exit(1);
  });

//使用基本的html文件进行测试
fs.copyFileSync("./index.html", "./dist/index.html");

```

## 修改package.json

将`type`设置为`module`
添加一个构建脚本

```json
{
  "type": "module"
  ...
  "scripts": {
    "build": "node buildscript.js"
  }
}
```

## 创建`src`目录和一些src文件

```sh
mkdir src
touch src/main.ts
touch src/app.svelte
touch src/counter.svelte
touch src/about.svelte
```

## Main.ts

```ts
import App from './app.svelte'

new App({
  target: document.body
})
```

## app.svelte

```html
<script lang="ts">
import { Route, router } from 'tinro'
import Counter from './counter.svelte'
import About from './about.svelte'

//添加基于哈希的路由，用于永久网络支持
router.mode.hash()

</script>
<nav>
<a href="/">首页</a> | <a href="/about">关于</a>
</nav>
<Route path="/"><Counter /></Route>
<Route path="/about"><About /></Route>
```

::: info 哈希路由
您会在脚本部分中注意到`router.mode.hash()`的设置，这是配置您的应用程序以使用基于哈希的路由的重要步骤，这将在以路径形式运行该应用程序时启用URL支持，例如`https://[gateway]/[TX]`
:::

## counter.svelte

```html
<script lang="ts">
let count = 0

function inc() {
  count += 1
}
</script>
<h1>您好，永久网络</h1>
<button on:click={inc}>增加</button>
<p>计数：{count}</p>
```

## about.svelte

```html
<h1>关于页面</h1>
<p>最少量的关于页面</p>
<a href="/">首页</a>
```

## 部署

### 生成钱包

```sh
yarn add -D arweave
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

### 安装bundlr

```sh
yarn add -D @bundlr-network/client
```

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

### 运行部署

```sh
yarn deploy
```

::: tip 成功
您现在在Permaweb上有一个Svelte应用程序！做得很好！
:::

::: warning 充值钱包
如果您的应用程序大于120 KB，则需要为bundlr钱包充值。有关更多信息，请参阅[https://bundlr.network](https://bundlr.network)。
:::

## 存储库

此示例的完成版本可在此处找到：[https://github.com/twilson63/permaweb-minimal-svelte-starter](https://github.com/twilson63/permaweb-minimal-svelte-starter)

## 总结

这是在永久网络上发布Svelte应用程序的最小版本，但您可能需要更多功能，例如热重载和tailwind等等。请查看`hypar`以获取一个即插即用的起始工具包。[HypAR](https://github.com/twilson63/hypar)