## White Spaces and Line Breaks in JavaScript

#### 1. [Line Breaks](#question1)

#### 2. [Function.prototype.apply](#question2)

#### 3. [BFE #95. implement String.prototype.trim()](#question3)

<div id="question1" />

### I. Line Breaks

- The Carriage Return (CR) character (`0x0D`, `\r`)
  moves the cursor to the beginning of the line without advancing to the next line. This character is used as a new line character in Commodore and Early Macintosh operating systems (OS-9 and earlier).

- The Line Feed (LF) character (`0x0A`, `\n`)
  moves the cursor down to the next line without returning to the beginning of the line. This character is used as a new line character in UNIX based systems (Linux, Mac OSX, etc)

- The End of Line (EOL) sequence (`0x0D 0x0A`, `\r\n`)
  is actually two ASCII characters, a combination of the CR and LF characters. It moves the cursor both down to the next line and to the beginning of that line. This character is used as a new line character in most other non-Unix operating systems including Microsoft Windows, Symbian OS and others.

**References:**

- https://stackoverflow.com/questions/1552749/difference-between-cr-lf-lf-and-cr-line-break-types
- https://www.ni.com/en-us/support/documentation/supplemental/21/labview-termination-characters.html

<div id="question2" />

### II. White Spaces

- `' '`
- `" "`
- `\u3000`: in unicode Full-width white space
  Reference:
  - https://www.compart.com/en/unicode/U+3000
  - https://www.fileformat.info/info/unicode/char/3000/index.htm
- `\t`: tab

<div id="question3" />

### III. BFE #95. implement String.prototype.trim()

**Docs:**
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Trim

> Whitespace in this context is all the whitespace characters (space, tab, no-break space, etc.) and all the line terminator characters (LF, CR, etc.).

**Solution:**

```js
function trim(str) {
  if (!str) {
    return str;
  }
  const whitespaces = ["s", "\t", "\n", "\r", " ", " ", "\u3000"];
  var i = 0,
    j = str.length - 1;
  while (whitespaces.includes(str[i])) {
    i++;
  }
  while (whitespaces.includes(str[j])) {
    j--;
  }
  if (j < i) {
    return "";
  }
  return str.slice(i, j + 1);
}
```
