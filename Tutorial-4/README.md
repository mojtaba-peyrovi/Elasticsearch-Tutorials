### Elastic Search Series

Resource is in [TechieLifestyle](https://www.youtube.com/watch?v=bA6sYTFwuTc&list=PLGZAAioH7ZlO7AstL9PZrqalK0fZutEXF&index=4) Youtube channel.

---

Multi Get API:
```
GET /_mget
{
    "docs" : [
       {
         "_index":<index_name>,
         "_type":<type_name>,
         "_id":<document_id"       
      },
      {
       ***Criteri2***       
      }و
      }
       ***Criteria3*** 
      }    
   ]
}
```
It returns all documents with this id. We can add as many criteria as we want, and ES will return the results.

Here is an example:
```
GET /_mget
{
  "docs" : [
    {
      "_index": "kibana_sample_data_ecommerce",
      "_type":"_doc",
      "_id": "tYSQuWoBTWxYcfcRDFar"
       
    },
    {
      "_index" : "kibana_sample_data_ecommerce",
      "_type":"_doc",
      "_id" : "toSQuWoBTWxYcfcRDFar"
    }
  ]
}
```

__BULK API:__ we can use it to do update, get, delete, etc. in bulk.    

Here is how we create a document inside an index called test and type of _doc:
```
POST _bulk
{"index":{"_index":"test","_type":"_doc","_id":1}}
{"field1":"value1"}
```
From now on, because we have the index already, we need to create documents like this:
```
POST _bulk
{"create": {"_index":"test","_type":"_doc","_id":2}}
```
The power is, we can make multiple documents by writing the above line for each record, and with one POST call they will be in the index.

We have similar API for update and delete. like this:
```
POST _bulk
{"index":{"_index":"test","_id":2}}
{"field1":"value1"}
{"create":{"_index":"test","_id":3}}
{"field1":"value3"}
{"update":{"_id":"1","_index":"test"}}
{"doc":{"field2":"value2"}}
```
`Specifying the type in the call is deprecated, so I removed them for above code.`

What is important, is that the bulk API is space sensitive between lines, we can't leave empty lines.

We can also reindex like this:
```
POST _reindex
{
    "source": {
        "index":<index1>
    },
    {
    "dest":{
        "index":<index2>
    }
}
```
#### SEARCH API:

When we say GET /<index_name>/_search, by default it shows 10 results.

Another way is to say:
```
GET _all/_search // it returns all data in all indecies
```
In order to search for semething using this, we say:
```
GET _all/_search?q=<field_name>:<query_value>
```

A Better way, is to use __QUERY DSL.__ here is an example:
```
GET /kibana_sample_data_ecommerce/_search
{
  "query": {
    "match": {
      "day_of_week": "Monday"
    }
  }
}
```
Whatever we say inside "query", is called query DSL. We can also provide FROM and SIZE to paginate the results of the search.

Here is how to add pagination:
```
GET /kibana_sample_data_ecommerce/_search
{
  "size":5,
  "from": 0,
  "query": {
      "match": {
      "day_of_week": "Monday"
    }
  }
}

```
ٌWe can also have as many sort criteria as we like, and any of them can be reversed too.

Here is an example:
```
GET /kibana_sample_data_ecommerce/_search
{
  "size":5,
  "from": 0,
  "query": {
      "match": {
      "day_of_week": "Monday"
    }
  },
  "sort": [
    {
      "manufacturer.keyword": {
        "order": "desc"
      }
    }
  ]
}
```
keyword is the data type we defined for the property Manufacturer in mapping.

__Query DSL:__

There are two types of query DSL:

1) Leaf Query Clauses: We are looking for a particular value in a particular field. 

2 ) Compound query clauses: it wraps leaf or other compound queries and it's used to combine multiple queries ina logical fashion.


 __Full Text query:__ is searching through text values, which is so difficult using RDBMS. It is so powerful. we can search a word within a full text field. For exmaple for the ecommerce database, we can search the word women's to get all women's products:
 ```
 GET /kibana_sample_data_ecommerce/_search
 {
    "query": {
        "match" : {
            "category":"women's"
        }   
    } 
 }          // this returns all documents where women's exists in the category field. 
 ```
 There is another search which is more precise, when we have more than one word. It find the exact match. It is called match_phrase.
 
  ```
  GET /kibana_sample_data_ecommerce/_search
  {
     "query": {
         "match_phrase" : {
             "category":"women's accessories"
         }   
     } 
  }          // this returns only the ones with women's accessories exact value. 
  ```   
 There is another concept, called __slop.__ It means how many words of the distance between words is acceptable for match_phrase. Here is an example:
 ```
GET /kibana_sample_data_ecommerce/_search
{
  "query": {
    "match_phrase": {
      "category": 
      {
        "query":"borwn dog",
        "slop": 3
        }
    }
  }
}

```
It means in the property(field) of category, look for "brown dog" but any distance less than 3 between the words is acceptable.

- The other search technique, is match_phrase_prefix which means look for the two words, but consider the first one as the prefix, it doesn't return values where the order is wrong. example:
```

GET /kibana_sample_data_ecommerce/_search
{
  "query": {
    "match_phrase_prefix": {
      "category":"brown dog"
    }
  }
}   // it returns wherever brown is behind dog exactly
```  
The last thing here, is called match_all(), which returns everything.
```
GET /kibana_sample_data_ecommerce/_search
{
    "query": {
        "match_all":{}
    }
}
```

__Query String:__ 

Here is the first example for query string:

```
GET kibana_sample_data_ecommerce/_search
{
  "query": {
    "query_string": {
      "default_field": "customer_full_name",
      "query": "Eddie Underwood",
      "default_operator": "OR"
    }
  }
}
```  
It finds wherever customer_full_name contains either Eddie or Underwood. By default the operator is OR, but we can change it to AND, which makes it exact.

There is another scenario, where we want to compare two values in two different fields then we do it this way:
```
GET kibana_sample_data_ecommerce/_search
{
  "query": {
    "query_string": {
      "fields":["customer_firs_name","customer_last_name"],
      "query": "Eddie Underwood",
      "default_operator": "OR"
    }
  }
}  
```    
Instead of default_field we say fields, and instead of one field name, we add an array of values.

__Term Query:__ It returns documents that contain an exact term in a provided field. 

Here is an example:
```
GET kibana_sample_data_ecommerce/_search
{
  "query": {
    "term": {
      "customer_first_name.keyword": {
        "value": "Eddie",
        "boost": 2
      }
    }
  }
}  // Boost is a float value that specifies the relevance to the value defaut is 1.0
```
We don't use __term__ for query for text fields. We use them for keywords. 

