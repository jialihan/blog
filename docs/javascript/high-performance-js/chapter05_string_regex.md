## Chapter 5 - String and Regular Expressions

### 1. ## String Concatenation

#### 1.1 Plus(+) and Plus-Equals(+=) Operators

Slower code:

```
str += "one" + "two";
```

- A temporary string is created in memory. (slower)
- The concatenated value `"onetwo"` is assigned to the temporary string. (slower)
- The temporary string is concatenated with the current value of `str`.
- The result is assigned to `str`.

To improve step 1 & step 2 for the temporary strings:

```
str += "one";
str += "two";
// OR
strt = str + "one" + "two";
```

#### 1.2 Array Joining

```
const strs=[];
const str = strs.join('');
```

#### 1.3 String.prototype.concat

Unfortunately, `concat` is a little slower than simple `+` and `+=` operators in most cases, and can be substantially slower in IE, Opera, and Chrome.

### 2. Regular Expression Optimization

#### 2.1 trim()

Remove leading and trailing spaces:

```
// trim 1
this.replace(/^\s+/, "").replace(/\s+$/, "");
```

more common alternatives you might encounter:

```
// trim 2
String.prototype.trim = function() {
    return this.replace(/^\s+|\s+$/g, "");
}
```

### 3 Summary

- When concatenating numerous or large strings, array joining is the only method with reasonable performance in IE7 and earlier.
- If you don’t need to worry about IE7 and earlier, array joining is one of the slowest ways to concatenate strings. Use simple `+` and `+=` operators instead, and avoid unnecessary intermediate strings.
- **Backtracking** is both a fundamental component of regex matching and a frequent source of regex inefficiency.
- Runaway backtracking can cause a regex that usually finds matches quickly to run slowly or even crash your browser when applied to partially matching strings. Techniques for avoiding this problem include making adjacent tokens mutually exclusive, avoiding nested quantifiers that allow matching the same part of a string more than one way, and eliminating needless backtracking by repurposing the atomic nature of lookahead.
- A variety of techniques exist for improving regex efficiency by helping regexes find matches faster and spend less time considering nonmatching positions (see [More Ways to Improve Regular Expression Efficiency](https://learning.oreilly.com/library/view/high-performance-javascript/9781449382308/ch05.html#more_ways_to_improve_regular_expression_)).
- Regexes are not always the best tool for the job, especially when you are merely searching for literal strings.
- Although there are many ways to **trim** a string, using two simple regexes (**one to remove leading whitespace and another for trailing whitespace**) offers a good mix of brevity and cross-browser efficiency with varying string contents and lengths. Looping from the end of the string in search of the first nonwhitespace characters, or combining this technique with regexes in a hybrid approach, offers a good alternative that is less affected by overall string length.
