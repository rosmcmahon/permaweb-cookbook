---
locale: zh
---
# 钱包和密钥

---

### Arweave钱包

在Arweave上，钱包用于在区块链上保护唯一的地址。该地址用于跟踪你的$AR余额，并与Arweave网络进行交互，例如发送交易或与[SmartWeave Contracts](../guides/smartweave/warp/intro.md)进行交互。

像大多数区块链一样，在Arweave上的钱包的概念是有点误导的。

钱包本身并不“持有”任何代币；代币余额存储在区块链上，并与钱包地址关联。相反，钱包持有用于签署交易以发布数据或转移代币的加密公钥-私钥对。钱包的所有者（即具有访问钱包的**私钥**的人）是唯一能够为该地址签署交易并访问其资金的人。

### 密钥对和钱包格式

Arweave使用存储在JWK（JSON Web Keys）格式中的*4096位*RSA-PSS密钥对。 JWK格式可以用来存储许多类型的加密密钥，不仅仅是RSA密钥对。

这是一个JWK文件的内容，它描述了一个RSA-PSS密钥对。值被缩写，以免意外地用作链上交易的发送者或接收者。在存储RSA-PSS密钥对时，与JWK中的`n`相关联的值是你钱包的**公钥**，可以安全地分享，而不会损害钱包的安全性。

```json
{
	"d": "cgeeu66FlfX9wVgZr5AXKlw4MxTlxSuSwMtTR7mqcnoE...",
	"dp": "DezP9yvB13s9edjhYz6Dl...",
	"dq": "SzAT5DbV7eYOZbBkkh20D...",
	"e": "AQAB",
	"ext": true,
	"kty": "RSA",
	"n": "o4FU6y61V1cBLChYgF9O37S4ftUy4newYWLApz4CXlK8...",
	"p": "5ht9nFGnpfW76CPW9IEFlw...",
	"q": "tedJwzjrsrvk7o1-KELQxw...",
	"qi": "zhL9fXSPljaVZ0WYhFGPU..."
}
```

你的**私钥**也存储在JWK中，主要是在与`d`关联的值中，但它也部分由JWK中的其他一些值派生而来。**私钥**就像钱包的密码-可以用于创建数字签名（例如用于签署交易）或解密数据。

这些JWK实际上是从钱包应用程序（如[Arweave.app](https://arweave.app)）创建和导出的`json`文件，或者使用[code](https://github.com/ArweaveTeam/arweave-js)通过代码生成的。

当使用钱包应用程序生成你的密钥对时，你的**私钥**也可以表示为助记词**种子短语**，在某些情况下可用作签署交易和/或恢复钱包的替代方法。

### 钱包安全性

你的**私钥**必须始终保密，因为它具有从你的地址转移代币给他人的能力。作为开发者，请确保不要将你的密钥文件包含在任何公共的GitHub存储库中或公开托管在其他地方。

### 钱包地址

有趣的是，钱包的地址是由其**公钥**派生而来。虽然安全地与他人分享你的**公钥**是可以的，但*4096位*的**公钥**有点大，不太方便传递。为了减少这种开销并使钱包地址更易读，**公钥**的`SHA-256`哈希被`Base64URL`编码并用作钱包地址。这种安全性和确定性地将一个唯一的43个字符的钱包地址与钱包的**公钥**链接起来，并提供了一个方便的简写形式，任何拥有该**公钥**的人都可以验证。

### 钱包

[Arweave.app](https://arweave.app/welcome) - Arweave网络钱包，用于部署永久数据，将你的帐户安全连接到分散应用程序，并浏览Weave。

[ArConnect](https://www.arconnect.io/) - Arweave钱包浏览器扩展

### 参考资料和进一步阅读：
[Arweave文档](https://docs.arweave.org/developers/server/http-api#key-format)

[JSON Web Key格式（RFC 7517）](https://www.rfc-editor.org/rfc/rfc7517)