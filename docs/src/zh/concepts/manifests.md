---
locale: zh
---
# 路径清单

## 概述

在将文件上传到Arweave时，每个文件都被分配一个独特的交易ID。默认情况下，这些ID并没有按照任何特定的方式进行分组或组织。

一张你的猫的照片可能会被分配一个交易ID为[bVLEkL1SOPFCzIYi8T_QNnh17VlDp4RylU6YTwCMVRw](https://arweave.net/bVLEkL1SOPFCzIYi8T_QNnh17VlDp4RylU6YTwCMVRw)，而另一张则是[FguFk5eSth0wO8SKfziYshkSxeIYe7oK9zoPN2PhSc0](https://arweave.net/FguFk5eSth0wO8SKfziYshkSxeIYe7oK9zoPN2PhSc0)。

| Cat1 | Cat2 |
|------|------|
| <img src="https://arweave.net/bVLEkL1SOPFCzIYi8T_QNnh17VlDp4RylU6YTwCMVRw" width="300">|<img src="https://arweave.net/FguFk5eSth0wO8SKfziYshkSxeIYe7oK9zoPN2PhSc0" width="360"> |
| bVLEkL1SOPFCzIYi8T_QNnh17VlDp4... | FguFk5eSth0wO8SKfziYshkSxeIYe7oK9zoPN2PhSc0 |

这些交易ID有些难以处理，使得难以找到所有相关文件。如果你上传了100张你猫咪的图片，你需要追踪**100个不同的ID和链接**！

路径清单是将多个交易链接在一个基本交易ID下的一种方式，并为它们分配可读的文件名。以猫的例子来说，你可以有一个基本交易ID来记住和使用它，就像一个文件夹 - 通过可记忆的文件名访问你的猫的照片，例如[{base id}/cat1.jpg](https://arweave.net/6dRh-TaiA5qtd0NWqrghpvC4_l3EtA3AwCluwPtfWVw/cat1.jpg)，[{base id}/cat2.jpg](https://arweave.net/6dRh-TaiA5qtd0NWqrghpvC4_l3EtA3AwCluwPtfWVw/cat2.jpg)等。

创建一组分组的可读文件名对于在Arweave上创建实用的应用程序至关重要，并且可以在下面的示例中用于托管网站或其他文件集合。

### 你可以使用清单做什么？

---

每当你需要以层次结构方式分组文件时，清单都非常有用。例如：

- **存储NFT收藏品：**
    - [https://arweave.net/X8Qm…AOhA/0.png](https://arweave.net/X8Qm4X2MD4TJoY7OqUSMM3v8H1lvFr-xHby0YbKAOhA/0.png)
    - [https://arweave.net/X8Qm…AOhA/1.png](https://arweave.net/X8Qm4X2MD4TJoY7OqUSMM3v8H1lvFr-xHby0YbKAOhA/1.png)

这与NFT收藏品在链接到存储API或IPFS上的NFT图像和元数据时使用的常见基本路径方法相似。


- **托管网站：**
    - ​​https://arweave.net/X8Qm… AOhA/index.html
    - ​​https://arweave.net/X8Qm… AOhA/styles.css
    - ​​https://arweave.net/X8Qm… AOhA/public/favicon.png


### 清单结构

---

路径清单是使用标签创建和发布到Arweave的一种特殊格式的交易：

 `{ name: "Content-type", value: "application/x.arweave-manifest+json" }`

并具有与下面示例相匹配的以JSON格式编写的交易数据。

```json
{
  "manifest": "arweave/paths",
  "version": "0.1.0",
  "index": {
    "path": "index.html"
  },
  "paths": {
    "index.html": {
      "id": "cG7Hdi_iTQPoEYgQJFqJ8NMpN4KoZ-vH_j7pG4iP7NI"
    },
    "js/style.css": {
      "id": "fZ4d7bkCAUiXSfo3zFsPiQvpLVKVtXUKB6kiLNt2XVQ"
    },
    "css/style.css": {
      "id": "fZ4d7bkCAUiXSfo3zFsPiQvpLVKVtXUKB6kiLNt2XVQ"
    },
    "css/mobile.css": {
      "id": "fZ4d7bkCAUiXSfo3zFsPiQvpLVKVtXUKB6kiLNt2XVQ"
    },
    "assets/img/logo.png": {
      "id": "QYWh-QsozsYu2wor0ZygI5Zoa_fRYFc8_X1RkYmw_fU"
    },
    "assets/img/icon.png": {
      "id": "0543SMRGYuGKTaqLzmpOyK4AxAB96Fra2guHzYxjRGo"
    }
  }
}
```

请参阅官方Arweave路径清单文档的源代码和更多信息：[Arweave Docs](https://github.com/ArweaveTeam/arweave/blob/master/doc/path-manifest-schema.md)