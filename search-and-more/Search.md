# Search

Elasticsearch is a Search-Machine.
Search is therefore, one of the main feature of Elasticsearch and should be understood.

The whole search is written in [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html).
It is based on JSON and is simple to read and write.
This JSON will is send to the [search endpoint](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html) of Elasticsearch.
We can perform a search on the whole cluster or on a specific Index or Alias of Index.

## Basics about the search

The search is build out of two Elements:

* Query Clauses
* Compound query clauses

With both together we can define exact queries for our own.
