## Understand CSS Specificity 

### O.Preface

**What is Specificity?**

**Specificity** is the means by which **browsers decide which CSS property values are the most relevant to an element** and, therefore, **will be applied**. Specificity is based on the matching rules which are composed of different sorts of [CSS selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference#selectors).

**Docs:** [Specifics on CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/)

**Online Specificity Calculator**: [test your css-selector here](https://polypane.app/css-specificity-calculator/#selector=)

### 1. type of css selectors
* universal selector: 
	```
		* {
			box-sizing: border-box;
		}
	```
* Element/ type selector:
	For example:
	```
	h1 {
	}
	head {
	}
	section {
	}
	p{
	}
	body{
	}
	```
* Classes Selector: use **". className"** syntax
	 ```
	 .className {
	 }
	 ``` 
* Attribute Selector: use **[attr_name]** syntax
	```
	[disable]{
	}
	[id="yourID"] {
	}
	[name="targetName"] {
	}
	```
* ID selector: use **#** syntax
	```
	#yourID {
	}
	```

### 2. Selector selector Order: from high to low
  <font size="5"> Inline-style >>
   ID selector >>
    .classes/:pseudo-class/[attribute] >>
    Tag or ::pseudo-element selector
 </font>
 ![image](../assets/specificity.png)
 
 ### 3. Examples
 * highest 
	 ```
	 <h1 style="background: red;"></h1>
	 ```
 * Medium
	 ```
	<h1 id="myh1"><h1>
	#myh1 {
	}
	```
* Lower
```
	<h1 id="myh1" class="headline1"></h1>
	.headline1 {
	} 
	// same with 
	[id="myh1"] {
	}
```
 * Smallest
    ```
    h1 {
    }
    ```

 ### 4. Reference
[CSS/Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)

 [Calculate Specificity](https://polypane.app/css-specificity-calculator/#selector=%5Bid%3D%22test%22%5D)