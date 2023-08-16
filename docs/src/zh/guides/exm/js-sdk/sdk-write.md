---
locale: zh
---
# 使用执行机器SDK编写无服务器函数

一旦函数部署完成，可以通过写操作来更新其状态。由于EXM的无服务器函数的独特架构，更新状态的逻辑与状态本身存储在一起，可以使用相同的`functionId`来引用它们。根据应用程序的需求，函数可以有一个或多个操作来更新状态，并且写调用的参数相应地有所不同。

<details>
<summary><strong>函数逻辑和相应的写入示例</strong></summary>

- <strong>带有单个操作用于更新状态的函数示例：</strong>

以下函数将名称添加到用户数组中：

```js
export async function handle(state, action) {
    state.users.push(action.input.name);
    return { state };
}
```

以下代码行更新状态：

```js
state.users.push(action.input.name);
```

在这种情况下，写调用只需要一个以`name`为键的键值对作为输入：

```js
const inputs = [{ name: '开源开发者' }];
```

- <strong>带有多个操作用于更新状态的函数示例：</strong>

以下函数创建帖子，但还具有更新或删除这些帖子的能力：

```js
export async function handle(state, action) {
  const { input } = action
  if (input.type === 'createPost' || input.type === 'updatePost') {
    state.posts[input.post.id] = input.post
  }
  if (input.type === 'deletePost') {
    delete state.posts[input.postId]
  }
  return { state }
}
```

帖子是具有以下格式的对象：

```js
post: {
  id: string
  title: string
  content: string
  author: string
}
```

我们为每个帖子分配一个唯一的`id`，以便我们可以通过它进行更新或删除。如果不存在相应的`id`，则会创建一个新的帖子。

然而，如上所示的函数逻辑具有执行多个操作的功能，因此为每个操作的`type`赋予了名称。必须将此名称作为输入与帖子或id一起传入，以执行适当的写调用。要更新帖子，写调用的输入应如下所示：

```js
const inputs = [{
  type: 'updatePost',
  post: {
    id,
    title: "我的帖子",
    content: "我的更新后的帖子",
    author: "开源开发者"
  }
}];
```
</details>
<br/>

写事务接受两个参数。需要与函数交互以及函数处理写请求和更新状态所需的任何`inputs`。

<CodeGroup>
  <CodeGroupItem title="write.js">

```js
import { Exm } from '@execution-machine/sdk';
import { functionId } from './functionId.js';

// 初始化新的EXM实例
const exm = new Exm({ token: process.env.EXM_API_TOKEN });

// inputs是一个对象数组
const inputs = [{ name: '开源开发者' }];

// 从缓存层读取
const writeResult = await exm.functions.write(functionId, inputs);
console.log(writeResult);
```

  </CodeGroupItem>
</CodeGroup>

成功的写请求将返回一个带有状态为SUCCESS的对象。

```bash
{
  status: 'SUCCESS',
  data: {
    pseudoId: 'txnId',
    execution: {
      state: [Object],
      result: null,
      validity: [Object],
      exmContext: [Object],
      updated: false
    }
  }
}
```