---
locale: zh
---
# 执行机API令牌

EXM旨在支持各种加密货币，并且只需要一个API令牌（也称为密钥）进行交互。在EXM中，大多数操作，如部署和写入操作，都需要此API密钥。

## 创建API令牌

创建API令牌需要执行以下步骤：

- 转到[主页](https://exm.dev/)。
- 选择首选的注册/登录方法。

![EXM登录选项](~@source/images/exm-sign-in-options.png)

- 在重定向到仪表板后，点击“New Token”。

![创建新的API令牌](~@source/images/exm-create-token.png)

- 复制生成的令牌，并与SDK或CLI一起使用。

## 安全处理API令牌

API令牌是我们帐户的识别符，并允许我们访问与之关联的功能。因此，确保保密此令牌以防止任何垃圾邮件和攻击对我们的功能至关重要。最好的方法是使用环境变量进行存储。

有两种存储环境变量的方法：

1. 通过命令行：

在项目目录中，执行以下命令：

```bash
export EXM_PK=<your_api_token>
```

2. 通过`dotenv` SDK：

- 在命令行中运行以下命令：

  ```bash
  npm install dotenv

  #或

  yarn add dotenv
  ```
- 在文件中导入该库以使用变量：

  ```jsx
  import dotenv from "dotenv";
  dotenv.config();
  ```

然后，可以在文件中使用`process.env.EXM_PK`引用此密钥，而无需将其公开或将其推送到版本控制系统（如GitHub）。