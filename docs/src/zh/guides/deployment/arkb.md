---
locale: zh
---
# arkb

## 要求
使用`arkb`进行部署需要一个Arweave钱包来支付数据交易成本。

## 安装

要安装`arkb`，请运行
<CodeGroup>
 <CodeGroupItem title="NPM">

```console:no-line-numbers
npm install -g arkb
```
 </CodeGroupItem>
 <CodeGroupItem title="YARN">

```console:no-line-numbers
yarn add ar-gql
```
  </CodeGroupItem>
</CodeGroup>

## 部署

当上传文件目录或一个Permaweb应用程序时，默认情况下，`arkb`将每个文件分别部署为L1事务，并且可以使用Bundlr选项将事务捆绑起来。

## 静态构建
Permaweb应用程序是静态生成的，这意味着代码和内容是提前生成并存储在网络上的。

下面是一个静态网站的示例。要将其部署到Permaweb，将`build`目录作为`deploy`标志的参数传入。

```js
|- build
    |- index.html
    |- styles.css
    |- index.js
```

#### 默认部署

作为L1事务部署可能需要更长的确认时间，因为它直接上传到Arweave网络。

```console
arkb deploy [folder] --wallet [path to wallet]
```
<br/>
<img src="https://arweave.net/_itbo7y4H0kDm4mrPViDlc6bt85-0yLU2pO2KoSA0eM" />

#### 捆绑部署
要使用Bundlr进行部署，您需要<a href="#fund-bundlr">为Bundlr节点提供资金</a>。

Bundlr节点2允许在100kb以下免费交易。

您可以使用`tag-name/tag-value`语法向部署添加自定义可识别标签。

```console
arkb deploy [folder] --use-bundler [bundlr node] --wallet [path to wallet] --tag-name [tag name] --tag-value [tag value]
```
<br/>
<img src="https://arweave.net/jXP0mQvLiRaUNYWl1clpB1G2hZeO07i5T5Lzxi3Kesk" />

## 其他命令

#### 为Bundlr提供资金

```console
arkb fund-bundler [amount] --use-bundler [bundlr node]
```

<sub style="float:right">\* 资金提供给Bundlr实例的处理时间最长可能需要30分钟</sub>

#### 保存密钥文件

```console
arkb wallet-save [path to wallet]
``` 

保存密钥后，您现在可以运行不带--wallet-file选项的命令，像这样

```console
arkb deploy [path to directory]
```

#### 查看钱包余额
```console
arkb balance
```