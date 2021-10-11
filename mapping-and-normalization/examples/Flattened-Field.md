# Example for flattened field

## Basic Dynamic Mapping

The Flattened field is a simple field to save count of documents.
It's used to denormalize the data in Elasticsearch, with less work on changing the data structure by ourselves.

If we add following document to a new index, we will get a dynamic mapping.
With this mapping all fields can be matched against.
[Delete old index](./Deelte-Index.md)

```json
POST new_idx/_doc
{
    "object": {
        "float": 12,
        "long": 1.12,
        "text-number": "12",
        "date": "2015/09/02"
    }
}
```

So that means, we can match against the field long in object like:

```json
GET _search
{
  "query": {
    "match": {
      "object.long": {
        "query": "1.12"
      }
    }
  }
}
```

We will get back the indexed document.

## Explicit Mapping

To show the different, we will create an index with following mapping:
[Delete old index](./Deelte-Index.md)

```json
PUT new_idx
{
  "mappings": {
    "properties": {
        "object": {
            "type": "flattened"
        }
    }
  }
}
```

After we added a document to this index, we can find the document as follow:

```json
GET _search
{
  "query": {
    "match": {
      "object": {
        "query": "1.12"
      }
    }
  }
}
```

We lost the context for the search, but all fields can be found by exact matches.
