# N-Gram-Tokenizer

The goal of N-Gram Tokenizer is to split a text with a window into much smaller tokens.
The size of the window can be set to different amount of symbols.
We can as well set a window range. So the text will be split into different sized tokens.

## Example

The predefined N-Gram Tokenizer will split a text into 1 and 2 length token.

The call:

```JSON
POST _analyze
{
    "tokenizer": "ngram",
    "text": "My dog"
}
```

Will split the text into:

```TEXT
    ["M", "My", "y", "y ", " ", " d", "d", "do", "o", "og", "g"]
```

With N-Gram Tokenizer the amount of tokens grows really fast, especially with a wide range of different sized tokens.

To set our own window range, we need to define the tokenizer in an Index.

Like:

```JSON
PUT analyzer_idx
{
  "settings": {
    "index": {
        "max_ngram_diff": "5"
    },
    "analysis": {
      "analyzer": {
        "my_analyzer": {
          "tokenizer": "my_tokenizer"
        }
      },
      "tokenizer": {
        "my_tokenizer": {
          "type": "ngram",
          "min_gram": 3,
          "max_gram": 5,
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

As you can see, we have different options to configure our tokenizer.
The **min** and **max-gram** defines the width of the window.
The **token_chars** defines which chars are seen to belong to a token.
If a char is encounter, which is not allowed, the text will be split.

This we will see, when we try out this tokenizer, with set analyzer:

```JSON
POST anaylzer_idx/_analyze
{
  "analyzer": "my_analyzer",
  "text": "I walkd 2 dogs down the 24th road"
}
```

If you run this example, you will see:

* "I" is not a token as it is two short
* White space are not used, so we don't have the token: "I w"
* We have tokens from the size 3 up to 5
* Numbers are as well-used for tokens.

## Edge-N-Gram

The Edge N Gram is a variation of the N-Gram tokenizer.
It only splits the text from the beginning of a text.
It will split the text otherwise the same way, with the defined window.

When we add change our index mapping to:

```JSON
{
  "settings": {
    "index": {
        "max_ngram_diff": "5"
    },
    "analysis": {
      "analyzer": {
        "my_analyzer": {
          "tokenizer": "my_tokenizer"
        }
      },
      "tokenizer": {
        "my_tokenizer": {
          "type": "edge_ngram",
          "min_gram": 3,
          "max_gram": 5,
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

And then rerun the analysis call for our sentence:

```JSON
POST anaylzer_idx/_analyze
{
  "analyzer": "my_analyzer",
  "text": "I walkd 2 dogs down the 24th road"
}
```

We will see, that now the token always starts from the beginning, but otherwise keeps the same rules as before.

Keep in mind, that the produces tokens are used to match the users input against, and so determine what is found by the input.

## Training Exercise

Try to write Tokenizer which produce following results:

### From

```Text
My secret email is 234.IAmTheBest@master.com. Please never write me.
```

### Goals

First

```Text
["secr", "ecre", "cret", "emai", "mail", "IAmT", "AmTh", ..., "writ", "rite"]
```

Second

```Text
["secr", ..., "mail", "234.", "34.I", "4.IA", ".IAm "IAmT", "AmTh", ..., "writ", "rite"]
```

Third

```Text
["My secre"]
```

Fourth

```Text
["234.IAmT"]
```

Fifth

```Text
["secre", "email", "234.I", "Pleas", "never", "write"]
```

### Solutions

The first and second are using n-gram of 4, whereas the seconds has punctuation as token symbol.

The Third is edge-n-gram without any token_chars, therefore uses all chars.

The fourth has the same token_chars as the second analyzer, but is as well an edge-n-gram.

The fifth is using a smaller window of 5, but is otherwise the same as the fourth.
