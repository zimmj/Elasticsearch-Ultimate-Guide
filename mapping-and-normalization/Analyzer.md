# Elasticsearch Analyzer

Analyzer are used to perform text analysis.
It is used, when you index or search for a [text field](./Elasticsearch-Field-Types#Text).
If the search is not working as expected you need to change something with your analyzers.

## Overview

An Analyzer consist of two parts:

* Tokenization
* Normalization

## Tokenization

Tokenization enables full text search.
It breaks down text into smaller parts, which can be matched individually with the search text.

There are different [Tokenizers](./Tokenizer).

## Normalization

Normalization ensures that the same string will match as well.
As Elasticsearch is matching the string literally, we need to make them the same.
So we need to lowercase theme, remove special character and so on.

To see different Normalizer go to the DOC: [Normalizer](./Normalizer)

## Conclusion

The goal of this two part is to bring the text field to a unified form for the search.
So that a specific set of search inputs will find the field.
You need to think about the specific need of the language searched in.
So that seemingly same entries will find the same document.

For example in German, the stings:

* Bäume
* Baeume

Are exactly the same, just different writing style.

These steps are described in the analyzer of Elasticsearch.
We can use the default analyzer or specify our own.

## Analyzer in Elasticsearch

When you create an explicit mapping, you can as well create your own Analyzer to use as a text-field analyzer.
As above explained, they are used to unify the appearance of a text and enable the matching of the user search.

### Parts of the Analyzer

An analyzer consist of three different part:

1. [Tokenizers](./Tokenizer)
2. [Character-Filter](./Character-Filter)
3. [Token Filters](./Token-Filters)

These three parts are run as the order mentioned.
An analyzer always needs a Tokenizer, but might have Filters.
Filters are used for all different things, as they can add, delete or modified token in the generated stream.

### Create Analyzer

Customized Analyzer, Filter and Tokenizer need to be defined in Explicit mapping.
They can't be used globally only on the defined mapping.

A simple example would look something like:

```JSON
{
    "settings": {
        "index": {
            "max_ngram_diff": "5"
        },
        "analysis": {
            "analyzer": {
                "my_analyzer": {
                    "tokenizer": "whitespace",
                    "filter": [
                        "lowercase",
                        "german_normalization",
                        "asciifolding",
                        "my_tokenizer"
                    ]
                }
            },
            "filter": {
                "my_tokenizer": {
                    "type": "ngram",
                    "min_gram": 5,
                    "max_gram": 6,
                    "token_chars": [
                        "letter",
                        "digit"
                    ]
                }
            }
        }
    }
}
```

We defined our own analyzer, which splits our text first in words.
Makes some Normalization on it.
Afterwards, split the words further into smaller tokens with an n-gram filter-tokenizer.

So when we would analyze the sentence:

```Text
 Meine Bäume wachsen nie.
```

Then we will get following tokens to match against:

```Text
 ["meine", "baume", "wachs", "wachse", "achse", "achsen", "chsen"]
```

When you start to think about, how the user should search your text, it's best to try out different Analyzer option and see if they produce what you expect.

There are countless possibilities and the best approach is to think, what do I want to type to find this.

To see more possibilities check out following examples:

* [Fun with Analyzer]
* [How to create a type ahead]
* [Write your own search]
