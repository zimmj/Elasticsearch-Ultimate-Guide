# Dynamic Mapping exercise

For trying out dynamic mapping we simply can send a document to a not existing index.
Like in this call:

 ```json
 POST /new_idx/_doc
 {
    "float": 12.3,
    "long": 12,
    "text-number": "12",
    "date": "2015/09/02"
 }
 ```

Elasticsearch Create for us now a new index with a mapping corresponding to the document we send.
We now can get the generated mapping with:

 ```json
 POST /new_idx/_mapping
 
 Result:
{
    "new_idx": {
        "mappings": {
            "properties": {
                "date": {
                    "type": "date",
                    "format": "yyyy/MM/dd HH:mm:ss||yyyy/MM/dd||epoch_millis"
                },
                "float": {
                    "type": "float"
                },
                "long": {
                    "type": "long"
                },
                "text-number": {
                    "type": "text",
                    "fields": {
                        "keyword": {
                            "type": "keyword",
                            "ignore_above": 256
                        }
                    }
                }
            }
        }
    }
}
 ```

As we can see, Elasticsearch generated for us the mapping of the index.
If we now try to index a new document with not corresponding field values.
It will handle it differently.
If we try to add a document with a text in a number field, it will be rejected:

```json
 POST /new_idx/_doc
 {
    "float": 12,
    "long": "1.12d",
    "text-number": "12",
    "date": "2015/09/02"
 }
```

The above request will be rejected with an error.
But if we try to index a float instead of a long, the document will be not rejected.
The analyzed value will be instead transformed to a long.
When we index following document:

```json
 POST /new_idx/_doc
 {
    "float": 12,
    "long": "1.12",
    "text-number": "12",
    "date": "2015/09/02"
 }
```

We can't find the document with a match query on the field long, with the value: 1.12.
Instead, we will find it with a match query on with the value 1.

```json
GET /new_idx/_search
{
  "query": {
    "match": {
      "long": {
        "query": "1"
      }
    }
  }
}
```

It seems like the value gets cast internally to a long.
Therefore, the document can only be found by the cast value.

## Conclusion

Dynamic mapping is a great tool to start mapping your data, and then explore them in Kibana or with queries.
It has some quirks, which you need to be mindful of.
For example is auto number detection for text turned off.
That's why text-number was analyzed as text and not as number.

Or the described trouble with long and float values.

### Turn on Auto Number detection

To turn on the auto number detection, the settings of the mapping need to be set like following:

```json
PUT /new_idx
{
  "mappings": {
    "numeric_detection": true
  }
}
```

If we now add the docs from before to it.
We will get following mapping:

```json
{
    "new_idx": {
        "mappings": {
            "numeric_detection": true,
            "properties": {
                "date": {
                    "type": "date",
                    "format": "yyyy/MM/dd HH:mm:ss||yyyy/MM/dd||epoch_millis"
                },
                "float": {
                    "type": "float"
                },
                "long": {
                    "type": "float"
                },
                "text-number": {
                    "type": "long"
                }
            }
        }
    }
}
```

The field, text-number is now a long and not a text field anymore.
