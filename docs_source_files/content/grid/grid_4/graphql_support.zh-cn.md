---
title: "GraphQL查询支持"
weight: 4
---
 

GraphQL 是一种用于API的查询语言, 也是用于使用现有数据完成这些查询的运行时. 其仅仅是使用户能够准确地获取所需. 

## 枚举
枚举是表示字段的可能值的集合.

例如, `Node`对象具有一个称为`status`的字段. 状态是一个枚举 (特别是`Status`类型) , 因为它可能是`UP` , `DRAINING` 或 `UNAVAILABLE`.

## 标量
标量是基本类型的值: `Int`, `Float`, `String`, `Boolean`, 或 `ID`.

在调用GraphQL API时, 必须指定嵌套子字段, 直到只返回标量.


## 模式的结构
网格模式的结构如下:

```shell
{
    session(id: "<session-id>") : {
        id,
        capabilities,
        startTime,
        uri,
        nodeId,
        nodeUri,
        sessionDurationMillis
        slot : {
            id,
            stereotype,
            lastStarted
        }
    }
    grid: {
        uri,
        totalSlots,
        usedSlots,
        sessionCount,
        nodes : [
            {
                id,
                uri,
                status,
                maxSession,
                sessions : [
                       {
                            id,
                            capabilities,
                            startTime,
                            uri,
                            nodeId,
                            nodeUri,
                            sessionDurationMillis
                            slot : {
                                id,
                                stereotype,
                                lastStarted
                            }
                        }
                    ]
               capabilities,
            }
        ]
    }
}
```

## 查询 GraphQL

查询GraphQL的最佳方法是使用`curl`请求. GraphQL允许您仅获取所需的数据, 仅此而已. 

下面给出了一些GraphQL查询的示例. 您可以根据需要构建自己的查询. 

### 查询网格中 `totalSlots` 和 `usedSlots` 的数量:

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { totalSlots, usedSlots } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

通常在本地机器上 `<LINK_TO_GRAPHQL_ENDPOINT>` 会是 `http://localhost:4444/graphql`

### 查询全部会话、及节点以及网格的详情 :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { id, uri, status, sessions {id, capabilities, startTime, uri, nodeId, nodeUri, sessionDurationMillis, slot {id, stereotype, lastStarted } ,uri }, maxSession, capabilities }, uri, totalSlots, usedSlots , sessionCount } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询以获取当前网格的会话总数 :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { sessionCount } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询以获取网格中的最大会话数量 :


```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { maxSession } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询以获取网格中所有节点的全部会话详情 :


```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { sessions { id, capabilities, startTime, uri, nodeId, nodeId, sessionDurationMillis } } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询以获取网格中每个节点中所有会话的插槽信息 :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { sessions { id, slot { id, stereotype, lastStarted } } } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询以获取给定会话的会话信息查询以获取给定会话的会话信息 : 

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ session (id: "<session-id>") { id, capabilities, startTime, uri, nodeId, nodeUri , slot { id, stereotype, lastStarted } } } "}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询网格中每个节点的功能 :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { nodes { capabilities } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询网格中每个节点的状态 :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { nodes { status } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### 查询每个节点和网格的 URI :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { nodes { uri }, uri } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```
