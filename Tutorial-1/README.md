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




