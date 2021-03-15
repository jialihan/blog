## Regular Expression in JavaScript

#### I. [What is a regular expression?](#chapter1)

#### II. [flags](#chapter2)

#### III. [patterns - brackets](#chapter3)

#### IV. [Quantifiers](#chapter4)

#### V. [Special Character with `\`](#chapter5)

#### VI. [Grouping](#chapter6)

#### VII. [RegExp Methods in JS](#chapter7)

#### VIII.  [Examples](#chapter8)
- [Phone number Regex example](#ch8-1)
- [URL parse QueryParams example](#ch8-2)

<div id="chapter1" />

### I. What is a regular expression?

A regular expression is a sequence of characters that forms a search pattern.

In Javascript:
In JavaScript, regular expressions are also objects. These patterns are used with the [`exec()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) and [`test()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) methods of [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp), and with the [`match()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match), [`matchAll()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll), [`replace()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace), [`replaceAll()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll), [`search()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search), and [`split()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) methods of [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String). This chapter describes JavaScript regular expressions.

**Docs:** [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)


**Constructor:**
```js
let re = /ab+c/;
let re = new RegExp('ab+c');
```

<div id="chapter2" />

### II. flags

Docs:[Advanced searching with flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags)

#### 2.1 constructor
- literal 
- RegExp Object constructor
```js
let re = /ab+c/i; // literal notation
let re = new RegExp('ab+c', 'i') // constructor with string pattern as first argument
let re = new RegExp(/ab+c/, 'i') // constructor with regular expression literal as first argument (Starting with ECMAScript 6)
```

#### 2.2 Flags in regex
There are **six optional flags** that allow for functionality like global and case insensitive searching, and they can alter the behavior of a regular expression.

| Flag | Description |
|--|--|
| g | global search. find all matches instead of stopping after the first match  |
| i | ignore case, `/[a-z]/` equals to `/[a-zA-Z]/`|
| m | multiline search. `^` and `$`  match the start and end of each line respectively, treating `\r`, `\n` as delimiters. |
| u | unicode. treat a pattern as a sequence of unicode code points. |
| s | Allows `.` to match newline characters.|
| y | Perform a "sticky" search that matches starting at the current position in the target string. See [`sticky`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/sticky). |

<div id="chapter3" />

### III. patterns - brackets

Brackets(`[]`) are used to find a range of characters.

| Pattern | Description |
|--|--|
| **[...]** | Any one character between the brackets. |
| **[^...]** | Any one character **not** between the brackets. |
| **[0-9]** |  It matches any decimal digit from 0 through 9. |
| **[a-z]** | It matches any character from lowercase  **a** through lowercase  **z**. |
|**[A-Z]** | It matches any character from uppercase  **A**  through uppercase  **Z**.|
| **[a-Z]** | It matches any character from lowercase  **a**  through uppercase  **Z**. |

<div id="chapter4" />

### IV. Quantifiers
Quantifiers define the number of occurrences of a string.

| Quantifier | Description |
|--|--|
| **p+** | + indicates one or more occurrence of the char p. |
| **p*** | + indicates zero or more occurrence of the char p. |
| **p?** | + indicates zero or one occurrence of the char p. |
| **p{N}** | It matches any string containing a sequence of  **N**  p's |
| **p{2,3}** | It matches any string containing a sequence of two or three p's. |
| **p{2, }** | It matches any string containing a sequence of at least two p's. |
| **p$** | It matches any string with p at the end of it. |
| **^p** | It matches any string with p at the beginning of it. | 

<div id="chapter5" />

### V. special characters with `\`

**Docs:** [character classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes)

|characters| description  |
|--|--|
| **.** | a single character |
| **\s** | a whitespace character (space, tab, newline) |
| **\S**  |  **non**-whitespace character, eg: `/\S\w*/` matches "foo" in "foo bar". |
| **\d** | a digit (0-9) |
| **\D** | **NOT** a digit |
| **\w** | a word character (a-z, A-Z, 0-9, _) |
| **\W** | **NOT** a word character |
| **\t** | Matches a horizontal tab. |
| **\n** | Matches a linefeed. |
| **[\b]** | a literal **backspace** (special case). If you're looking for the word-boundary character (`\b`), see  [Boundaries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Assertions). |
| **[aeiou]** | matches a single character in the given set |
| **[^aeiou]** | matches a single character outside the given set |
|`(foo|bar|baz)` | matches any of the alternatives specified |

<div id="chapter6" />

### VI. Grouping

#### 6.1 capturing group `(x)`

**Capturing group:** Matches  `_x_`  and remembers the match.
For example,  
```js
var reg = /(foo)/g;
var string = "foo bar";
var match;
while(match = reg.exec(string))
{
	console.log(match[1]); // "foo"
}
``` 
**Capturing groups have a performance penalty.** If you don't need the matched substring to be recalled, prefer non-capturing parentheses (see below).

[String.match()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match)` won't return groups if the `/.../g` flag is set. However, you can still use `[String.matchAll()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)` to get all matches.


#### 6.2 Named capturing group `(?<Name>x)`
Matches "x" and stores it on the groups property of the returned matches under the name specified by  `<Name>`. The angle brackets (`<`  and  `>`) are required for group name.

For example:
```js
let reg =  /name: (?<name>\w+), age: (?<age>\d+)/g;
var string = "name: jelly, age: 18";
var match;
while(match = reg.exec(string))
{
	console.log(match.groups.name, match.groups.age); // "jelly", "18"
}
```

#### 6.3 Non-capturing group `(?:x)`

**Non-capturing group:** Matches "x" but does not remember the match. The matched substring cannot be recalled from the resulting array's elements (`[1], ..., [n]`) or from the predefined `RegExp` object's properties (`$1, ..., $9`).

<div id="chapter7" />

### VII. RegExp Methods in JS

**Docs:** [regular expression in Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#using_regular_expressions_in_javascript)


| Method | Description |
|--|--|
| [`exec()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) | Executes a search for a match in a string. It returns an array of information or  `null`  on a mismatch. |
| [`test()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) | Tests for a match in a string. It returns  `true`  or  `false`. |
| [`match()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match) | Returns an array containing all of the matches, including capturing groups, or  `null`  if no match is found. |
| [`matchAll()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll) | Returns an iterator containing all of the matches, including capturing groups.  |
| [`search()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search) | Tests for a match in a string. It returns the index of the match, or  `-1`  if the search fails. |
| [`replace()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace) | Executes a search for a match in a string, and replaces the matched substring with a replacement substring. |
| [`replaceAll()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll) | Executes a search for all matches in a string, and replaces the matched substrings with a replacement substring. |
| [`split()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) | Uses a regular expression or a fixed string to break a string into an array of substrings. |

For example:
```js
var myRe = /d(b+)d/g;
var myArray = myRe.exec('cdbbdbsbz');
```

<div id="chapter8" />

### VIII. Examples

<div id="ch8-1" />

#### 8.1 Phone number Regex example
The following regex will match  all :
- `"(xxx)xxx-xxxx"`
- `"xxx-xxx-xxxx"`
- `"xxxxxxxxxx"`

```js
var regex = /^\(?\d{3}\)?[-. ]?\d{3}[-. ]?\d{4}$/;
```
1 ) `\(?` and `\)?` means including the parentheses or not
2 )  `[-. ]?`: means zero or one times of  `-` or `.` or ` (space)` character.
3 ) `\d{3}` or `\d{4}` means the numbers section of 3 or 4 digits from `[0-9]`.

**Test & results:**
```js
var s = '1234567890';  // true
s = '(123)456-7890';  // true
s = '123-456-7890';   // true
console.log(regex.test(s));
```

<div id="ch8-2" />

#### 8.2 URL parse QueryParams example

```js
var  regex = /[?&]([^=#]+)=([^&#]*)/g;
```
1 ) no `#` sign in key and value
2 ) using grouping `()` to get our parts of key and value

3 ) `g` flag or modifier means search multiple matched string in whole string
4 ) `[^...]` mean any char except those inside of the `[^...]`

**Test & results:**
```js
var  url = "www.domain.com/?age=29&na#me=jelly#hpos";
var  params = {};
var  match;
while (match = regex.exec(url)) {
	params[match[1]] = match[2];
}
console.log(params); // {age: 29}
```








