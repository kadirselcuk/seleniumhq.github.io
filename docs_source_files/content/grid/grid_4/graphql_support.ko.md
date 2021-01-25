---
title: "GraphQL querying support"
weight: 4
---

{{% notice info %}}
<i class="fas fa-language"></i> Page being translated from
English to Korean. Do you speak Korean? Help us to translate
it by sending us pull requests!
{{% /notice %}}

GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. It gives users the power to ask for exactly what they need and nothing more.

## Enums
Enums represent possible sets of values for a field.

For example, the `Node` object has a field called `status`. The state is an enum (specifically, of type `Status`) because it may be `UP` , `DRAINING` or `UNAVAILABLE`.

## Scalars
Scalars are primitive values: `Int`, `Float`, `String`, `Boolean`, or `ID`.

When calling the GraphQL API, you must specify nested subfield until you return only scalars.


## Structure of the Schema
The structure of grid schema is as follows:

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

## Querying GraphQL

The best way to query GraphQL is by using `curl` requests. GraphQL allows you to fetch only the data that you want, nothing more nothing less.

Some of the example GraphQL queries are given below. You can build your own queries as you like.

### Querying the number of `totalSlots` and `usedSlots` in the grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { totalSlots, usedSlots } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

Generally on local machine the `<LINK_TO_GRAPHQL_ENDPOINT>` would be `http://localhost:4444/graphql`

### Querying all details for session, node and the Grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { id, uri, status, sessions {id, capabilities, startTime, uri, nodeId, nodeUri, sessionDurationMillis, slot {id, stereotype, lastStarted } ,uri }, maxSession, capabilities }, uri, totalSlots, usedSlots , sessionCount } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Query for getting the current session count in the Grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { sessionCount } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Query for getting the max session count in the Grid :


```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { maxSession } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Query for getting all session details for all nodes in the Grid :


```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { sessions { id, capabilities, startTime, uri, nodeId, nodeId, sessionDurationMillis } } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Query to get slot information for all sessions in each Node in the Grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ grid { nodes { sessions { id, slot { id, stereotype, lastStarted } } } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Query to get session information for a given session: 

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query":"{ session (id: "<session-id>") { id, capabilities, startTime, uri, nodeId, nodeUri , slot { id, stereotype, lastStarted } } } "}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Querying the capabilities of each node in the grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { nodes { capabilities } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Querying the status of each node in the grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { nodes { status } } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```

### Querying the URI of each node and the grid :

```shell
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ grid { nodes { uri }, uri } }"}' -s <LINK_TO_GRAPHQL_ENDPOINT>
```
