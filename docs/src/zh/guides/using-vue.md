---
locale: zh
---
在Markdown中使用Vue

## 浏览器API访问限制

由于VuePress应用程序在生成静态构建时是在Node.js中进行服务器呈现的，因此任何Vue使用都必须符合[通用代码要求](https://ssr.vuejs.org/zh/universal.html)。简而言之，请确保只在`beforeMount`或`mounted`钩子中访问浏览器/DOM API。

如果您正在使用或演示不符合SSR要求的组件（例如包含自定义指令的组件），则可以将它们包装在内置的`<ClientOnly>`组件中：