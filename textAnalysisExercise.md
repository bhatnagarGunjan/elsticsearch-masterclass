1. Create a new index called my_index.
2. Define a custom analyzer that uses a lowercase tokenizer, an ASCII folding character filter, and a stop word token filter.
3. Analyze the text "The quick brown fox jumped over the lazy dog" using the custom analyzer.
4. Analyze the same text using the standard analyzer.


**TRY IT YOURSELF FIRST :) **

Custom analyzer
```
PUT my_index
{
  "settings":
  {
    "analysis": {
    "analyzer": {
      "my_analyzer": {
        "type": "custom",
        "tokenizer": "lowercase",
        "char_filter": [
          "html_strip"
        ],
        "filter": [
          "stop"
        ]
      }
    },
    "filter": {
      "stop": {
        "type": "stop",
        "stopwords": "_english_"
      }
    }
  }
  }
}
```
