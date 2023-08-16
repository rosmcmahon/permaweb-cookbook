---
locale: zh
---
### Bundlr CLI

---

`bundlr upload-dir <folder>` 将一个本地目录上传到 Arweave，并自动生成文件清单(manifest)。

如果您想手动上传自己的清单文件，可以在任意交易上使用 `--content-type "application/x.arweave-manifest+json"` 标志来指定它为一个清单交易。

### Bundlr JS 客户端

---

使用以下代码片段将一个本地目录上传到 Arweave，并自动生成文件清单:

```js
await bundlr.uploadFolder("./path/to/folder", {
     indexFile: "./optionalIndex.html", // 可选的索引文件（用户访问清单时加载的文件）
     batchSize: 50, // 一次性上传的项目数量
     keepDeleted: false   // 是否保留先前上传项目中已删除的项
    }) // 返回清单ID
```

如果您想手动上传自己的清单文件，可以使用 `await bundlr.upload(data, { tags: [{ name: "Content-type", value: "application/x.arweave-manifest+json" }] } )` 来指定上传的 `data` 为一个清单交易。

---

来源和进一步阅读：[Bundlr 文档](https://docs.bundlr.network/docs/overview)