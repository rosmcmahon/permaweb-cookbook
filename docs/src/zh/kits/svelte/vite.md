---
locale: zh
---
# Svelte/Vite 起始套件

Svelte 是一個能夠產生小型打包檔的框架，非常適合於永久網路 (permaweb)。作為開發人員，我們重視開發體驗 (Dev Experience) 和使用者體驗 (User Experience) 一樣。這個套件使用 `vite` 打包系統，為開發人員提供了很好的開發體驗 (DX)。

## 使用 vite 、svelte 和 typescript 安裝

<CodeGroup>
  <CodeGroupItem title="NPM v6">

```console
npm create vite@latest my-perma-app --template svelte-ts
```

  </CodeGroupItem>
  <CodeGroupItem title="NPM v7">

```console
npm create vite@latest my-perma-app -- --template svelte-ts
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

```console
yarn create vite my-perma-app --template svelte-ts
```

  </CodeGroupItem>
  <CodeGroupItem title="PNPM">

```console
pnpm create vite my-perma-app --template svelte-ts
```

  </CodeGroupItem>
</CodeGroup>

## 專案資訊

vite 打包系統將你的 index.html 檔案放在根目錄，這是你可以在其中包含任何需要的 CSS 或全域程式碼的地方。有關 vite 專案佈局的更多資訊，請參閱 [vite 文件](https://vitejs.dev/guide/#index-html-and-project-root)。

## 設置 hash-router

為了設置 hash-router ，我們將使用 [tinro](https://github.com/AlexxNB/tinro)。`tinro` 是一個簡小的聲明式路由庫，類似於 React Router。

<CodeGroup>
  <CodeGroupItem title="NPM">

```console
npm install --save-dev tinro
```

  </CodeGroupItem>
  <CodeGroupItem title="YARN">

```console
yarn add -D tinro
```

  </CodeGroupItem>
</CodeGroup>

## 告訴 Svelte 使用 hash routing

在 `src/App.svelte` 檔案中，你需要設定路由器以使用 hash routing 模式。

```html
<script lang="ts">
  import { Route, router } from "tinro";
  router.mode.hash();
  router.subscribe((_) => window.scrollTo(0, 0));
</script>
<main>

</main>
```

`router.mode.hash` 函式啟用了 hash 路由模式。
`router.subscribe` 回調函式可以在頁面轉換時將頁面滾動至頂部。

## 新增一些過渡元件

這些元件將在路由時管理一個頁面到另一個頁面的過渡效果。

在 `src` 目錄下新建一個名為 `components` 的目錄，並在其中新增以下兩個檔案：

announcer.svelte

```html
<script>
  import { router } from "tinro";
  $: current = $router.path === "/" ? "Home" : $router.path.slice(1);
</script>

<div aria-live="assertive" aria-atomic="true">
  {#key current}
    Navigated to {current}
  {/key}
</div>

<style>
  div {
    position: absolute;
    left: 0;
    top: 0;
    clip: rect(0 0 0 0);
    clip-path: inset(50%);
    overflow: hidden;
    white-space: nowrap;
    width: 1px;
    height: 1px;
  }
</style>
```

> 這個元件用於螢幕閱讀程式在頁面變更時發出及時的通知。

transition.svelte

```html
<script>
  import { router } from "tinro";
  import { fade } from "svelte/transition";
</script>

{#key $router.path}
  <div in:fade={{ duration: 700 }}>
    <slot />
  </div>
{/key}
```

> 這個元件為頁面過渡效果添加了一個淡入淡出效果。

## 將路由加入應用程式

```html
<script lang="ts">
  ...
  import Announcer from "./components/announcer.svelte";
  import Transition from "./components/transition.svelte";
  ...
</script>
<Announcer />
<Transition>
  <Route path="/">
    <Home />
  </Route>
  <Route path="/about">
    <About />
  </Route>
</Transition>
```

將 Announcer 和 Transition 元件添加到我們的路由系統中將負責發布頁面過渡通知以及為過渡添加動畫效果。

## 建立一些頁面

### home.svelte

```html
<script lang="ts">
let count = 0

function inc() {
  count += 1
}
</script>
<h1>Hello Permaweb</h1>
<button on:click={inc}>Inc</button>
<p>Count: {count}</p>
<a href="/about">About</a>
```

### about.svelte

```html
<h1>About Page</h1>
<p>Svelte/Vite About Page</p>
<a href="/">Home</a>
```

### 修改 `App.svelte`

```html
<script lang="ts">
  ...
  import Home from './home.svelte'
  import About from './about.svelte'
  
</script>
...
```

## 發布至 Permaweb

### 產生錢包

```sh
yarn add -D arweave
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

### 安裝 bundlr

```sh
yarn add -D @bundlr-network/client
```

### 更新 package.json

```json
{
  ...
  "scripts": {
    ...
    "deploy": "yarn build && bundlr upload-dir dist -h https://node2.bundlr.network --wallet ./wallet.json -c arweave --index-file index.html --no-confirmation"
  }
}
```

### 執行部署

```sh
yarn deploy
```

::: tip 成功
你現在應該在 Permaweb 上擁有了一個 Svelte 應用程式！做得好！
:::

::: warning 充值錢包
如果你的應用程式大於 120KB，你將需要給你的 bundlr 錢包充值。詳細資訊請參閱 [https://bundlr.network](https://bundlr.network)。
:::

## 存放庫

這個例子的完整版本可在此處找到：[https://github.com/twilson63/svelte-ts-vite-example](https://github.com/twilson63/svelte-ts-vite-example)

## 總結

這只是一個在 Permaweb 上發布 Svelte 應用程式的最基本版本，但你可能需要更多功能，例如熱重載和 tailwind 等。你可以參考 `hypar` 提供的一個開箱即用的起始套件。[HypAR](https://github.com/twilson63/hypar)