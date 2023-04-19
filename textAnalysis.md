#Tokens created by standard analyser
```
GET _analyze 
{
  "analyzer": "standard", 
  "text" : "The 2 QUICK Brown-Foxes jumped over the lazy dog's bone."
}
```
Let's add some documents to a index and have a look at the tokens!

Populating the index
```
PUT /products/_doc/1
{
  "product_name": "Apple AirPods Pro",
  "description": "The Apple AirPods Pro offer immersive sound, noise cancellation, and customizable fit for all-day comfort."
}
```
```
PUT /products/_doc/2
{
  "product_name": "Samsung Galaxy Watch 4",
  "description": "The Samsung Galaxy Watch 4 is a stylish and feature-packed smartwatch that tracks your fitness, monitors your heart rate, and connects seamlessly to your Samsung phone."
}
```
```
PUT /products/_doc/3
{
  "product_name": "Amazon Echo Dot",
  "description": "The Amazon Echo Dot is a compact smart speaker that can play music, answer questions, and control your smart home devices using Alexa voice commands."
}
```
```
PUT /products/_doc/4
{
  "product_name": "Sony PlayStation 5",
  "description": "The Sony PlayStation 5 is a powerful gaming console that delivers stunning graphics, lightning-fast load times, and immersive gameplay experiences."
}
```
```
PUT /products/_doc/5
{
  "product_name": "Garmin Forerunner 945",
  "description": "The Garmin Forerunner 945 is a GPS smartwatch designed for serious athletes and fitness enthusiasts, featuring advanced metrics for running, cycling, and swimming, as well as onboard music storage and Garmin Pay."
}
```
Search the products 
```
GET /products/_search
{
  "query": {
    "match": {
      "description": "apple"
    }
  }
}
```

**Character filters**
Html strip
```
GET /_analyze
{
  "tokenizer": "standard",
  "char_filter": [
    "html_strip"
  ],
  "text": "<p>I&apos;m so <b>happy</b>!</p>"
}
```
Mapping
```
GET /_analyze
{
  "tokenizer": "keyword",
  "char_filter": [
    {
      "type": "mapping",
      "mappings": [
        "٠ => 0",
        "١ => 1",
        "٢ => 2",
        "٣ => 3",
        "٤ => 4",
        "٥ => 5",
        "٦ => 6",
        "٧ => 7",
        "٨ => 8",
        "٩ => 9"
      ]
    }
  ],
  "text": "My license plate is ٢٥٠١٥"
}
```
**Tokenizer**
Ngram tokenizer
```
GET _analyze
{
  "tokenizer": "ngram",
  "text": "Quick Fox"
}
```
**Token filter**
Stemmer filter
```
GET _analyze
{
  "tokenizer": "standard",
  "filter": ["stemmer"],
  "text": "foxes jumping so high"
}
```
