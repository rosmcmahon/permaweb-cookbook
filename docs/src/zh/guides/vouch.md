---
locale: zh
---
# Vouch（背书）

有几种查询 Arweave 地址以验证是否已由某个服务背书的方法。以下是其中的两种方法。
## VouchDAO 包
`isVouched` 函数可直接在您的应用程序中使用。

#### 安装
使用 npm 添加包：
```console:no-line-numbers
npm i vouchdao
```
或者使用 yarn：
```console:no-line-numbers
yarn add vouchdao
```

#### 用法
在异步函数内部，您可以使用 `isVouched` 函数，该函数返回 true（如果用户已被背书）。

```js:no-line-numbers
import { isVouched } from 'vouchdao'
(async () => {
  const res = await isVouched("ARWEAVE_ADDRESS") // true 或 undefined
  // ...
})();
```

## 使用 GraphQL
您可以使用 GraphQL 查询 Arweave 网络，以查明给定的 Arweave 地址是否已被背书。

```graphql
query {
  transactions(
    tags:{name:"Vouch-For", values:["ARWEAVE_ADDRESS"]}
  ) {
    edges {
      node {
        id
        tags {
          name 
          value 
        }
      }
    }
  }
}
```

如果地址已被背书，则返回一组与发行 ANS-109 服务相关的标签的节点。您可以与通过 VouchDAO 社区投票传递的 [社区投票](https://community.xyz/#_z0ch80z_daDUFqC9jHjfOL8nekJcok4ZRkE_UesYsk/votes) 交叉参考 `owner address` 值来确保该服务已通过社区投票获得验证。

```graphql
"owner": {
 "address": "Ax_uXyLQBPZSQ15movzv9-O1mDo30khslqN64qD27Z8"
},
"tags": [
  {
    "name": "Content-Type",
    "value": "application/json"
  },
  {
    "name": "App-Name",
    "value": "Vouch"
  },
  {
    "name": "App-Version",
    "value": "0.1"
  },
  {
    "name": "Verification-Method",
    "value": "Twitter"
  },
  {
    "name": "Vouch-For",
    "value": "ARWEAVE_ADDRESS"
  }
]
```

## 资源
* [VouchDAO](https://vouch-dao.arweave.dev)
* [VouchDAO 合约](https://sonar.warp.cc/?#/app/contract/_z0ch80z_daDUFqC9jHjfOL8nekJcok4ZRkE_UesYsk)
* [Arweave/GraphQL Playground](https://arweave.net/graphql)