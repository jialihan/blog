## Introduce to SASS/SCSS

#### I. [ What is SASS?](#question1)

#### II. [ SASS features](#question2)

#### III. [Install sass](#question3)

#### IV. [Nested Selectors](#question4)

#### V. [Variables](#question5)
- [define a simple variable](#q5-1)
- [Lists](#q5-2)
- [Maps](#q5-3)

#### VI. [built-in functions](#question6)

#### VI.I [Import and Partials](#question7)

#### VIII. [improve media queries](#question8)

#### IX. [mixins](#question9)

#### X. [Use Ampersand Operator ](#question10)

#### XI. [Pros & Cons](#question11)





<div id="question1" />

### I. what is SASS?
- it doesn't use new rules
- it extends the CSS during development only
- compile it to CSS before production

<div id="question2" />

### II. SASS features
Some important:
- nested css rules
- inherit css rules
- helper functions: eg: to make color brighter
- mixins: create re-usable code 
- partials
- variables: eg: some color variable can be re-used

<div id="question3" />

### III. install
DOCS: https://sass-lang.com/
install:
```bash
npm install -g sass
```
Syntax: `.sass`
- no semi-column: `;`
- no curly braces: `{, }`
- use **indent / indentation**

use `.scss` can keep the original CSS syntax:

<div id="question4" />

### IV. Nested Selectors
original CSS:
```css
.mylink li {
	...
}
.mylink .linkA {
	...
}
```

in `.scss` file with nested rules:
```css
.mylink {
	padding: 0;
	li {
		...
	}
	.linkA {
		...
	}
}
```

Nested properties: 
for example: `flex-direction flex-wrap` becomes:
```css
flex:
{
	direction: column;
	wrap: nowrap;
}
```

<div id="question5" />

### V. variables

<div id="q5-1" />

#### 5.1 define  a  simple variable
Syntax:  dollar sign: `$`
For example:
```css
/* define a variable */
$main-color: #0D0D0D;
/* usage */
.header{
	color: $main-color;
	border: 1px solid $main-color;
}
```

<div id="q5-2" />

#### 5.2 Lists variables
Use case: when we want to use a collection of values, for example: `1px solid $main-color` on "border" property.

```css
$border-default: 1px solid $main-color;
/* usage */
.header {
	border: $$border-default;
}
```

<div id="q5-3" />

#### 5.3 Maps variable
When you want to group your vars and more structure of your variables, for example, create a map of colors to use:
```css
$colors:  (main: #0D0D0D, secondary: #521751);
```
How to get values in map variable ?
[`map-get(_mapName, _keyName)`](https://sass-lang.com/documentation/modules/map) function

For example:
```css
color: map-get($color-map, main);
border-color: map-get($color-map, secondary);
```


<div id="question6" />

### VI. built-in functions
**Docs:** https://www.rubydoc.info/gems/sass/Sass/Script/Functions

For example:
color functions: 
```css
color: lighten($color-main);
```

<div id="question7" />

### VII. better import and partials

import different `.scss` files:
```js
import "_variables.scss";
import "typography.scss";
```
After compiling into `.css` file, no longer have imports separate other `.css` files, we get one CSS file as the whole.

So **no extra HTTP request to get the multiple css files.**

<div id="question8" />

### VII. improve media queries
put media query just near you current css class code, more readable for developers, for example:
```css
.header {
	.padding: 20px;
	@media (min-width: 768px)
	{
	 .padding: 0;
	}
}
```

<div id="question8" />

### VIII.  inheritance

Docs: [@extend](https://sass-lang.com/documentation/at-rules/extend)
group some sharable same css properties, create a new class and put all those shared features into it:
For example:
```css
.content-section{
	box-sizing: border-box;
	background: white;
	@media (min-width: 768px)
	{
	 .padding: 0;
	}
}
```

usage: extend this shared class on sub detailed classes
```css
.content-header
{
	@extend .content-section;
	...
}
.content-body
{
	@extend .content-section;
	...
}
.content-footer
{
	@extend .content-section;
	...
}
```

<div id="question9" />

### IX.  mixins

Docs: [@mixin and @include](https://sass-lang.com/documentation/at-rules/mixin)

**Create a mixin:** 
define any properties you want
```css
@mixin reset-list() {
  margin: 0;
  padding: 0;
  list-style: none;
}
```
**Usage:**
```css
.container {
	@include reset-list();
	...
}
```

**Mixin width parameters:**
```css
@mixin media-min-width($w) {
	@media (min-width: $w)
	{
		font-size: 125%;
	}
}
``` 

<div id="question10" />

### X.  Use Ampersand Operator 

When some pseudo selector like `:hover, :active`, we can use `&` sign to write code inside the css class.
**Old nested:**
```css
.container {
	...
	.item:hover,
	.item:active {
		...
	}
}
```

**New code:**
```css
.item {
	&:hover,
	&:active {
		...
	}
}
```

<div id="question11" />

### XI. Pros and Cons

Pros: see all above features:
-   Sass variables
-   maps
-   nesting
-   functions
-   mixins
-   extending
-   and operations.

Cons:
- 1. Tooling and developer convenience: need to install sass, and compile at local when development
- 2. Compilation slows down development
- 3. They can produce very large CSS files
	Because: hiearchy will be flattened in the resulting CSS. And the tree of selectors will be duplicated for every selector.
- 4. VARIETY OF SYNTAX
	Each CSS Processor’s syntax is different. The features largely overlap, but they’re implemented differently. eg: SASS, PostCSS..
	