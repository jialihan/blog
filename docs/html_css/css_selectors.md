## CSS Selectors

#### 1. [Basic Types of Selectors](#question1)

#### 2. [Specificity](#question2)

#### 3. [Inheritance Styles from parent](#question3)

#### 4. [Combinator Selectors](#question4)
- [Adjacent Sibling: `'+'`](#q4-1)
- [General Sibling: `'~'`](#q4-2)
- [Child Combinator: `'>'`](#q4-3)
- [Descendant Combinator : `' '(space)`](#q4-4)

#### 5. [More on ID & Classes selector](#question5)
- [Multiple classes on same element](#q5-1)
- [Combinations of Classes and IDs](#q5-2)
- [Compare ID & classes](#q5-3)

#### 6. [More on Pseudo Classes](#question6)
- [User action pseudo-classes](#q6-3)
- [Tree-structural pseudo-classes](#q6-3)
- [ `:not` selector](#q6-3)

#### 7. [`!important`  notation](#question7)

<div id="question1" />

### 1. Basic Types of Selectors
Docs: [css_selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)
- Elements selector
- Classes selector
- Universal selector: `*`
- ID selector
- Attribute selector

| selectors | html | css|
|--|--|--|
| Elements | `<h1>My Header</h1>`   |  h1 { <br /> &nbsp;&nbsp;&nbsp;&nbsp; color: red; <br /> }|
| Classes | `<h1 class="header">`<br />My Header `</h1>`   |  .header { <br /> &nbsp;&nbsp;&nbsp;&nbsp; color: red; <br /> }|
| Universal | whole HTML | * { <br /> &nbsp;&nbsp;&nbsp;&nbsp; margin: 0; <br /> }|
| ID | `<h1 id="myheader">` <br /> My Header `</h1>`  |  #myheader { <br /> &nbsp;&nbsp;&nbsp;&nbsp; color: red; <br /> }|
| Attribute | `<button disabled>` <br /> &nbsp;&nbsp;&nbsp;&nbsp; Click Me <br /> `</button>`  |  [disabled] { <br /> &nbsp;&nbsp;&nbsp;&nbsp; color: red; <br /> }|

<div id="question2" />

### 2. Specificity

**What is Specificity?**

**Specificity** is the means by which **browsers decide which CSS property values are the most relevant to an element** and, therefore, **will be applied**. Specificity is based on the matching rules which are composed of different sorts of [CSS selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference#selectors).

**Docs:** [Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)

**Online Specificity Calculator**: [test your css-selector here](https://polypane.app/css-specificity-calculator/#selector=)

**Wins order:**
- inline-style: **~~not recommended~~**
	- hard to read & maintain in HTML
	- lots of styles declarations in HTML, hard to work with
- ID
- classes + pseudo classes
- element

See this article here: [css-specificity](https://jialihan.github.io/blog/#/html_css/specificity)

<div id="question3" />

### 3. Inheritance Styles from parent 

- inheritance has a **very low specificity**, always comes at the bottom even **below the browser defaults**.
- use `inherit` value in other selector
	For example:
	```css
	/* inherit font from body */
	.section-title {
		font-family: inherit; 
	}
	```

<div id="question4" />

### 4. Combinator Selectors

Docs: [Combinators - MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Combinators)

<div id="q4-1" />

#### 4.1 Adjacent Sibling: `'+'`
1 ) elements share the same parent
2 ) second element comes **immediately after** the first element

CSS:
```css
h2 + p {
	color: red;
}
```
HTML selected:
```html
<div>
	<h2> not selected. </h2>
	<p> This is selected.</p>  /* selected */
	<p> not selected. </p>
	<h2> not selected. </h2>
	<p> This is selected. </p>  /* selected */
</div>
```

<div id="q4-2" />

#### 4.2 General Sibling: `'~'`
1 ) elements share the same parent
2 ) second element **not need to immediately after the first one**
3 ) second element comes after the first element

CSS:
```css
h2 ~ p {
	color: red;
}
```
HTML selected:
```html
<div>
	<h2> not selected. </h2>
	<p> This is selected.</p>  /* selected */
	<p> This is selected.</p>  /* selected */
	<h3> not selected. </h2>
	<p> not selected. </p>
</div>
```

<div id="q4-3" />

#### 4.3 Child Combinator: `'>'`
1 ) **Any** direct child of the first element can share the same style
2) second element is **a direct child** of the first element
CSS:
```css
div > p {
	color: red;
}
```
HTML selected:
```html
<div>
	<h2> not selected. </h2>
	<p> This is selected.</p>  /* selected */
	<div>
		<p> not selected. </p>
	<div>
	<p> This is selected.</p>  /* selected */
</div>
```

<div id="q4-4" />

#### 4.4 Descendant Combinator : `' '(space)`
1 )  **Any direct or non-direct** child of the first element can be selected
2 ) second element is **a descendant** of the first element
3 ) this one is **most popular** used !

CSS:
```css
div p {
	color: red;
}
```
HTML selected:
```html
<div>
	<h2> not selected. </h2>
	<p> This is selected.</p>  /* selected */
	<div>
		<p> This is selected.</p>  /* selected */
	<div>
	<p> This is selected.</p>  /* selected */
</div>
```

<div id="question5" />

### 5. Classes selector

<div id="q5-1" />

#### 5.1 multiple classes on same element
- use multiple classes on one element
	```html
	<div class="class1 class2" />
	```
- they have the **same specificity**, then styles finally decided by the **order written in the source code**. Later code will **override** the prev code.
	```css
	.class1{
	}
	.class2{
	}
	```
	
<div id="q5-2" />

#### 5.2  Combinations of Classes and IDs

**Reference Docs:**  [multiple-classes-ids-selector](https://css-tricks.com/multiple-class-id-selectors/)

The big point here is that you can **target elements that have combinations of classes and IDs** by stringing those selectors together without spaces.

- **ID and Class Selector**
	As we covered above, you can target elements by a combination of ID and class.
	```html
	/* HTML */
	<h1 id="one" class="two">This Should Be Red</h1>
	```
	```css
	/* CSS */
	#one.two { 
		color: red; 
	}
	```

-  **Double Class Selector**
	Target an element that has all of multiple classes. Shown below with two classes, but not limited to two.
	```html
	/* HTML */
	<h1 class="three four">Double Class</h1>
	```
	```css
	/* CSS */
	.three.four { color: red; }
	```	
- **Multiples**
	We aren’t limited to only two here, we can combine as many classes and IDs into a single selector as we want.
	```css
	/* CSS */
	.snippet#header.code.red { color: red; }
	```	
	Although bear in mind that’s getting a little ridiculous.
	
<div id="q5-3" />

#### 5.3 Compare ID & classes

| class selector | ID selector |
|--|--|
| `.class1` {...} | `#id1` {...} |
| `<div class="class1" >`| `<div id="id1" >` |
| select multiple elements| only select one element |
| re-usable |  only used once per page|
| most used selector| | 

<div id="question6" />

### 6. More on Pseudo Classes

**Docs:** [Pseudo-classes-MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)

A [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)  **pseudo-class** is a keyword added to a selector that specifies a special state of the selected element(s).

<div id="q6-1" />

#### 6.1 [User action pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes#user_action_pseudo-classes)

- [`:hover`](https://developer.mozilla.org/en-US/docs/Web/CSS/:hover)
- [`:active`](https://developer.mozilla.org/en-US/docs/Web/CSS/:active)
	Matches when an item is being activated by the user, for example clicked on.
- [`:focus`](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus)

<div id="q6-2" />

#### 6.2 [Tree-structural pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes#tree-structural_pseudo-classes "Permalink to Tree-structural pseudo-classes")
List some common used here:
- [`:nth-child`](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-child)
	Uses A_n_+B notation to select elements from a list of sibling elements.
	```css
	/* Selects the second <li> element in a list */
	li:nth-child(2) {
	  color: lime;
	}

	/* Selects every fourth element
	   among any group of siblings */
	:nth-child(4n) {
	  color: lime;
	}
	```
- [`:nth-last-child`](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-last-child)
- [`:first-child`](https://developer.mozilla.org/en-US/docs/Web/CSS/:first-child)
- [`:last-child`](https://developer.mozilla.org/en-US/docs/Web/CSS/:last-child)
- [`:nth-of-type`](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-of-type)
	Uses A_n_+B notation to select elements from a list of sibling elements that match a certain type from a list of sibling elements.
	For example:
	```css
	/* Odd paragraphs */
	p:nth-of-type(2n+1) {
	  color: red;
	}

	/* Even paragraphs */
	p:nth-of-type(2n) {
	  color: blue;
	}

	/* First paragraph */
	p:nth-of-type(1) {
	  font-weight: bold;
	}
	```
- [`:nth-last-of-type`](https://developer.mozilla.org/en-US/docs/Web/CSS/:nth-last-of-type)
- [`:first-of-type`](https://developer.mozilla.org/en-US/docs/Web/CSS/:first-of-type)
- [`:last-of-type`](https://developer.mozilla.org/en-US/docs/Web/CSS/:last-of-type)

<div id="q6-3" />

#### 6.3 `:not` selector

The **`:not()`**  [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)  [pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) represents elements that do not match a list of selectors

**Cons:**
- it's hard to use to check positive rules
- use with caution

**For example:**
```css
/* any <a> tag that has no active class */
a:not(.active) {
  color: green;
}
```

<div id="question7" />

### 7. `!important`  notation

Resources:  [When Using !important is The Right Choice](https://css-tricks.com/when-using-important-is-the-right-choice/)

- It overrides all other specificity
- In general: Don't use `!imporatant`
	- it makes your code hard to understand
	- use css rules according to your needs
	- write better CSS code 



 




