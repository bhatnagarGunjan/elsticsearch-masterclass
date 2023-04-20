
**Full Text Queries**
#Match
#Find the recipes with tomato in the title
```
GET recipes/_search
{
  "query": {
    "match": {
      "title": "Tomato"
    }
  }
}
```
```
GET recipes/_search
{
  "query": {
    "match": {
      "title": "Tomato sauce"
    }
  }
}
```

#Match phrase : Matches the phrase with slop value. Terms are tokenised.
#Find the recepies with tomato sauce or tomato based sauce in title.
```
GET recipes/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "TOMATO sauce",
        "slop": 2
      }
    }
  }
}
```
#Multimatch : Matches multiple fields to the search string.
#Get the Pasta or Pasta based dishes as per title and description
```
GET recipes/_search
{
  "query": {
    "multi_match": {
      "query": "Pasta",
      "fields": ["title","description"]
    }
  }
}
```

**Term Level Queries**

#Term : Usage reccomended on keyword datatype. Finds exact matches for the search string.
#Let's Search by ingredients?
```
GET recipes/_search
{
	"query": {
		"nested": {
			"path": "ingredients",
			"query": {
				"term": {
					"ingredients.name": "Kosher Salt"
				}
			}
		}
	}
}
```

#Range
#Let's get the recipies which will take less than 15 mins
```
GET recipes/_search
{
  "query": {
    "range": {
      "preparation_time_minutes": {
        "lte": 15
      }
    }
  }
}
```
#Prefix
```
GET recipes/_search
{
  "query": {
    "prefix": {
      "description": {
        "value": "pre"
      }
    }
  }
}
```
#Wildcard
```
GET recipes/_search
{
  "query": {
    "wildcard": {
      "description": {
        "value": "cream*"
      }
    }
  }
}
```
#Fuzzy
#Yes, we can handle the typos as well!
```
GET recipes/_search
{
  "query": {
    "fuzzy": {
      "title": {
        "value": "rasta",
        "fuzziness": "AUTO"
      }
    }
  }
}
```
**Compound Queries**

#Bool queries
#Find all recipes that contain both garlic and Parmesan cheese
```
GET recipes/_search
{
  "query": {
    "bool": {
      "must": [
        {"nested": {
          "path": "ingredients",
          "query": {
            "match": {
              "ingredients.name": "garlic"
            }
          }
        }},
        {
          "nested": {
            "path": "ingredients",
            "query": {
              "match": {
                "ingredients.name": "Parmesan cheese"
              }
            }
          }
        }
      ]
    }
  }
}
```
#Find all recipes that contain both garlic and Parmesan cheese which should take less than 15 min
```
GET recipes/_search
{
  "query": {
    "bool": {
      "must": [
        {"nested": {
          "path": "ingredients",
          "query": {
            "match": {
              "ingredients.name": "garlic"
            }
          }
        }},
        {
          "nested": {
            "path": "ingredients",
            "query": {
              "match": {
                "ingredients.name": "Parmesan cheese"
              }
            }
          }
        }
      ],
      "should": [
        {
          "range": {
            "preparation_time_minutes": {
              "lte": 15
            }
          }
        }
      ]
    }
  }
}
```
#Same as above, just thati dont want tomatoes in my dish!
```
GET recipes/_search
{
  "query": {
    "bool": {
      "must": [
        {"nested": {
          "path": "ingredients",
          "query": {
            "match": {
              "ingredients.name": "garlic"
            }
          }
        }},
        {
          "nested": {
            "path": "ingredients",
            "query": {
              "match": {
                "ingredients.name": "Parmesan cheese"
              }
            }
          }
        }
      ],
      "should": [
        {
          "range": {
            "preparation_time_minutes": {
              "lte": 15
            }
          }
        }
      ],
      "must_not": [
        {"nested": {
          "path": "ingredients",
          "query": {
            "match": {
              "ingredients.name": "tomatoes"
            }
          }
        }}
      ]
    }
  }
}
```
#I am so hugry and just have 15 mins! what can i cook?
```
GET recipes/_search
{
  "query": {
    "bool": {
      "filter": [
        {"term": {
          "preparation_time_minutes": "15"
        }}
      ]
    }
  }
}
```
#Boosting queries
#Find all recipes that contain garlic, but negatively boost recipes which are dry
```
GET recipes/_search
{
  "query": {
    "boosting": {
      "positive": {
        "nested": {
          "path": "ingredients",
          "query": {
            "match": {
              "ingredients.name": "garlic"
            }
          }
        }
      },
      "negative": {
        "bool": {
          "must": [
            {"match_phrase": {
              "steps": "heavy cream"
            }}
          ]
        }
      },
      "negative_boost": 0.5
    }
  },
  "highlight": {
    "fields": {"ingredients.name": {}}
  }
}
```
