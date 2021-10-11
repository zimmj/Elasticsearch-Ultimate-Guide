# Elasticsearch Filed Types

Elasticsearch has specific field types.
Each of these types have specific function and searchability.
It's really important to understand when you use which type of field for your data.
And how to optimise your data structure.

## Field Types [Guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-types.html)

Elasticsearch has really specialized field types, like the Aggregate data types.
These types are rarely needed, and we will concentrate us on the used and to avoid ones.
As we mostly only use them in mappings.

## Common Types

* Binary
* Boolean
* Keywords
* Numbers
* Dates

The use of these fields is straight forward.
They behave the same, as in the most programming language.
All of them can only be [searched](link-to-search-todo) with the same value, as it is indexed.
Additional to it, this field can be [aggregated](link-to-aggregation-todo) to generate statistical inside views.

## Object field types

* object
* flattened
* nested
* join

### [object type](https://www.elastic.co/guide/en/elasticsearch/reference/current/object.html)

___

This field defined a flat single object with inner fields.
The inner fields can be reached like: object.inner_field.
Object are not used for Arrays, for it consider [nested object fields](#nested-field)

### [flattened type](https://www.elastic.co/guide/en/elasticsearch/reference/current/flattened.html)

___

This field type takes all the field in class together and put it in a searchable flat field.
All the flattened values are considered keywords.
Therefore, all queries, which can handle them, can be used to search through it.

When we have the following class:

```json
{
    "toFlattened": {
    "first": "first field",
    "second": "second field"
    }
}
```

It will be transformed into something like:

```json
{
    "toFlattened": [
        "first field",
        "second field"
    ]
}
```

Therefore, all the values still can be matched, but we lose all the meta information about the field in the object.
This benefits us, that we can map an entire object to one field, regardless how many unique fields it has.
So we can stop a mapping excess.
[Example how to use.](./examples/Flattened-Field.md)

### [Nested type](https://www.elastic.co/guide/en/elasticsearch/reference/current/nested.html)

___

This datatype is used, to describe object array in the mapping.
It will generate extra document for each object saved in this list.
This comes with cost and is as well limited by index-limits.

For this extra cost, we can run a search, which keeps the context of this field in Mind.
We can find the object, which have in two specific fields, of an object, the searched values.
For an example, go to [practical example](nested-example-todo)

### [Join type](https://www.elastic.co/guide/en/elasticsearch/reference/current/parent-join.html)

___

Join create a parent child relationship in with two documents in the same index.
A field can describe a single relation from these documents.

As this field resemble logic from SQL, join statement.
It is not really optimised for search and has a big cost in data usage as well.

Because of these reasons, this field should not be used at all.
We should try to only have a denormalized flat JSON structure.
This structure is simple to search and analyze.

## Text search Types

### [Text](https://www.elastic.co/guide/en/elasticsearch/reference/current/text.html)

___

This is the most used field.
It allows analyzing text and optimise it for search.
The analyzer is parsing the text and transforms it to list of individual terms, which are then indexed.

This process allows Elasticsearch to make complex partial search on text.

The most important Parameters are [analyzers](./Analyzer.md):

#### analyzer

Set of rules how the text should be analyzed at index-time and search-time, as long it is not overwritten.

#### Search-analyzer

Overwrites the analyzer on search-time

### [Search as you type](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-as-you-type.html)

___

This field is a special field, which provides an out-of-the-box search by Elasticsearch.
It generates its own subfields, which are used for the search itself.

It's perfect option for writing a quick partial-search on a field, but has it own troubles.

See [Search as you type, practical look at](todo-look-at)

## Arrays

Elasticsearch has no specific type for Arrays, which is sometimes confusing for object fields.
As it could be an array or just an object.

All values in an array, need to have the same type.

## Multi-fields

This allows to analyze a field in different ways.
So we can analyze text field with different language normalizer.
Therefore, we can take the uniqueness of a language in account.
Most field-types supports multifield with the [fields](https://www.elastic.co/guide/en/elasticsearch/reference/current/multi-fields.html) parameter.

This is already used in the dynamic mapping by Elasticsearch.
Text field always get a subfield keyword, to keep the exact value of the field searchable.

In queries, we can directly point to these subfields the same way as fields in object.
With this ability, we can write different queries on the same data-structure.
You will this subfield in many cases.

```json
"text-number": {
    "type": "text",
    "fields": {
        "keyword": {
            "type": "keyword",
            "ignore_above": 256
        }
    }
}
```

## Other Fields

There are plenty other specialized fields from Elasticsearch, like geo_point.
If you have special data types, check them out by yourself.
