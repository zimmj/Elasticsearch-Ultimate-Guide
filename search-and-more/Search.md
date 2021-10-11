# Search

Elasticsearch is a Search-Machine.
Search is therefore, one of the main feature of Elasticsearch and should be understood.

The whole search is written in [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html).
It is based on JSON and is simple to read and write.
This JSON will is send to the [search endpoint](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html) of Elasticsearch.
We can perform a search on the whole cluster or on a specific Index or Alias of Index.

## Basics about the search

A search consist of standalone Queries or compound Queries.
The Compound Queries are usually the bool compound, which combines queries logically.

### [Bool Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html)

The bool Query has four different stages:

* must
* filter
* should
* must_not

Filter and must_not are executed in the [filter context](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html), which means, that it's result is not used to calculate the score for the hit.

To safe calculation time, it is useful to run basic exclusion filter in the filter context.
As no score need to be calculated.
Otherwise, we can build the most complicated search with several bool queries.

## Queries

For the most cases we only need 3 Query types and their deviates:

* [Match-Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html)
* [Term-Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html)
* [Range-Query](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html)

### Term and Match Query

This both queries are almost the same, but the term query only works with exact matches on a specific field.
As the Match query is the standard query for the text based search.
As it supports fuzzy matching.

The reason we distinct this two queries is, that the term query is really specialized and therefore really fast.
As the Match query has more options.

To see them in action see practical example:

* [Some simple queries](todo)
* [Build your own queries](todo)
* [How to build a Type ahead](todo)
