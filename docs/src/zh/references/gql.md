---
locale: zh
---
# 交易的完整GraphQL结构
以下GraphQL查询返回由索引服务捕获的交易的所有属性。

```graphql:no-line-numbers
query {
    transactions {
        
        pageInfo { 
          hasNextPage
        }
        edges {
            cursor
            node {
                id
                anchor
                signature
                recipient
                owner {
                    address
                    key
                }
                fee {
                    winston
                    ar
                }
                quantity {
                    winston
                    ar
                }
                data {
                    size
                    type
                }
                tags {
                    name
                    value
                }
                block {
                    id
                    timestamp
                    height
                    previous
                }
                parent {
                    id
                }
            }
        }
    }
}

```

## 分页
默认情况下，GraphQL查询返回前10个结果。可以通过在交易查询中添加`first: X`选项（其中`X`是从1到100的值）来请求更多的结果集。
```graphql{4}
query
{
  transactions(
    first:100,
    tags: [
      {
        name: "App-Name",
        values: ["PublicSquare"]
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
如果结果集中有超过100个项目，可以使用光标来检索后续的结果页。
```graphql{13-15,17}
query
{
  transactions(
    first:100,
    tags: [
      {
        name: "App-Name",
        values: ["PublicSquare"]
      }
    ]
  ) 
  {
    pageInfo { 
      hasNextPage
    }
    edges {
      cursor
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
如果有后续的结果页，`hasNextPage`的值将为`true`。取结果集中最后一项的`cursor`值，并将其用作`after`查询参数的值。
```graphql{5}
query
{
  transactions(
    first:100,
    after: "WyIyMDIyLTEyLTMwVDE2OjQ0OjIzLjc0OVoiLDEwMF0=",
    tags: [
      {
        name: "App-Name",
        values: ["PublicSquare"]
      }
    ]
  ) 
  {
    pageInfo { 
      hasNextPage
    }
    edges {
      cursor
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
要检索整个结果集，重复使用具有来自每个页面的最后一个项目的更新的`cursor`值的`after`查询，直到`hasNextPage`为`false`为止。

## 速率限制
索引服务将实施速率限制以防止对其服务的攻击和滥用。`arweave.net/graphql`服务将GraphQL查询限制为每5分钟600个查询（每个IP地址）。在解析响应之前，始终检查查询结果是否具有200系列的状态代码。HTTP状态代码429表示正在执行速率限制。HTTP状态代码503通常表示查询结果集对于`arweave.net/graphql`而言太大。

## 资源
* 有关Arweave GraphQL模式的更完整列表，请参阅[Arweave GraphQL指南](https://gql-guide.arweave.dev)
* [ArDB软件包](../guides/querying-arweave/ardb.md)
* [ar-gql软件包](../guides/querying-arweave/ar-gql.md)
* 要了解有关GraphQL的通用指南，[graphql.org/learn](https://graphql.org/learn)是一个很好的起点