#### Elasticsearch Tutorial | Getting Started with Elasticsearch | ELK Stack Training | Edureka:

Here is the [link]() to the YouTube page.

Elasticsearch is:

- Open source and free
- Java based
- Uses Restful API
- Is built on the top of Apache Lucene.
- used for single page application projects.
- Document based, instead of tables and schemas

__Why Elastic Search:__ It can combine many types of searches like structured, unstructured, geo, metric, etc. Simply you can find as complicated answers as possible.

Also, it can help analyize billions of log lines easily and it provides aggregation which helps us zoom out data to explore trends and patterns in data.

here: elastic-search-use-case-1,2,3.jpg  is an example of how ES can be so powerful.

Elastic search in multilingual.

#### Installation:

We just download the appropriate version, then inside bin folder, we run in terminal:
```
elasticsearch.bat 
```

After it runs, we open this link:
```
localhost:9200
```
If it shows a json, it means that it is working.

In order to use it easier, we can add this extension to Chrome: __Sense__, in case we work locally. If not, we need to set it up on AWS.

Seems like Sense has been deprecated. The extension substitutes it is called __ElasticSearch Toolbox.__

__Some concepts:__

1) ES is near real-time because it is so quick.
2) It is distributed means that it can be run along multiple clusters. A cluster is a collection of nodes. By default, the cluster name is __elasticsearch__ but it is customizable.
3) Node is a single server which participates in the cluster.

#### Index and Type: 
photo: index-type.jpg


#### Document, Shard, and replica:
photo: document-shard-replica.jpg


#### API Conventions:

__Multiple Indices:__ photo: api-convention-multiple-indices.jpg

__Date, math support in index name:__ photo: api-convention-date-math-support-in-index-name.jpg

__Common Options:__ api-convention-common-options.jpg

__URL based access control:__ api-convention-url-based-access-control.jpg

#### ES different API's:

- __Document API__: It consists of two categories:
    
    1) Single document API (Index API, Get API, Update API, Delete API)
    2) Multi-document API (Multi get API, Bulk API, Delete by query API, Update by query API, Reindex API)
    
 (photo: sample-document-api.jpg)
 
    
- Search API

    1) Multi-index: we can search for the documents present in all the indices or in some specific indices.
    2) Multi-type:  we can search for all documents in an index across all types or in some specific types.
    3) URI-search: various parameters can be passed in a search opearation: (q, lenient, fields, sort, etc.)
    (photo: sample-search-uri.jpg)
    (photo: sample-search-multi-index.jpg)
- Aggregation : here is the syntax: aggregation-syntax.jpg

    There are different syntax types:
   
     1) Bucketing
     2) Metric
     3) Matrix
     4) Pipeline
     
- Index API : We can do all operations on indices as we see here: index-api.jpg
- Cluster API : It is used for getting information about cluster and its nodes and making changes in them. cluster-api.jpg


#### Query DSL(Domain Specific Language):

as we see in this photo: query-dsl.jpg, there are two types of DSL clauses:
   
   1) Leaf Query Clauses: works on a particular value.
   2) Compound Query Clauses: works on multiple leaf structures.
   
#### Mapping: 
Mapping is the process of defining how a document, and the fields that it contains are sorted an indexed.

Here are some mapping parameters: mapping-parameters.jpg
   


