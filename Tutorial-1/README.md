### Elastic Search Basics:
Here is the YouTube [video link](https://www.youtube.com/watch?v=8r_IMTerZSY).

---

We save data in Elastic Search using JSON. 

The smallest building block of JSON is __Document.__ It is equivalent to __Rows__ in RDBMS.

A collection of similar documents is called __Index.__ It is like __Database.__

Data inside index, are divided into multiple chunks which are called __Shard.__ Shards make data to be easily searchable and queryable. The chunks can be stored in one, or multiple machines.

We call each machine __Nodes.__

A collection of Shards, is called __Cluster.__

There is a copy of each Shard in cluster, called __Replica__ and if data in shards are lost, replicas still have the data.

(cluster-structure.jpg)

What we have on each node, is a collection of shards, and when we have multiple nodes, the shards will be automatically distributed in them. (node-destribution.jpg)

__IMPORTANT:__ While we have shards distributed in different nodes, the shards with the same cluster name can find each other and relate to each other.

When creating the index, we have to define the schema, documents, etc. They have their own names, such as __Mapping, Post, Properties.__

Mapping is like schema, Post is like table, types-mappings-schemas.jpg

In order to create Index:
```
PUT <index_name>
{
    "mappings": {
    
        "properties": {
                "field1": {"type":"text"},
                "field2": {"type" : "integer"}
            }
    }
}

```

To search all documents in the index:
```
GET employees_details/_search
```

If want to update a document, we just PUT it again, it immutable. The way the update works, is by deleting the old and making the new document.


To search all documents in more than one index:
```
GET /employees_detail, ecommerce_data/_search
```
This returns data from both employees_detail, and ecommerce_data indices.

Sometimes we need to see all documents inside indices starting with ind, then we use wildcards:

```
GET /ind*/_search
```
Sometimes we need to search in a specific type of an index:
````
GET /employees_detail/_doc/search
````
However now I get this message when I run this query:
```
#! Deprecation: [types removal] Specifying types in search requests is deprecated.
```
For getting two types in one index:
```
GET /employees_detail/type1, type2/_search
```
To get all types starting with typ in all indices:
```
GET /_all/typ*/_search
```

#### Search Pagination:

In order to do this, we add the following parameters to the search:
```
GET /employees_detail/_search?size=5&from=0     //it starts from the first page, and returns 5 by 5
GET /employees_detail/_search?size=5&from=5     //it starts from the sixth, and returns 5 by 5
```

#### Search Lite:

When we specify a value for a type directly in the query string:
```
GET /employees_detail/_search?q=name:Anna
```
This querying method is so powerful, and we can easily use it in ad-hoc projects, but isn't recommended for production. some examples:

```
+tweet:foo   // it says the tweet has to be necessarily foo
+name:john   // It says the name has to be necessarily john
+date:>2018-05-01   // It says the date has to be after the specified date
````

But in order to make it a readable query string, we need to use __Percentage Encoding.__ The photo percentage-encoding-example.png shows an example of percentage encoding.

Sometimes we don't know the field name, or we just want anywhere, a specific name is mentioned:
```
GET /employees_detail/_search?q=anna   // It returns anywhere Anna is mentioned no matter what field name
```
