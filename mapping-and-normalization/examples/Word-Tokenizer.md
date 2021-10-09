# Word Tokenizer

The Goal of this Tokenizer is to split a sentence into word Tokens.
Therefore, a text like:

```TEXT
        I walk my Dog down the street.
```

Will be Tokenized into:

```text
    [I, walk, my, Dog, down, the, street]
```

The [standard Tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-tokenizer.html) will generate the shown output above.
To see it by yourself just run:

```JSON
POST /_analyze
{
  "tokenizer": "standard",
  "text": "I walk my Dog down the street."
}
```

With this call we can try out all predefined tokenizer from Elasticsearch.

For example, we easily can try out the [URL Tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-uaxurlemail-tokenizer.html):

```JSON
POST /_analyze
{
  "tokenizer": "uax_url_email",
  "text": "Meine Hompage ist: www.joel.zimmerli.swiss."
}
```

This tokenizer is recognizing URL and keep them intact.
Does not split the URL in its subparts.

## Conclusion

Elasticsearch has great predefined word tokenizer.
They work fine with the most languages.
For more complicated language, more grammar based tokenizer, like classic tokenizer is needed, to split sentences in sensible word tokens.
