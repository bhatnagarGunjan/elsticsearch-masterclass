#Aggregation
#Get the average prep time in the recipies with avg rating of 4 grouped by servings
```
GET recipes/_search
{
  "query": {
    "bool": {
      "filter": [
        {"range": {
          "ratings": {
            "gte": 4
            }
        }}
      ]
    }
  }, 
  "aggs": {
    "avgPrepTime":{
      "avg": {
        "field": "preparation_time_minutes"
      }
    },
    "groupByServings":{
      "terms": {
        "field": "servings.max",
        "size": 10
      }
    }
  }
  ```
