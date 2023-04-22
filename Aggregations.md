
**Aggregations**

**Metric Aggregation**
#Stats
```
GET recipes/_search
{
  "size": 0, 
  "aggs": {
    "statsPrepTime": {
      "stats": {
        "field": "preparation_time_minutes"
      }
    }
  }
}
```
#Extendex stats
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "xStatsPrepTime": {
      "extended_stats": {
        "field": "preparation_time_minutes"
      }
    }
  }
}
```
#Cardinality
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "cardinalityPrepTime": {
      "cardinality": {
        "field": "preparation_time_minutes"
      }
    }
  }
}
```
#Value count
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "valueCountPrepTime": {
      "value_count": {
        "field": "preparation_time_minutes"
      }
    }
  }
}
```
#Percentile
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "percentilePrepTime": {
      "percentiles": {
        "field": "preparation_time_minutes"
      }
    }
  }
}
```


**Bucket aggregations**
#Terms
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "termsPrepTime": {
      "terms": {
        "field": "preparation_time_minutes"
      }
    }
  }
}
```
#Range
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "rangePrepTime": {
      "range": {
        "field": "preparation_time_minutes",
        "ranges": [
          {
            "from": 0,
            "to":15
          },
          {
            "from": 15,
            "to":31
          }
        ]
      }
    }
  }
}
```
#Filter
```
GET recipes/_search
{
  "size": 0,
  "aggs": {
    "filterIngredients": {
      "filter": {
        "nested": {
          "path": "ingredients",
          "query": {
            "match": {
              "ingredients.name.keyword": "Kosher salt"
            }
          }
        }
      }
    }
  }
}
```
#Date histogram
```
GET recipes/_search
{
  "size": 0, 
  "aggs": {
    "dateHist": {
      "date_histogram": {
        "field": "created",
        "calendar_interval": "year"
      }
    }
  }
}
```
#Get the average prep time of the recipies which have got 4 or 5 star rating grouped by servings
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
}
```
