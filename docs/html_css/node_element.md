## DOM Node & Element
#### I. [Node](#question1)

#### II. [Element](#question2)

#### III. [](#question3)

#### IV. [](#question4)

<div id="question1" />

### I. Node
**Docs:** [Node](https://developer.mozilla.org/en-US/docs/Web/API/Node)
#### 1.1 childNodes
#### 1.2 [nodeType](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeType)

| Name  | Value |
|--|--|
| `Node.ELEMENT_NODE` |  `1` |
| `Node.ATTRIBUTE_NODE` | `2` | 
| `Node.TEXT_NODE` | `3` |
| ... | ... | 

<div id="question2" />

### II. Element

#### 2.1 [tagName](https://developer.mozilla.org/en-US/docs/Web/API/Element/tagName)

Note: it's **upper case** string !!!

```js
element.tagName; // "DIV"
element.tagName.toLowerCase(); // 'div'
```

#### 2.2 attributs
- `element.attributes`:  [link](https://developer.mozilla.org/en-US/docs/Web/API/Element/attributes)
	It is a [`NamedNodeMap`](https://developer.mozilla.org/en-US/docs/Web/API/NamedNodeMap), not an `Array`.
	```js
	// cannot loop
	attr.name; // attr
	attr.value; // value
	```
- `element.getAttribute('attr_name')`: [link](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute)
 If the given attribute does not exist, the value returned will either be `null` or `""` (the empty string); see [Non-existing attributes](https://developer.mozilla.org/en-US/docs/Web/API/Element/getAttribute#non-existing_attributes) for details.
-  `element.setAttribute(name,value)`: [link](https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttribute)
	It always return **LowerCase**, or could directly use `element[attr] = value;`.

<div id="question3" />

### III. 


<div id="question4" />

### IV.
