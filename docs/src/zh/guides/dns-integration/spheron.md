---
locale: zh
---
# Spheron

Spheron协议是一个去中心化平台，旨在简化现代dapp的创建。它为开发者提供了无缝体验，可以快速部署、自动扩展，并在去中心化网络上进行个性化内容交付。

Spheron使用GitHub集成来处理持续部署，并使我们能够将自定义DNS集成到任何给定的部署中。

## 设置Spheron帐户所需的内容

* GitHub帐户
* 一个部署在永久Web上的永久Web应用标识符

::: tip
要使用Spheron部署Arweave应用程序，您将需要20美元/月的专业计划。
:::

## 身份验证/登录

Spheron依赖于GitHub、GitLab或BitBucket仓库来进行部署，类似于Vercel。

要登录Spheron，请转到[Spheron Aqua仪表板](https://app.spheron.network/)，然后选择您的首选身份验证方式。

## 导入仓库

登录后，您将看到用户仪表板。单击仪表板右上角的“新项目”按钮来导入一个仓库。选择您想要的仓库，并选择将其部署到Arweave的选项。

## 连接到DNS

现在，您已经导入了项目并进行了部署，请转到“域名”选项卡。输入域名、环境，并选择一个域名指向部署的域名。

在继续之前，系统将要求您验证已配置的记录。在您的域名管理器中更新该记录。更新DNS可能需要最多72小时。您将看到类似于以下图片的东西：

<img src="https://arweave.net/8BNk8spFayPCdCHx1XrsoMtMdX1-qsDYAORPJ8BNZ3Y" />

更新完成后，您需要在Spheron中进行验证。点击`验证`按钮，然后您应该准备好开始了。现在，每当您将新版本部署到GitHub时，您的域名将被更新为最新版本！🎉

::: tip
要创建一个完全去中心化的应用程序，请确保使用[ArNS](https://ar.io/arns)或任何去中心化DNS服务器
:::

## 总结

Spheron是将Permaweb应用程序部署到Arweave并将其重定向到自定义域名的简单方法。结合持续集成和持续部署，确保了全面的开发者体验！