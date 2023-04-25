Please create a new index called products and load the products-bulk.json in it. 
(delete the pre-existing products index if u have created before)

You can bulk load the data from : https://github.com/bhatnagarGunjan/elsticsearch-masterclass/blob/main/bulk-data/products-bulk.json

Try below Queries:

**1. Create an analyzer supporting this search: Example = Name: Apple pie | search strings: ap, pi, pp **
```
GET _analyze 
{
  "tokenizer": "ngram",
  "text": ["apple pie"]
}
```

**2. How many products have ‘Beverage’ tag?**
```
GET products/_search
{
  "query": {
    "term": {
      "tags.keyword": "Beverage"
    }
  }
}
```
**3. How many of above products are active?**
```
GET products/_search
{
  "query": {
    "term": {
      "is_active": {
        "value": "true"
      }
    }
  }
}
```
**3. How many products have sale > 100 after 2015**
```
GET products/_search
{
  "query": {
    "bool": {
      "must": [
        {"range": {
          "sold": {
            "gte": 100
          }
        }},
        {
          "range": {
            "created": {
              "gte": "2015/12/31"
            }
          }
        }
      ]
    }
  }
}
```
**5. •Search for the products which are “Vegetable”. Give preference to the ones whose sale is >150 and negative boost the ones whose price is >200**
```
GET products/_search
{
  "query": {
    "boosting": {
      "positive": {
        "bool": {
          "must": [
            {"term": {
              "tags.keyword": {
                "value": "Vegetable"
              }
            }}
          ],
          "should": [
            {"range": {
              "sold": {
                "gte": 150
              }
            }}
          ]
        }
      },
      "negative": {
        "range": {
          "price": {
            "gte": 200
          }
        }
      },
      "negative_boost": 0.5
    }
  }
}
```
