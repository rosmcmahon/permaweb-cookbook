---
locale: zh
---
# 你好，世界（代码）

本指南将指引您使用少量代码和[命令行界面（CLI）](./hw-cli.md)，快速将静态HTML、CSS和JavaScript网页部署到Permaweb上。

## 要求

* [NodeJS](https://nodejs.org) LTS版本或更高版本
* 基本的HTML、CSS和JavaScript知识
* 文本编辑器（VS Code、Sublime或类似）

## 描述

使用终端/控制台窗口创建一个名为`hello-world`的新文件夹。

## 设置

```sh
cd hello-world
npm init -y
npm install arweave @bundlr-network/client
mkdir src && cd src
touch index.js index.html style.css
```

接下来，打开您的文本编辑器并导入`hello-world`目录。

## 生成钱包

```sh
node -e "require('arweave').init({}).wallets.generate().then(JSON.stringify).then(console.log.bind(console))" > wallet.json
```

## 创建网页
该网页使用基本的HTML、CSS和JavaScript创建了一个带有样式的按钮，点击按钮时标题文本的颜色会改变。完成后，我们将使用Bundlr和我们之前生成的钱包来部署一个完全功能的静态永久网页到Arweave。

将以下代码块中的代码粘贴到它们各自的文件中：

**index.html**

<details>
<summary>点击查看HTML代码</summary>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" type="text/css" href="style.css">
  <script src="index.js"></script>
  <title>Cookbook Hello World!</title>
</head>

<body>
  <button onclick="changeColor()" class="button">Click Me!</button>
  <h1 id="main">Hello World!</h1>
</body>

</html>
```

</details>
<hr />

**style.css**

<details>
<summary>点击查看CSS代码</summary>

```css
.button {
  padding: '10px';
  background-color: #4CAF50;
}
```

</details>
<hr />

**index.js**

<details>
<summary>点击查看JS代码</summary>

```javascript
function changeColor() {
  const header = document.getElementById("main");
  header.style.color === "" ? header.style.color = "red" : header.style.color = ""
}
```

</details>

<hr />

现在有了一个静态站点可以部署，可以通过在控制台/终端中输入`open src/index.html`来检查它是否正常工作。如果一切按预期工作，那么现在是将其部署到Arweave的时候了！

## 使用bundlr上传
以下命令将部署`src`目录，同时将`index.html`文件指定为清单的索引（相对于提供给`upload-dir`标志的路径）。

```sh
npx bundlr upload-dir src -h https://node2.bundlr.network --index-file index.html -c arweave -w ./wallet.json
```

## 恭喜！！

您刚刚使用几个命令和几行代码在Arweave上发布了一个静态站点！