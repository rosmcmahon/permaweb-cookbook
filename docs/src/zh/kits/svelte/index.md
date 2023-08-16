---
locale: zh
---
# Svelte起始套件

Svelte是一个编译成JavaScript捆绑包的框架，同时在该过程中将框架从应用程序的分发中移除。与其他框架相比，这导致了更小的占用空间。Svelte是Permaweb应用程序的完美框架。Permaweb应用程序基于单页面应用程序的原则构建，但它们存在于Arweave网络上，并由Permaweb网关进行分发。

Svelte启动套件指南：

* [Minimal](./minimal.md) - 构建Svelte Permaweb应用程序所需的最小配置
* [Vite](./vite.md) - Svelte、TypeScript和Vite

::: info Permaweb应用程序限制
* 100%前端应用程序（无服务器端后端）
* 应用程序从子路径提供服务（https://[gateway]/[TX_ID]）
:::