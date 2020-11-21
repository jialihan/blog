## Deep Understand Flexbox in CSS

I. [Basics](#basic)

II. [Flexbox Container Properties](#flex-container)
- [flex-direction](#flex-direction)
- [flex-wrap](#flex-wrap)
- [justify-content](#justify-content)
- [align-items](#align-items)
- [align-content](#align-content)

III. [Flexbox Item Properties](#flex-item)

- [order](#order)
- [align-self](#align-self)
- [flex-grow](#grow)
- [flex-shrink](#shrink)
- [flex-basis](#basis)
- [shorthand: flex](#flex)


<div id="basic" />

### I. Basics

#### 1. flexbox includes two parts
* Flexbox Container (parent)
* Flexbox Item (children)

#### 2. simple starting code
- `flex` : this equlals to default property values: 
	```
	flex-direction: row;
	flex-wrap: nowrap;
	```
- `inline-flex`
```css
// block level container
display: flex;  
// inline level container
display: inline-flex; 
```
#### 3. main axis & cross axis

<div id="flex-container" />

### II. Flexbox container property

<div id="flex-direction" />

#### 1. flex-direction: 
reference link: [flex-direction](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-direction)
Values for flex-direction:
- `row` (default)

    ![image](../assets/flexrow.png ':size=285x185')

- `row-reverse`

    ![image](../assets/flexrowrev.png ':size=285x185')

-  `column` 

	![image](../assets/flexcol.png ':size=285x185')

- `column-reverse`

	![image](../assets/flexcolrev.png ':size=285x185')

<div id="flex-wrap" />

#### 2. `flex-wrap`
Here are multiple **wrap values**:
- nowrap (default)
- wrap
- wrap-reverse

<div id="justify-content" />

#### 3. [`justify-content`](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content) : align items on **main axis**

Here are multiple **direction value** examples:
*  **flex-start**: this is the **default**, items are packed toward the start of the flex-direction.

*  **start**: items are packed toward the start of the `writing-mode` direction.

	![image](../assets/flexstart.png)

*  **flex-end**: items are packed toward the end of the flex-direction.

*  **end**: items are packed toward the end of the `writing-mode` direction.

	**!!!Compare flex-end vs. end**

	When we have other flex layout properties, `flex-end` will respect apply on flex items, but `end` will not respect the flex properties.
	```
	flexbox {
	display: flex;
	flex-direction: row-reverse;
	}
	```
	We have the following difference between `writing mode` or `flex layout direction`:

	![image](../assets/flexend.png)

*  **center**

	![image](../assets/flexcenter.png)

*  **space-around**: css distribute items evenly. Items have a half-size space on either end.

	![image](../assets/spacearound.png)

*  **space-between**: items are evenly distributed in the line; first item is on the start line, last item on the end line.

	![image](../assets/spacebtw.png)

*  **space-evenly**

	![image](../assets/spaceeven.png)


<div id= "align-items" / >

#### 4. [`align-items`](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items) : align items on **cross axis**

- stretch: default

- flex-start

- flex-end

- center

- baseline: align to the baseline of the actual content (eg: some texts)

**UI examples:**

![image](../assets/alignitems.png ':size=327x469')

<div id= "align-content" / >

  #### 5. [`align-content`](https://developer.mozilla.org/en-US/docs/Web/CSS/align-content) : sets the distribution of space between or around on **cross-axis**

<div id="flex-item" />

### III. Flex Item properties

<div id="order" />

#### 1. `order` property

* order: default is 0
* larger order means later position, smaller order means early position.
* even when we change directions to `column`, or `row-reverse`, it will be correct in that **corresponding & matched** order.

For example, we move `item6` to front, and move `item1` to the last:

```
.container.order :nth-child(1){
	order: 2;
}
.container.order :nth-child(6){
	order: -1;
}
```
**UI examples:**

![image](../assets/flexorder.png)

 <div id="align-self" />
 
#### 2. **align-self**
It overrides a grid or flex item's [`align-items`](https://developer.mozilla.org/en-US/docs/Web/CSS/align-items  "The CSS align-items property sets the align-self value on all direct children as a group.") value.
For example:
* Parent container:  
	```
	align-items: center;
	```
* Child Item:  this value will override and take effect on this child
	```
	align-self: start;
	``` 

**UI examples:**

![image](../assets/alignself.png ':size=472x319')
	
<div id="grow" />

#### 3. **flex-grow**
it let items to take up all remaining spaces !!!

Then how much spaces each item can grow?

- **default value: 0**, not grow

- flex-grow = 1: all items take up equally the remaining spaces

- 2nd item`flex-grow=3`, other other item `flex-grow=1`. Then spaces are splitted into (1+3+1+1) = 6 units, 2nd item takes up `3 unit`, while other takes up `1 unit`.

```
.container.grow :nth-child(1){
	flex-grow: 1;
}
.container.grow :nth-child(2){
	flex-grow: 3;
}
.container.grow :nth-child(3){
	flex-grow: 1;
}
.container.grow :nth-child(4){
	flex-grow: 1;
}
```
**UI examples:**

![image](../assets/flexgrow.png)

<div id="shrink" />

#### 4. **flex-shrink**

-  **default value: 1**: allow item to shrink, if the size of all flex items is larger than the flex container.

- when 2nd item shrink = 2, make item smaller, but due to the content text inside of the item, it might not shrink as much as you expected!
- ~~**value 0**: **not allowed to shrink!!!!!**~~

**UI examples:**

![image](../assets/flexshrink.png)

<div id="basis" />

#### 5.  "flex-basis" vs "height" vs "width"

it sets the base size on **main-axis**. In row-direction, it means `width`, in column-direction, it means `height`. And **it will override the original "width" or "height" value** depends on the main axis.

Values on flex-basis property:
-  **auto**: **Default** value. The length is equal to the length of the flexible item. If the item has no length specified, the length will be according to its content

-  **number:** number values 100px, or percentages 25%

<div id="flex" />

#### 6. shorthand property: "flex"

1 ) Syntax: 
**flex: grow_val |  shrink_val | flex-basis_val;**

eg: `flex: 1 1 20%;`

2 ) How it works?

- according to the **flex-basis**value: eg: 100px

-  **container has remaining space** to grow or shrink

- according to the grow/shrink property, **calculate** each item's **adjusted** length.

- **add that adjusted value** to the **flex-basis** on each item
