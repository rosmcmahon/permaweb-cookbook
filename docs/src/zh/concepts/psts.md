---
locale: zh
---
# 概述

---

利润分成代币（PST）是一种包含以下结构的智能合约令牌（SmartWeaveToken）：

| 属性       | 类型      |
| ----------| ---------|
| 余额       | 对象      |
| 名称       | 字符串    |
| 代号       | 字符串    |
| 转账       | 方法      |
| 余额       | 方法      |

PST通常用于管理协议或“利润共享社区”（PSC）- 类似于DAO。

### PST如何分配？

---

应用的创始人可以创建一定数量的PST，并按他们认为合适的方式进行分发 - 保留或出售给投资者以筹集资本。

协议可以通过提供PST作为奖励来鼓励成员做出贡献或完成社区任务以促进增长。

PST还可以在[Permaswap](https://permaswap.network/#/)（当前处于测试网中）之间相互交换，开发者可以使用[Verto Flex](https://github.com/useverto/flex)建立令牌交易权限。

### 特点

---

PST作为“**微股息**”。当使用协议时，将一定金额的小费分配给持有者。该小费以$AR的形式支付 - 而不是以PST的货币形式支付。这在正在开发的应用程序和Arweave本身之间创造了一种非常特殊的关系。

### 好处

---

- 提供了一种灵活的方式，供开发者运行协议并根据自己认为合适的方式分配“所有权”
- PST可以用作用于协议工作或社区任务的支付方式
- 创始人有动力增加网络使用量，因为这与收入直接相关
- 持有者获得**固有**价值（奖励为$AR，而不是更多的“股权”）

### 示例PST：ARDRIVE代币

---

ArDrive是一个永久网络应用程序，使用了其恰当命名的PST：ARDRIVE。

当有人支付$AR通过ArDrive上传数据时，将按照随机加权的方法将15%的社区费用分配给单个代币持有者。

![ArDrive PST Cycle](~@source/images/ardrive-pst.png)

用户上传数据->一个ARDRIVE代币持有者获得$AR->ARDRIVE代币持有者可以使用这笔$AR来上传文件->循环重复。希望这样能给您一个好的想法，如何实施您自己的PST。

### 探索PST

---

直接使用ViewBlock和Sonar by Redstone可能是最恰当的。只需使用专门显示PST的链接，这样读者就不必搜索才能找到它们。

您可以使用[ViewBlock](https://viewblock.io/arweave)来获得类似于Etherscan的体验，并查看PST合约，例如[这里](https://viewblock.io/arweave/contract/-8A6RexFkpfWwuyVO98wzSFZh0d6VJuI-buTJvlwOJQ)。

另一个选择是Sonar，这是由[RedStone Finance](https://sonar.redstone.tools/#/app/contracts)构建的Arweave智能合约浏览器。在此处查看同一合约 [这里](https://sonar.warp.cc/?#/app/contract/-8A6RexFkpfWwuyVO98wzSFZh0d6VJuI-buTJvlwOJQ)。

> 一些社区成员讨论将PST称为“永久网络服务代币”。关于PST还有很多待探索的内容，请参加[此处](https://discord.com/channels/999377270701564065/999377270701564068/1055569446481178734)的讨论（Discord）。