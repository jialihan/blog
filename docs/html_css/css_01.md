## CSS basics
### 1. what is CSS?
HTML: **C**ascading **S**tyle **S**heets
CSS defines exactly how HTML looks like.
HTML is `content`, and CSS is `style`.
### 2. how to use css ?
There are 3 ways to use CSS:
*  CSS code right inside an HTML tag `using the style attribute`.
	```
	<p style="font-size: 120%">
	```

* CSS code in the `<head> section` of an HTML document.
	```
	<style>
		p {
			font-size: 120%
		}
	</style>
	```
* CSS code can be put into an external file, `.css` file.
	```
	p {
		font-size: 120%
	}
	```
	Then we need to link external css file into html file use `<link>` tag: 
	```
	<link rel="stylesheet" type="text/css" href="style.css">
	```
	`<Link>` tag and attributes introduction:
	The  `<link>`  tag defines the relationship between the current document and an external resource. The  `<link>`  tag is most often used to link to external style sheets. The  `<link>`  element is an empty element, it contains attributes only.
	Attributes:
	- `href`:  Specifies the location of the linked document
	- `rel`: Required. Specifies the relationship between the current document and the linked document. Example: 
		```
		alternate  
		author  
		dns-prefetch  
		help  
		icon  
		license  
		next  
		pingback  
		preconnect  
		prefetch  
		preload  
		prerender  
		prev  
		search  
		stylesheet
		```
	
### 3. how to write css code?
1 )  For example: 
text selector `h1`: 
```
h1 {
	// decoration block
}
```
2 )  common styles for multiple selectors: `h1, h2`
```
h1, h2 {
  color: green;
}
```
3 ) child elements inherit parent style
So child elements inherit the properties from their parent elements. Unless we overwrite their styles. For example: we define common styles in whole `<body>` selector, then we can `adjust & override` styles in different `children elements.`
```
body {
  font-size: 18px;
}
h1 {
  font-size: 40px;
}
```

### 4. Colors in CSS
1 ) Use RGB model in colors: #RRGGBB, hex value from `hex: 0` to `hex: ff`:

![image](../assets/colorcss.png ':size=444x252')

For example: #1da717 => red: `1d`, green: `a7`, blue: `17`

2 ) Sometimes we only write 3 numbers for hex value, for example:
If all six values in a hex code are the same, like `#555555`, we simply write `#555`.

3 ) transparency parameter
CSS colors can also have transparencies. If we want colors for transparencies, we do not use the hexadecimal notation.  Color: `rgba(r, g, b, 0.75)` but with 75% transparency.

![image](../assets/transparency.png ':size=663x128')

### 5. More selectors: Class and Id
1 ) difference of class and id
* `class`:  the same class can be attributed to as many elements as you like
	```
	// html
	<p class="article-text">
	
	// css
	.article-text{
	    border: 2px solid blue;
	}
	```


* `ID`:  can be used only once inside each HTML document.
	```
	// html
	<p id="author-text">
	
	// css
	#author-text {
	    width: 100px;
	}
	```
### 6. Box model in CSS
1 ) Basically, all HTML elements can be seen as boxes. Box model allows us to define space between elements and to add a border around elements. The box model consists of `margins, padding, borders and content `of the box.
* Content: text, images, etc.
* Padding: transparent area around the content, `inside of the box`.
* Border: goes around the padding and the content.
* Margin: space between boxes, `outside of the box`.

![image](../assets/boxcss.png ':size=360x264')

2 ) **Box-sizing**: can define the width and height
With box-sizing, border-box in css we can define the height and width for the entire box, rather than just for the content.

![image](../assets/boxsizing.png ':size=352x261')

3 )  **Block elements vs Inline elements**
* Block elements use the full width of the browser and force line breaks.
	For example: Headings and paragraphs are block elements:
	![image](../assets/inlineblock.png ':size=531x230')

 4 ) **asterisk selector: `*`**
Remove all default values of styles: we create some rules that will affect all elements on our web page, using the asterisk selector `*`.
```
    * {
        margin: 0;
        padding: 0;
    }
```

And we can add margin by ourselfves.
-   `margin-top`
-   `margin-right`
-   `margin-bottom`
-   `margin-left`

### 7. build layout
#### 7.1 use `<div>` tag  build boxes layout
`<Div>` stands for divide, and we use it to `divide our page into sections` by `creating boxes` where we `put our contents in`.
```
<div class="container"></div>
```
#### 7.2  use `margin: auto` align center
our container should be in center of the `x-axis`, style code is:
```
{
    width: 80%;
    margin-left: auto;
    margin-right: auto;
}
``` 
And the `auto` means that the left and right margin are adjusted automatically
according to the context of the element, which is the browser window in this case.

In order to write simple and clear code for margin in online, the order is:
```
margin: top right bottom left; 
```

#### 7.3 use `float` property
With float, an element can be pushed to the left or to the right, allowing other elements to wrap around it.
```
float: none|left|right|initial|inherit;
```

#### 7.4 use `border-radius` on image
```
img {
  border-radius: 50%;
}
  ```

### 8. element positioning: relative & absolute
* relative: elements were relative positioned elements. and their position on the page is determined by other elements.
* absolute: absolute position can be positioned anywhere we want inside their parent elements.
1 ) set the parent element to be `{ position: relative }`
2 ) in the child element to be `{ position: absolute }`
For example: html structure is like the following:
	```
	<div class="parent">
		<p class="child"></p>
	</div>
	```
  CSS style for this structure is like the following:
	```
	.parent {
		position: relative;
	}
	.child {
		position: absolute;
		top: 10px;
		right: 30px;
	}
	```






