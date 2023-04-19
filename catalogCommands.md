Get cluster health
```
GET /_cluster/health
```

Compact and aligned text (CAT) APIs
Get compact details on the existing nodes
```
GET /_cat/nodes?v
```
Get low level details of the nodes
```
GET /_nodes
```

Get the custom indices. Add parameter expand_wildcards=all to get the system indices
```
GET /_cat/indices?v
```
Get shards
```
GET /_cat/shards
```
