# Mapping

> Mapping is the process of defining how a document, and the fields it contains, are stored and indexed. [Elasticsearch Guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.14/mapping.html)

The mapping can be created in two ways, either we use [Dynamic Mapping](#dynamic-mapping) or we are using [Explicit Mapping](#explicit-mapping).
The dynamic mapping is useful to understand and explore your data, but in production we should always use Explicit Mapping.

The mapping can only consist of specific field defined from Elasticsearch.
On these field, different operation and search can be performed.
Explanation about fields you can find here [Place Holder](Place-holder)

## Dynamic Mapping [Elastic Dynamic Mapping Guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.14/dynamic-field-mapping.html)

Dynamic Mapping allows you to simply add new Data to your index.
With this feature on, you can explore your data and start to build understanding for it.
Just by adding new Documents to the index, Elasticsearch will expand the mapping and allows you to search and see the new fields.

### Which Type is chosen

Elastic search takes the JSON datatype and maps it to a predefined type in the mapping.
See table below:

| JSON data type | Dynamic Mapping |
| --- | --- |
| null | No field is added |
| true or false | boolean |
| double | float |
| integer | long |
| object | object |
| array | depends on first not null value |
| string | depends on working detection: number or date
| text | if detection found nothing, text field with keyword subfield is generated|

Dynamic mapping has the trouble, that when the first document has a value, which differ from the other, o wrong mapping type could be detected.
For example:

```JSON
    {
        "amount": "23"
    }
```

Will be mapped to a long field type. But if we are talking about money amount then we could have:

```JSON
{
    "amount": "23.3"
}
```

This document will not be rejected, but Elasticsearch will not save the exact value.
To find this document, the search need to be for 23 not 23.3. ([How to search](Place-holder))
But non error will be thrown, anymore.

When you still want to use dynamic mapping, it's important to keep in mind, that there are [limit settings](#mapping-limit-settings).
These settings try to keep the mapping in bay.

### Practical examples

[Dynamic Mapping Exercise](./examples/Dynamic-Mapping-Exercise.md)

## Explicit Mapping [Elastic Guide](https://www.elastic.co/guide/en/elasticsearch/reference/7.14/explicit-mapping.html)

As soon as the data is know, a mapping should be written.
As the expected type of field is known, there should be no trouble with indexing new documents.

An explicit mapping should be used.
Firstly to avoid mapping error on runtime, and secondly to improve and fine tune the search.
With Explicit mapping we can tell Elasticsearch how a field should be saved in the search tables.

### Create Explicit Mapping

Explicit mapping is set at index creation time or can be added at runtime.
Existing field can't be updated.
This action could invalidate Documents in Elasticsearch.
Therefore, it is forbidden.

#### Set explicit mapping

```json
PUT /my-index 
{
  "mappings": {
    "properties": {
      "age":    { "type": "integer" },  
      "email":  { "type": "keyword"  }, 
      "name":   { "type": "text"  }     
    }
  }
}
```

The call above creates a new Index with the explicit mapping set.

#### Change mapping on runtime

We can add new field to a mapping without any troubles.

```json
PUT /my-index/_mapping
{
  "properties": {
    "employee-id": {
      "type": "keyword"
    }
  }
}
```

But we can't update existing mapping, when there are already indexed data in Elasticsearch.
To Change a mapping on runtime, we need to create a new Index, as shown above and reindex the old index into the new index.
This is described in [Change Mapping in Production](Link-to-docu-todo)

To create an explicit mapping it is important to understand the different field types given by Elasticsearch.
The field types are described in [Elasticsearch Field Types](./Elasticsearch-Field-Types.md).

#### Control the field

When the mapping is written, we can set many parameters to the field.
All the options are described in [Fields in Mapping](todo).

Important to mention are settings to control if a field should be indexed (Searchable) or counted (aggregation) at all.
When we add:

```json
 "index": false
 "doc_values": false
```

To a countable field, this field can't be searched or aggregated.

* Index: false:
Describes that the field is not indexed.
The field is not written into search table.
Therefore, the field is not searchable
* doc_values: false
Describe that the field is not counted in the doc values table.
Therefore, no aggregation can be made with the field.

We should use them, when we now that the field is not used for search or aggregation at all.
The use of this exact configuration of the mapping reduced the index time of a document.
It decreases as well the size of an indexed document.

## Mapping limit settings

These settings help to keep the mapping explosion in bay.
Under Mapping explosion we understand, that many new fields are added.
That there are to many nested field in List and so on.

To see the exact settings got to [Elasticsearch Mapping limit settings](https://www.elastic.co/guide/en/elasticsearch/reference/7.14/mapping-settings-limit.html).

### Practical Examples

todo
