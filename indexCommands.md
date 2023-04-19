Create index with default settings
```
PUT pages
```
Delete index
```
DELETE pages
```
Create index defining shards and replicas
```
PUT products 
{
  "settings":{
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
}
```
Posting a value in index
```
POST /products/_doc
{
  "name": "Coffee Maker",
  "price": 3000,
  "category": "Electronics",
  "in-stock": true,
  "count": 10
}
```
