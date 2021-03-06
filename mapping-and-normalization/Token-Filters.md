# [Token Filters](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenfilters.html)

These filters are used to change tokens in the token stream generated by the tokenizer.
We can add, delete or remove tokens directly from the stream with different filters.
As we change the stream, the order of this filter matter a lot.

Elasticsearch has many predefined filters, with them, you can mostly find a solution for your search.

For example, when we create following Analyzer:

```json
"my_word_anaylzer": {
  "tokenizer": "whitespace",
  "filter": [
    "lowercase",
    "german_normalization",
    "asciifolding"
  ]
}
```

We will first Tokenize our text, then the filter chain will run over our stream.
Therefore, all tokens get lowercase, and then normalized with the German-normalizer and ascii-folding.

When we would analyze:

```text
 Meine Bäume wachsen nie.
```

We should get following stream:

```Text
[ "meine", "baeume", "wachsen", "nie"]
```

That's how Char-Filters are use.
