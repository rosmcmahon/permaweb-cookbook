---
locale: zh
---
# 原子令牌的概念与原则

![https://arweave.net/bcHI0TW_nH-iTfZnobD9Wt6d0Qe6thWfPrGhnvi1m1A](https://arweave.net/bcHI0TW_nH-iTfZnobD9Wt6d0Qe6thWfPrGhnvi1m1A)

原子令牌是一个永久性标识符，引用了Permaweb上的数据和SmartWeave合约。

## 规格

数据必须存储在arweave网络上，并且可以通过交易标识符进行引用。

合约必须实现一个`balances`对象，表示原子令牌的所有权。

合约必须实现一个`transfer`函数，接受以下参数：
- target {钱包地址或合约}
- qty {数量}

> transfer函数应该将所有权从调用者转移给目标对象。

## 选项

_以下是可以使原子令牌在Permaweb上可发现和可交易的实现选项_

[Verto Flex](https://github.com/useverto/flex) - Flex库允许您的原子令牌在无需信任交易所的情况下进行买卖。

[发现性标签 - ANS 110](https://github.com/ArweaveTeam/arweave-standards/blob/master/ans/ANS-110.md) - 这些额外的标签可以帮助Permaweb应用和服务发现您的令牌。

[查看指南](../guides/atomic-tokens/intro.md)