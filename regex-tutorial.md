# Computer Science for JavaScript: Regular Expressions (RegEx) Tutorial

Regular expressions, also known as RegEx, are a "sequence of characters and symbols to define a pattern of text." These patterns of text are used to automate the search and replacement of text, and they provide great flexibility in the automation of such task. (Morrison, Michael et. al. _JavaScript® Bible, Seventh Edition_. Wiley, 2010.)

Regular expressions allow software developers to implement search and replace operations that would be a nightmare if implemented by brute force. Some popular applications include:

- matching of password patterns,
- email format checker,
- smart character replacement, and
- old school function to arrow function

An explanation of the use cases above by Fernando Doglio can be found at _medium.com_: ["Four practical use cases for regular expressions"](#https://blog.bitsrc.io/4-practical-use-cases-for-regular-expressions-b6ab140894fd).

## Summary

Regular expressions are oftenly mystified as advanced material in the coding arena. For starters, they require an advanced understanding of boolean logic and feature recognition, and a very good understanding of the programming language where they are to be implemented. If the logic and features conforming the regular expression is poorly defined, debugging a piece of code that uses regular expressions to validate inputs or format outputs can be a real nightmare.

Familiarizing oneself with the structure of regular expressions can save us, developers, precious time in coding new pieces of software and debugging existing code. There are a few excellent books and resources explaining regular expressions at its core. One of such books is the [Javascript Bible](#https://learning.oreilly.com/library/view/javascript-bible-seventh/9780470526910/), and another is a post by Slawomir Chodnicki, who provides a full description of regular expression patterns in ["Everything you need to know about regular expressions"](#https://towardsdatascience.com/everything-you-need-to-know-about-regular-expressions-8f622fe10b03).

The main objective of my post is not to portray regular expressions as an easy or complex subject, but rather, to describe one of the most interesting applications of regular expressions in **Computer Science (CS)** and **Natural Language Processing (NLP)**: _word tokenization_, the segmentation of text into words as applied in **Machine Learning (ML)** and **Text Mining**; for example, given the following versicles from The Bible:

```bash
1. In the beginning God created the heaven and the earth.
2. And the earth was without form, and void; and darkness was upon the face of the deep. And the Spirit of God moved upon the face of the waters.
3. And God said, Let there be light: and there was light.
4. And God saw the light, that it was good: and God divided the light from the darkness.
5. And God called the light Day, and the darkness he called Night. And the evening and the morning were the first day...
```

(The First Book of Moses. _The Creation of Heaven and Earth, King James Version_. The Bible.)

The segmentation should produce the following tokens:

```bash
tokens = ['In', 'the', 'beginning', 'God', 'created', 'the', 'heaven', 'and', 'the', 'earth',..., 'the', 'first', 'day', '...']
```

Further analysis of the tokens should produce the following conclusion:

```bash
conclusion = ['God is the creator of heaven and earth']
```

**Note**: most programming languages include a _tokenize_ method, allowing to split a string into its tokens; for instance, Javascript provides **String.split** method, which returns an array of the tokenized string as the following code shows:

```bash
const str = 'The quick brown fox jumps over the lazy dog.';

const words = str.split(' ');
console.log(words[3]);
// expected output: "fox"

const chars = str.split('');
console.log(chars[8]);
// expected output: "k"
```

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

### Anchors

### Quantifiers

### Grouping Constructs

### Bracket Expressions

### Character Classes

### The OR Operator

### Flags

### Character Escapes

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
