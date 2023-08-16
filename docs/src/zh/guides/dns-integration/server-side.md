---
locale: zh
---
# 服务器端 DNS 集成

所以你有一个永久网络（permaweb）应用，并且它在永久网络上，但你还有一个特定的域名，你希望用户使用该域名访问这个应用程序。mydomain.com，为了将你的域名连接到永久网络应用程序，你有几个选项，我们将展示这里的一个选项，称为服务器端重定向。重定向作为一个反向代理发生，使得用户在他们的浏览器中保持在 mydomain.com，而在幕后应用程序是从永久网络中提供的。

::: tip
你可以使用任何反向代理来设置服务器端重定向，在本指南中，我们将使用 deno 和 deno.com，一个轻量级边缘主机服务。
:::

## 设置使用 deno.com 的反向代理所需的内容

* 一个 deno.com 账户，在撰写本文时是免费的。
* 一个带有访问 DNS 设置权限的域名
* 一个永久网络应用程序标识符，并且已部署在永久网络上

## 在 Deno.com 上创建代理

Deno Deploy 是一个在边缘运行的分布式系统。全球有 35 个地区。打开你的浏览器访问 [https://deno.com](https://deno.com)，如果你还没有账户，点击登录或注册。

点击 `New Project`，然后点击 `Play`

在 deno playground 上，我们可以在不离开浏览器的情况下创建一个代理。

复制以下代码：

```ts
import { serve } from "https://deno.land/std/http/mod.ts";

const APP_ID = "你的 Arweave 标识符";

const fileService = `https://arweave.net/${APP_ID}`;

// 处理请求
async function reqHandler(req: Request) {
  const path = new URL(req.url).pathname;
  // 代理到 arweave.net
  return await fetch(fileService + path).then(res => {
    const headers = new Headers(res.headers)
    // 指示服务器使用边缘缓存
    headers.set('cache-control', 's-max-age=600, stale-while-revalidate=6000')

    // 返回从 arweave.net 得到的响应
    return new Response(res.body, {
      status: res.status,
      headers
    })
  });
}

// 等待请求
serve(reqHandler, { port: 8100 });
```

这个代理服务器将接收来自 mydomain.com 的请求，并将请求代理到 arweave.net/APP_ID，然后返回响应作为 mydomain.com。你的 APP_ID 是你永久网络应用程序的 TX_ID 标识符。

点击 `Save and Deploy`

## 连接到 DNS

在项目设置中转到域部分，并点击添加域。

输入 `mydomain.com` 域名，并按照说明修改你的 DNS 设置来指向 deno deploy 的边缘网络。

可能需要几分钟才能解析到 DNS 上，但一旦解析完成，你的应用程序现在将从 mydomain.com 渲染。

:tada: 恭喜你，你已经发布了一个服务器端重定向到你的永久网络应用程序。

::: warning
请注意，对应用程序的任何更改都将生成一个新的 TX_ID，你需要修改该 TX_ID，以将新更改发布到你的域名上。
:::

## 自动化部署

如果你想要自动化你的永久网络应用的新部署，请查阅 GitHub Actions，并使用 deno deploy 的 GitHub Action：[https://github.com/denoland/deployctl/blob/main/action/README.md](https://github.com/denoland/deployctl/blob/main/action/README.md)


## 摘要

服务器端重定向非常适合为用户提供一个域名系统URL来访问你的永久网络应用程序。希望你在永久网络开发过程中发现本指南很有用！