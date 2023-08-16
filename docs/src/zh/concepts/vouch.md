---
locale: zh
---
# Vouch
## 概述
#### Vouch协议
Arweave引入了ANS-109 Vouch（身份声明）的概念。它是一种使用特定交易格式和一些标签的标准，允许永久网络上的任何人为任何Arweave地址的身份和人性进行“担保”。

将ANS-109标准添加到永久网络将有助于最小化肆意攻击和不良行为，使永久网络用户的体验更加安全。

#### VouchDAO
VouchDAO是基于Vouch标准构建的社区主导的去中心化验证层。开发人员创建担保服务，VouchDAO社区的成员对这些验证服务进行投票，以确定其是否值得信赖。

一旦创建了新的服务，其地址将在[VouchDAO community.xyz](https://community.xyz/#_z0ch80z_daDUFqC9jHjfOL8nekJcok4ZRkE_UesYsk)界面上进行投票。如果投票通过，则将其作为已验证的担保添加进来。

<img src="https://arweave.net/7W9krszlEXdR38LB7uXgJ_EPXGj-woXljsA5h5GpGzk" />

## 工作原理
开发人员可以创建不同的Vouch服务，根据一组给定的要求对用户的Arweave钱包进行证明。目前的例子是Twitter服务，这是第一个担保服务，迄今为止已对180个Arweave地址进行了担保。

VouchDAO智能合约状态具有一个`vouched`属性。每当用户通过验证，此状态就会得到更新。`vouched`对象以以下格式存储一组已担保地址：
```
VOUCH_USER_ADDRESS:[
  {
    service:"SERVICE_ADDRESS_1"
    transaction:"TX_ID"
  },
   {
    service:"SERVICE_ADDRESS_2"
    transaction:"TX_ID"
  }
]
```

通过验证的用户将收到ANS-109令牌，以表示该钱包已由该服务担保。

## ANS-109交易格式
| 标签名称 | _可选？_ | 标签值 |
|---|---|---|
|App-Name|否|`Vouch`|
|Vouch-For|否|在此交易中被担保的Arweave `address`|
|App-Version|是|`0.1`|
|Verification-Method|是|验证个人身份的方法。示例-`Twitter`/ `In-Person`/ `Gmail`/ `Facebook`|
|User-Identifier|是|根据验证方法对用户的标识符。示例-`abhav@arweave.org`|

## 资源
* [VouchDAO](https://vouch-dao.arweave.dev)
* [VouchDAO合约](https://sonar.warp.cc/?#/app/contract/_z0ch80z_daDUFqC9jHjfOL8nekJcok4ZRkE_UesYsk)