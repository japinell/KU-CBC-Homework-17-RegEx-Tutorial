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

The main objective of my post is not to portray regular expressions as an easy or complex subject, but rather, to describe one of the most interesting applications of regular expressions in **Computer Science (CS)** and **Natural Language Processing (NLP)**: _word tokenization_, the segmentation of text into words as applied in **Machine Learning (ML)** and **Text Mining**; for example, given the following statement by [Forbes Magazine](https://www.forbes.com/sites/mikepatton/2021/05/03/us-national-debt-expected-to-approach-89-trillion-by-2029/?sh=25edd7e05f13):

```bash
U.S. National Debt Expected To Approach $89 Trillion By 2029
```

Your task is to automate finding relevant pieces of information within the text so that important decisions can be recommended and planned ahead. One approach to this problem is to have a group of humans draw conclusions from the data. In this case, we have to factor that humans are subjective creatures and are prone to biases like political beliefs and level of education. Another approach might be to have a computer automate this task while eliminating the potential for intrinsic biases; enter the world of **Artificial Intelligence**, **Machine Learning**, and **Regular Expressions**. (Full disclosure, the above statement might be easy task for a human, but the point of discussion here is automation.)

**Note**: Most programming languages include a _tokenize_ method, which allows to split a string into its tokens; for instance, Javascript provides the **String.split** method, which returns an array of the tokenized string as the following code shows:

```bash
const str = 'The quick brown fox jumps over the lazy dog.';

const words = str.split(' ');
console.log(words[3]);
// expected output: "fox"

const chars = str.split('');
console.log(chars[8]);
// expected output: "k"
```

For the purposes of this discussion, I will limit the analysis to the application of **RegEx**; the discussion of **AI** and **ML** is left for another post; but, for now, it suffices for you to know that regular expressions are not only used to validate user inputs. Their applications can be quite interesting!

Before solving the problem in question, let's review some basics of **RegEx**. If you are already familiar with the topic, and would like to skip the review, check the solution to the problem [here](#solution).

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

**Anchors** are used to define the begining and the end of a regular expression; amongst other anchors, the "^" and "$" characters are used for this purpose as follows:

```bash
const match = /^a/.test("abc")
console.log(match)              // true, because the string "abc" starts with "a"

const match = /^b/.test("abc")
console.log(match)              // false, because the string "abc" does not start with "b"

const match = /c$/.test("abc");
console.log(match);             // true, because the string "abc" ends with "c"

const match = /a$/.test("abc");
console.log(match);             // false, because the string "abc" does not end with "a"
```

One most pay attention to the position of anchors as they might produce unexpected results; for example, to _validate that an input contains only numbers_, one might write the following code:

```bash
const match = /d+/.test("abcd7efg");
console.log(match);             // true, because the string "abcd7efg" does contain a digit, "7"
```

However, the correct code should be:

```bash
const match = /^[0-9]\w*/g.test("abcd7efg");
console.log(match);             // false, because the string "abcd7efg" contains characters other than digits, the group [0-9]

const match = /^[0-9]\w*/g.test("123");
console.log(match);             // true, because the string "123" contains only digits
```

Other anchors include:

- \G: defines the starting point of a search; useful along with the /g flag
- \A: matches the start of a string
- \Z: matches the end of a string
- \z: similar to \Z, but the main difference is that it will not match before a trailing newline at the end of a string
- \b: used to match a character matched by \w and a character not matched by \w
- \B: used to match the position between characters matched by \w

### Quantifiers

Quantifiers, also known as repetition operators, are used to define the number of matching tokens in the regular expression. The following characters are used for this purpose:

- Question mark ("?"): used to match the preceeding token zero or one time, which makes the token optional
- Astherisk ("\*"): used to match the preceeding token zero or more times
- Plus ("+"): used to match the preceeding token one or more times, which makes the token required

For example, to match an HTML tag with no attributes, one could write the following regular expression:

```bash
const match = /^<[A-Za-z][A-Za-z0-9]*>/g.test("<b>");
console.log(match); // true, because the string "<b>" contains a character group, "[A-Za-z][A-Za-z0-9]", that is repeated zero or more times between the sharp brakets, "<>"

Also,

const match = /^<[A-Za-z][A-Za-z0-9]*>/g.test("<section1>");
console.log(match); // true, because the string "<section1>" contains a character group, "[A-Za-z][A-Za-z0-9]", that is repeated zero or more times between the sharp brakets, "<>"
```

It is possible to specify the number of times a token can be repeated. For this, regular expressions allow to specify a tuple, {min, max}, where _min_ indicates the minimum number of matches, and, as you probably guessed, _max_ indicates the maximum number of matches. Either value, _min_ or _max_, are optional. Therefore, {0, } is equivalent to "\*"; {1, } is equivalent to "+"; and {n} is equivalent to repeating the token _n_ times.

The "+", "\*", and {min, max} operators are often called **greedy operators** because they will try to find a matching character at any cost. On the other hand, the "?" operator is called **lazy operator** because it will cause the preceeding matching character as few times as possible.

### Grouping Constructs

Round brackets ("()") are used in regular expressions to group blocks, that is, to apply an operator to the entire group defined in parentheses. This is different to using square brackets ("[]"), which are used to define character classes; and curly braces ("{}"), which are used by the repetition operators. For example, to match "Hello" and "Hello World", one could write the following regular expression:

```bash
const match = /^Hello (World)?/g.test("Hello ");
console.log(match); // true, because the string "Hello" matches the first pattern, "Hello", even if the second pattern, "World" is not matched

Likewise,

const match = /^Hello (World)?/g.test("Hello World");
console.log(match); // true, because the whole string "Hello World" matches both patterns, "Hello" and "World"
```

Note that in the above example, the repetition operator "?" was used after the pattern "(World)"; this causes the pattern to be optional. If the "?" quantifier is ommitted, or the "+" quantifier replaces the "?" quantifier, the pattern "(World)" becomes required.

### Bracket Expressions

Bracket expressions are a special type of character class used to define a list of valid entries; for instance, to define a list of valid digits, one could write the following regular expression:

```bash
const match = /^[0123456789]/g.test("7");
console.log(match); // true, because the string "7" matches any of the characters in the pattern, "0123...9"
```

The above example can also be achieved with a **range expression** like this:

```bash
const match = /^[0-9]\w\*/g.test("7");
console.log(match); // true
```

It is possible to represent bracket expressions in the user's or application's locale by defining the locale type; for instance, "[[:punct:]]" represents the punctuation characters in the locale and ASCII character encoding, as the following example shows:

```bash
const match = /^[0-9]\w\*/g.test("7");
console.log(match); // true
```

### Character Classes

Character classes, also known as character sets, are used to match only one out of several characters. These matching characters are expressed in square brackets ("[]"). It is possible to use a hyphen inside a character class to specify a range. Also, character classes can be negated using the "^" operator inside the square brackets; for instance, to specify that a string should contain only alpha characters, one could write the following regular expression:

```bash
const match = /^[a-zA-Z]/g.test("ABCabc");
console.log(match); // true, because the string "ABCabc" contains only alpha characters

However,

const match = /^[^0-1a-zA-Z]/g.test("123ABCabc");
console.log(match); // false, because the string "123ABCabc" does contain alpha-numeric characters

In contrast,

const match = /^[^0-1a-zA-Z]/g.test("%$#@&-+=");
console.log(match); // true, because the string "%$#@&-+=" does not contain alpha-numeric characters
```

It is also possible to use short-hand character classes to represent oftenly used character classes as summarized below:

- \d: represent digits; it is equivalent to [0-9]
- \w: represent words; it is equivalent to [A-Za-z]
- \s: represent the whitespace character (" ")

Just like the other commonly used character classes, short-hand character classes can be negated and used along with repetition operators; so, for instance:

```bash
const match = /^[0-9]+/g.test("765");
console.log(match); // true, because the string "765" matches the pattern, [0-9]+.
```

To match a repeating sequence, [Grouping Constructs](#grouping-constructs) can be used; for example, to validate if a string has repeating numbers, one could write the following regular expression:

```bash
const match = /^([0-9])\1+/g.test("765");
console.log(match); // false, because the string "765" is not a repeating sequence
```

### The OR Operator

The OR ("|") operator is used in conjunction with the [Grouping Constructs](#grouping-constructs) to define a logical matching condition that evaluates to true by means of either the left- or the right-hand side of the operator; the operator works just like an OR operation in boolean logic. For example, to define a regular expression to define that one likes small dogs, but not big dogs or cats, one could write the following:

```bash
const match = /^I like small dogs, but not (?:big dogs|cats)/g.test(
  "I like small dogs, but not big dogs"
);
console.log(match); // true, because the string "I like small dogs, but not big dogs" matches the pattern by means of the "|" operator

const match = /^I like small dogs, but not (?:big dogs|cats)/g.test(
  "I like small dogs, but not cats"
);
console.log(match); // true, because the string "I like small dogs, but not cats" matches the pattern by means of the "|" operator

const match = /^I like small dogs, but not (?:big dogs|cats)/g.test(
  "I like big dogs, but not cats"
);
console.log(match); // false, because the string "I like big dogs, but not cats" does not match the pattern, even if the "|" operator does match
```

### Flags

Flag are used to enable advanced searching features like case sensitivity and global searching. They are optional and can be used in separately or together. The following flags are available in regular expressions:

- \d: used to generate indices for substring matches
- \g: used to specify a global search
- \i: used to specify a search that does not regard for case sensitivity
- \m: used to specify a multiline search
- \s: used to specify that the "." operator allows to match newline characters; it is equivalent to (" ")
- \u: used to treat a pattern as a sequence of unicode points
- \y: used to perform a "sticky" search

For example, to create a regular expression that looks for a word followed by another word separated by a space character, and to specify that the search is performed globally throughout the entire string, one could write the following:

```bash
const match = /^\w+\s\w+/g.test("Hello World");
console.log(match); // true, because the string "Hello World" matches the pattern, a word followed by a space, followed by another word

However,

const match = /^\w+\s\w+/g.test("Hello");
console.log(match); // false, because the string "Hello" does not match the pattern, a word followed by a space, followed by another word
```

### Character Escapes

Character escapes are used to treat strings of characters in regular expressions as literals. The most common character escapes include "\r" for carriage return, "\n" for new line, "\v" for vertical tab, "\t" for a tab, "\f" for form feed, and "\xNN" for hexadecimal numbers. It also includes the classes discussed in the [Character Classes](#character-classes) section, namely:

- \d: used to specify a class composed of digit characters
- \D: used to specify a class of characters that is not composed of digits
- \w: used to specify a sequence of characters, or a word
- \W: used to specify a sequence of characters, except a word

Escapes are useful for handling regular expressions in strings that already include the "\" or the "/" characters; for example, when evaluating strings representing files within folders, or URL addresses.

## Solution

In the [Summary](#summary) section above a posted the problem of parsing a statement from Forbes Magazine regarding the status of the U.S. National Debt. The statement included text and numbers, and the problem was to parse the different components of the statement using regular expressions with the purpose of using the parsed data to perform other advanced tasks, say, to analyze the data and facilitate decision-making. It was also clarified that the purpose of this article is not to deep-dive into **AI** and **ML** concepts, but to make the reader aware of more advanced uses of **RegEx** in those applications.

The statement is presented below:

```bash
U.S. National Debt Expected To Approach $89 Trillion By 2029
```

Building on all the topics covered in this article, the regular expression used to solve the problem is provided below:

```bash
(?x)([A-Z]\.)+|\w+(-\w+)*|\$?\d+(\.\d+)?%?|\.\.\.|[][.,;"’?():-_‘]
```

Using regular expressions validators like [RegEx 101](https://regex101.com/r/SxCdMO/1), one can see that the regular expression above matches all the components in the statement, namely, it matches the text representing the name of country, "U.S."; the rest of the topic, "National", "Debt" "Expected", "to", "Approach"; the amount, "$89"; the amount unit, "Million"; and the rest of the text, "By", and "2029".

If one only wanted to know the amount of the U.S. National Debt, it would have sufficed to execute the portion of the regular expression that extracts the amount without regard for where in the text the amount is. On the other hand, if the problem were solved by simply tokenizing the text, the solution would not meet the requirement if the text were moved around, thus breaking the logic of parsing the text as simple tokens. It would definitely need more coding to handle that possibility.

## Author

My name is **Rigo A Pinell**. I specialize in building applications for desktop, web, and mobile devices. I am currently enrolled in the **Flex Stack Coding Boot Camp** at
**Kansas University** where I am learning to develop, test, and deploy web and mobile applications using HTML, CSS, JavaScript, Bootstrap, Tailwind CSS, JQuery, Node.JS, Express.JS, Sequelize, MySQL, JawsDB, Express Handlebars, and other frameworks and libraries including Inquirer, Jest, Dotenv, Express Session, Connect Session
Sequelize, Bcrypt, DayJS, Google Fonts, and Font Awesome.

Would you like to know more about the _cool_ projects I am building? Check them in [Git Hub](https://github.com/japinell). Want to connect, follow, or contact me to talk about my
professional career? Check out my [LinkedIn](https://www.linkedin.com/in/japinell/) Profile.
