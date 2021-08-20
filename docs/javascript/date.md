## Date Object in Javascript

#### 1. [create a Date object](#question1)

#### 2. [To get Date, Month and Year or Time ](#question2)

#### 3. [Set Date Object](#question3)

#### 4. [format Date and Time](#question4)

#### 5. [How JSON stringify the date object](#question5)

#### 6. [Date.now()](#question6)

#### 7. [Different to get current date](#question7)

<div id="question1" />

### I. create a Date object

**Docs:** [Several ways to create a Date object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#several_ways_to_create_a_date_object "Permalink to Several ways to create a Date object")

#### 1. default constructor

```js
var d = new Date();
console.log(d.toString()); // current date & time in your local time zone
// "Thu Aug 19 2021 21:44:20 GMT-0700 (Pacific Daylight Time)"
```

#### 2. pass milliseconds as parameter

```js
d = new Date(1621234770513);
console.log(d, d.toString());
// Sun May 16 2021 23:59:30 GMT-0700 (Pacific Daylight Time)
```

#### 3. pass a Date-Time string

NOT recommended, since different browser running JS interpreting things differently.

- use "T" to separate time and date
- use "+" to represent the time zone ahead of the UTC/GMT time 10 hours.

```js
let D = new Date("1995-12-17T03:24:00+10:00");
```

#### 4. pass individual time parts

Time zone will be the local time zone in your device.

```js
// the month is 0-indexed
let d = new Date(1995, 11, 17, 3, 20, 27, 0);
// (yy, mm, dd, h, m, s, ms)
// Sun Dec 17 1995 03:20:27 GMT-0800 (Pacific Standard Time)
```

If NOT pass all the params, the default will be "0", means the first day and all "0" in your time.

```js
d = new Date(1995, 11);
console.log(d.toString());
// Fri Dec 01 1995 00:00:00 GMT-0800 (Pacific Standard Time)
```

<div id="question2" />

### II. [To get Date, Month and Year or Time](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date#to_get_date_month_and_year_or_time "Permalink to  To get Date, Month and Year or Time")

**Code Example:**

```js
const date = new Date();
console.log(date.getMonth()); // 7, August
console.log(date.getDate()); //
console.log(date.getFullYear());
console.log(date.getHours()); // 22
console.log(date.getMinutes());
console.log(date.getSeconds());
```

Also you can get UTC times:

```js
console.log(date.getUTCHours()); // 5
```

<div id="question3" />

### III. Set Date Object

```js
d.setMinutes(14);
d.setDate(5);
d.setUTCHours(4);
console.log(d.toString());
```

<div id="question4" />

### IV. format Date and Time

#### 1. [toISOString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString)

The **`toISOString()`** method returns a string in _simplified_ extended ISO format ([ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)), which is always 24 or 27 characters long (`YYYY-MM-DDTHH:mm:ss.sssZ` or `±YYYYYY-MM-DDTHH:mm:ss.sssZ`, respectively). The timezone is always zero UTC offset, as denoted by the suffix "`Z`".

```js
d = new Date();
console.log(d.toISOString());
```

**Output:**

```text
2021-08-20T05:06:14.001Z
```

#### 2. [toLocaleString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString)

**Code Example:**

```js
d = new Date();
console.log(d.toLocaleString("en-US"));
console.log(d.toLocaleString("en-AU"));
console.log(d.toLocaleString("zh"));
```

**Output:**

```text
8/19/2021, 10:13:27 PM
19/08/2021, 10:13:27 pm
2021/8/19下午10:13:27
```

Use 2nd param in `toLocaleString()` as "options",
reference doc: [Using options](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString#using_options "Permalink to Using options").

```js
console.log(
  d.toLocaleString("en-US", {
    timeZone: "America/Los_Angeles"
  })
);
```

<div id="question5" />

### V. How JSON stringify the date object?

It use `toISOString()`:

```js
console.log(
  JSON.stringify({
    myDate: d
  })
);
```

**Output:**

```text
{"myDate":"2021-08-20T05:22:33.120Z"}
```

<div id="question6" />

### VI. Date.now()

Docs:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/now

The static **`Date.now()`** method **returns the number of milliseconds** elapsed since January 1, 1970 00:00:00 UTC.

```js
console.log(Date.now());
// 1629437096040 : number
```

<div id="question7" />

### VII. Different to get current date

Reference: [stackoveflow-link](https://stackoverflow.com/questions/33184096/date-new-dated-valueof-vs-date-now/33184171)

- `new Date()` - creates a Date object representing current date/time
- `new Date().valueOf()` - returns number of milliseconds since midnight 01 January, 1970 UTC
- `new Date().getTime()` - Functionally equivalent to new Date().valueOf()
- `Date.now()` - returns the number of milliseconds since midnight 01 January, 1970 UTC
