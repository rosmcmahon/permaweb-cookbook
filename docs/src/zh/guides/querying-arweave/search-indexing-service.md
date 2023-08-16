---
locale: zh
---
# 搜索索引服务

简介：

- 兼容 Arweave GraphQL 的语法
- 对复杂查询（比如多标签搜索）有更快的响应时间
- 提供更多查询选项
---

[Goldsky](https://goldsky.com) 的免费搜索服务使用优化的后端，可以在 arweave 区块和交易中进行更快速的复杂查询，还提供了用于模糊搜索和通配符搜索用例的额外查询语法。

搜索 GraphQL 语法是 [Arweave GraphQL 语法](./queryingArweave.md) 的超集。它完全向后兼容，对于相同的查询将返回相同的结果，但是还有一些额外的修饰符可以提供帮助。

- 灵活的标签过滤器
  - 只搜索标签名或值
- 高级标签过滤器
  - 模糊搜索
  - 通配符搜索
- 仅过滤 L1 交易
- 结果集总计数

如果有任何定制需求或功能建议，请随时通过电子邮件或 Discord 联系 Goldsky 团队！

## 搜索网关端点

目前，唯一支持这种语法的服务是 Goldsky。如果有人有兴趣使用相同的语法托管自己的网关，请随时联系 [Goldsky](https://goldsky.com) 寻求帮助。

- [Goldsky 搜索服务](https://arweave-search.goldsky.com/graphql)

## 功能

### 灵活的标签过滤器

搜索网关语法更加灵活，可以仅搜索标签名或值。

#### 示例
搜索标签值为 "cat" 的交易

```graphql:no-line-numbers
query just_values {
  transactions(
    first: 10,
    tags: [
      {
        values: ["cat"]
      }
    ]
  ) 
  {
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

搜索具有 "In-Response-To-ID" 的交易

```graphql:no-line-numbers
query just_name {
  transactions(
    first: 10,
    tags: [
      {
        name: "In-Response-To-ID"
      }
    ]
  ) 
  {
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


### 高级标签过滤器

搜索网关语法为标签过滤器提供了额外的参数 `match`。

| 匹配值 | 描述 | 
|-------------|-------------|
| EXACT |（默认）仅精确匹配。 |
| WILDCARD |启用 * 以匹配任意字符，例如 `text/*`。 |
| FUZZY_AND |包含所有搜索项的模糊匹配 |
| FUZZY_OR |包含至少一个搜索项的模糊匹配 |

打开 Playground 并尝试以下查询！

使用通配符搜索所有具有图像内容类型的交易
```graphql:no-line-numbers
{
    transactions(        
      tags: [
        { name: "Content-Type", values: "image/*", match: WILDCARD}
      ]
      first: 10
    ) {
        edges {
            cursor
            node {
                id
              tags {
                name
                value
              }
              block { height }
              bundledIn {id}
            }
        }
    }
}
```

### 模糊搜索

模糊搜索非常强大，可以搜索包含许多变体的“相似”文本。

搜索包含 'cat' 或 'dog'（或 CAT 或 doG 或 cAts 或 CAAts 等）的所有交易。因此，标签可以包含至少一个类似于猫或狗的术语。

```graphql:no-line-numbers
{
    transactions(        
      tags: [
        { name: "Content-Type", values: ["cat", "dog"], match: "FUZZY_OR"}
      ]
      first: 10
    ) {
        edges {
            cursor
            node {
                id
              tags {
                name
                value
              }
              block { height }
              bundledIn {id}
            }
        }
    }
}
```

搜索具有类似于猫和狗的标签值的交易
```graphql:no-line-numbers
{
    transactions(        
      tags: [
        { name: "Content-Type", values: ["cat", "dog"], match: "FUZZY_AND"}
      ]
      first: 10
    ) {
        edges {
            cursor
            node {
                id
              tags {
                name
                value
              }
              block { height }
              bundledIn {id}
            }
        }
    }
}
```

### 排除打包（L2）交易

只需设置 `bundledIn: NULL`

```graphql:no-line-numbers
query just_l1 {
  transactions(
    first: 10,
    bundledIn: null
  ) 
  {
    edges {
      node {
        id
        signature
        owner {
          address
        }
        block {
          height
        }
      }
    }
  }
}
```


### 给定查询的总计数

如果您想要了解符合某个过滤器集合的交易数量，请使用 `count` 字段。这会触发一个额外的优化计数操作。这可能会使返回查询所需的时间翻倍，所以只在需要时使用。

```graphql:no-line-numbers
query count_mirror {
  {
  	transactions(tags:{values:["MirrorXYZ"]})
      {
        count
      }
  }
}
```
