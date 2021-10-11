# Tokenizer

In case of Elasticsearch, a token is a part of a text field.
Therefore, there are different ways to split a text in smaller pieces, and this is reflected in the [tokenizer of Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-tokenizers.html).

## Word Oriented Tokenizer

Tokenizing full text in words or logical substrings

The most used Tokenizer is the [Standard Tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-standard-tokenizer.html), which splits a text at the word boundaries, as space.
It removes as well punctuation from the text.

Other useful tokenizer might be:

* [UAX URL Email Tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-uaxurlemail-tokenizer.html): Keeps URLs together
* [Classic Tokenizer](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-classic-tokenizer.html): Grammar Based tokenizer for English.

## Partial Tokenizer

Partial Tokenizer are used to break up text and words into small fragments.
There are two types of them.
The [N-Gram](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-ngram-tokenizer.html) and [Edge-N-Gram](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-edgengram-tokenizer.html) tokenizer.

The N-Gram Tokenizer splits a word with a window into fragments.
The Edge-N-Gram makes the same, but always stays at the beginning of the word, text.

## Use of tokenizer

Tokenizers are an important part in creating a search for the user.
Therefore, it is important to understand, how the user is intended to search for the data.
Overtokenizing comes with a cost and should be avoided.

To understand the different ways to tokenize, it is best to try them out by themselves.

#### Practical example

* [Word oriented tokenizer](./examples/Word-Tokenizer.md)
* [N-Gram Tokenizer](./examples/Ngram-Tokenizer.md)
