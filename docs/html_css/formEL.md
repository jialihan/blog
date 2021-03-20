## HTML - Form Elements

#### 1. [radio groups](#question1)

#### 2. [](#question2)

#### 3. [](#question3)

#### 4. [](#question4)
- [Adjacent Sibling: `'+'`](#q4-1)
- [General Sibling: `'~'`](#q4-2)
- [Child Combinator: `'>'`](#q4-3)
- [Descendant Combinator : `' '(space)`](#q4-4)


<div id="question1" />

### 1. radio groups

Docs: [input/radio](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio)

Syntax:
```html
<input type="radio" id="tea" name="drink" value="tea" checked>
<label for="tea">Tea</label>
<input type="radio" id="coffeee" name="drink" value="coffee">
<label for="coffee">Coffee</label>
```

**Input Attributes:**
- `type`: "radio"
- each also have a unique [`id`](https://developer.mozilla.org/en-US/docs/Web/API/Element/id "id")
- Only same `name` attribute will be a group, here is `name="drink"`
- `checked`: [docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio#checked) - default selected in the same group
- `value`: [docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio#value) -  the **value of the radio when submitting the form**

**Label Attribute:**
- `for` - value is the `id of the input element` bounded to.
- two ways to use the Label
	- wrap the input element inside
		```html
		<label>username:
			<input name="name" id="name" placeholder="Username" /></label>
		```
	- be a sibling of the input element using `for` label.
		```html
		<label for="name">username:<label>
		<input name="name" id="name" placeholder="Username" />
		```
**Source code:** [codepen link](https://codepen.io/jellyhan27/pen/zYoXwRa)

<div id="question3" />

### 3. Inheritance Styles from parent 

### 4. Combinator Selectors
