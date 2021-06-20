## HTML - Form Elements

#### 1. [radio groups](#question1)

#### 2. [Upload File button](#question2)

#### 3. [Input Element](#question3)

#### 4. [](#question4)

<div id="question1" />

### 1. radio groups

Docs: [input/radio](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio)

Syntax:

```html
<input type="radio" id="tea" name="drink" value="tea" checked />
<label for="tea">Tea</label>
<input type="radio" id="coffeee" name="drink" value="coffee" />
<label for="coffee">Coffee</label>
```

**Input Attributes:**

- `type`: "radio"
- each also have a unique [`id`](https://developer.mozilla.org/en-US/docs/Web/API/Element/id "id")
- Only same `name` attribute will be a group, here is `name="drink"`
- `checked`: [docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio#checked) - default selected in the same group
- `value`: [docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/radio#value) - the **value of the radio when submitting the form**

**Label Attribute:**

- `for` - value is the `id of the input element` bounded to.
- two ways to use the Label - wrap the input element inside
  `html <label>username: <input name="name" id="name" placeholder="Username" /></label> ` - be a sibling of the input element using `for` label.
  `html <label for="name">username:<label> <input name="name" id="name" placeholder="Username" /> `
  **Source code:** [codepen link](https://codepen.io/jellyhan27/pen/zYoXwRa)

<div id="question2" />

### 2. Upload File button

**HTML Code:**

```html
<label for="myfile">Select a file:</label>
<input type="file" id="myfile" name="myfile" />
<label for="myfiles">Select your files:</label>
<input type="file" id="myfiles" name="myfiles" multiple />
```

**How to style the input button?**

use `::file-selector-button` selector: [docs](https://developer.mozilla.org/en-US/docs/Web/CSS/::file-selector-button)

```css
input[type="file"]::file-selector-button {
}
input[type="file"]::file-selector-button:hover {
}
input[type="file"]::-webkit-file-selector-button {
}
input[type="file"]::-webkit-file-upload-button:hover {
}
```

![image](../assets/uploadfile-btn.png ":size=257x214")

**Source Code:**
[codepen-link](https://codepen.io/jellyhan27/pen/GRrbeJW)

<div id="question3" />

### 3. Input Element

#### 3.1 type = number

**Docs:** [`<input type="number">` - mdn](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/number)

**Syntax:**

```html
<label for="tentacles">Number of tentacles (10-100):</label>
<input type="number" name="tentacles" min="10" max="100" />
```

**UI result:**

![image](../assets/number-input-ui.png)

**Additional Attributes:**
| Attribute | Description |
|--|--|
| list | The id of the `<datalist>` element that contains the optional pre-defined autocomplete options |
| **max** | The maximum value to accept for this input |
| **min** | The minimum value to accept for this input |
| placeholder | An example value to display inside the field when it's empty |
| readonly | A Boolean attribute indicating whether the value is read-only |
| **step** | A stepping interval to use when using up and down arrows to adjust the value, as well as for validation|

### 4. Combinator Selectors
