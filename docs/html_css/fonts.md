## Deep Understand Text Font in CSS

I. [Generic & Font Families](#generic-font)

II. [Using & Importing Font Families](#usage)

III. [Font Properties](#property)

IV. [Font Shorthand](#shorthand)

<div id="generic-font"/>

### I. Generic & Font Families

#### 1. Generic Families & Font Families
|generic| font (examples)|
|--|--|
| serif | Times New Roman, Georgia |
|sans-serif | Helvetica, Verdana|
|cursive | Brush Script, Mistral |
|monospace | Courier New, Lucida Bright |
|fantasy <br> (not a usual generic family) ||

#### 2. browser settings
For example: Chrome
**Settings -> Appearance -> Customize Font**

![image](../assets/browserfont.png ':size=406x537')

What will be displayed on screen?
- Default (not specify any font):  setting on browser
- Generic Family defined in CSS file: then use the font in browser settings
- Font Family defined in CSS file: this **might not available** on every user's computer
	- User's computer
	- Web fonts
	- Retrieve frome server

<div id="usage"/>

### II. Using & Importing Font Families

#### 1. Use default font families
If in our code, there is no specific assigned font-family, it will use browser's settings. We can inspect it in **"chrome dev tools -> computed"** .

#### 2. Understand "font-family" Syntax

font-family: [family-name](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#%3Cfamily-name%3E)  | [generic-name](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family#%3Cgeneric-name%3E)

- you can assign multiple font-families, if the first is not found, it will go and find the next one.
- generic-name at last working as a fall-back mechanism

For example:
```css
.body {
  font-family: "Unkown", Verdana, Arial, sans-serif;
}
```

#### 3. Locally Saved fonts - installed on user's computer

There are two factors we cannot control:
- we cannot force user to install that font-family
- we cannot force user to use a specific browser

Solution:
to go[CSS-Font-Stack](https://www.cssfontstack.com/), and check available font-family there, try to find one that most users might installed.

#### 4. working with google fonts - maybe a better way

* import as CSS link:
	```
	<link href="https://fonts.googleapis.com/css2?family=Roboto:wght@100&display=swap" rel="stylesheet">
	```
	Usage:
	```css
	font-family: 'Roboto', sans-serif;
	```
* Import as CSS style:
   in our .css file:
	 ```css
	@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@100&display=swap');  
	```
	or use a `<style>` tag in .html file
	```html
	<style>  
		@import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;1,100&display=swap');  
	</style>
	```
	
**Tip:**
A cleaner way in project: use **CSS style import** into our 	`shared.css` file, then all pages can use those fonts, and won't mess up the multiple .html files.

#### 5. Font Faces  "font-style"

Syntax:
```
font-weight: 100, 200, ..., 800, 900, bold, normal
```
Convention: A 400 weight is a normal one.

What if want to **import multiple font-weight** for the same font-family?

google fonts supports import multiple font faces. Select more and multiple font-faces, for example: `400;500;900`
```html
<style>  
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;900&display=swap');
</style>
```

Add `font-style` css property: for example:  "italic", because **browser can add italic font style**, but not from our font-faces.

```css
{
	font-weight: 100;
	font-style: italic;
}
```

#### 6. Import Custom Fonts - create our own fonts
If we downloads those fonts, we can see some **".ttf"** files. And import these local files in our project in css files, then **create our own font-face**.
```css
@font-face {
	font-family: "MyRoboto";
	src: url("Roboto-Regular.ttf");
}
```
Use our custom fonts:
```css
{
	font-family: "MyRoboto", sans-serif;
}
```

Import multiple font faces, be careful to add the important font-weight to differentiate, otherwise, it will override on the same font family. For example: we have 3 font-faces of normal, 700,900.

```
@font-face {
	font-family: "MyRoboto";
	src: url("anonymousPro-Regular.ttf");
}
@font-face {
	font-family: "Roboto";
	src: url("Roboto-Bold.ttf");
	font-weight: 700;
}
@font-face {
	font-family: "Roboto";
	src: url("Roboto-Black.ttf");
	font-weight: 900;
}
```

Helpful Resources: **[Common weight name mapping](https://developer.mozilla.org/en-US/docs/Web/CSS/font-weight)**
| Value | Common weight name |
|--|--|
| 100 | Thin (Hairline) |
| 200 | Extra Light (Ultra Light)|
| 300 | Light |
| 400 | Normal (Regular) |
| 500 | Medium |
| 600 | Semi Bold (Demi Bold) |
| 700 |  Bold |
| 800 | Extra Bold (Ultra Bold) |
| 900 | Black (Heavy)|
| 950 | [Extra Black (Ultra Black)](https://docs.microsoft.com/en-us/dotnet/api/system.windows.fontweights?view=netframework-4.8#remarks) |

**Tip:**
Manuel refers to an IE issue with @font-face. You will find a hint here:
[https://www.w3schools.com/cssref/css3_pr_font-face_rule.asp](https://www.w3schools.com/cssref/css3_pr_font-face_rule.asp)
**_"Tip:_** _**Use lowercase letters for the font URL**. Uppercase letters can give unexpected results in IE! "_


#### 7. understand font-format 

1 ) TTF/OTF - **TrueType and OpenType** font support
Syntax in CSS:
```css
@font-face {
	font-family: "MyRoboto";
	src: url("Roboto-Regular.ttf") 	
	format("truetype");
}
```
2 ) WOFF - Web Open Font Format
It's **compressed** and smaller and make our website faster, compression and loading time is smaller. 
But the browser support is not that good for IE, and limited Safari.
Syntax in CSS:
```css
@font-face {
	font-family: "MyRoboto";
	src: url("Roboto-Regular.woff2") format("woff2"),
		 url("Roboto-Regular.woff") format("woff");
}
```

<div id="property"/>

### III. Font Properties

#### 1. [font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)

- pixel values
- [em](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units): size of parent element
- rem: 1rem = 16px
- percentages


#### 2. [font-variant](https://developer.mozilla.org/en-US/docs/Web/CSS/font-variant)

For examples:
```css
font-variant: small-caps;
font-variant: common-ligatures small-caps;
```


#### 3. [font-stretch](https://developer.mozilla.org/en-US/docs/Web/CSS/font-stretch)

- keywords
- percentage values

Usage examples: 
```css
/* Keyword values */
font-stretch: condensed;
font-stretch: normal;
font-stretch: expanded;

/* Percentage values */
font-stretch: 50%;
font-stretch: 100%;
font-stretch: 200%;
```

Keyword to Numeric Mapping:

| Keyword | Percentage |
|--|--|
| 50% | `ultra-condensed` |
| 62.5% | `extra-condensed`|
| 75% | `condensed` |
| 87.5% | `semi-condensed` |
| 100% | `normal` |
| 112.5% | `semi-expanded` |
| 125% | `expanded` |
| 150% | `extra-expanded` |
| 200% | `ultra-expanded` |

#### 4. [letter-spacing](https://developer.mozilla.org/en-US/docs/Web/CSS/letter-spacing)
Use length as values: px, em, rem, percentages, etc, also can use keyword values.

Usage examples:
```css
/* Keyword value */
letter-spacing: normal;
/* <length> values */
letter-spacing: 0.3em;
letter-spacing: 3px;
letter-spacing: .3px;
```
#### 5. [white-space](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space)

- `normal`
- `nowrap`: Collapses white space as for  `normal`, and show in one line, no line breaks.
- `pre`: white spaces are **preserved** !
- `pre-wrap`
	Sequences of white space are **preserved**. Lines are broken at newline characters, at `<br>`, and as **necessary to fill line boxes**.
- `pre-line`
	Sequences of white space are **collapsed**. Lines are broken at newline characters, at `<br>`, and as **necessary to fill line boxes**.

#### 6. [line-height](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height)

- Preferred Way: Number: use this number multiplied by the element's font size 
	```css
	line-height: 3.5;
	```
- Percentage : multiplied by the element's computed font size. ~~Percentage~~ values may produce unexpected results.
	```css
	line-height: 150%;
	```
- Length value:
  Attention: Values given in **em** units may **produce unexpected results** , multiplied with parent element's font-size, not its own font-size.
	```css
	line-height: .3px;
	line-height: 3em;
	```
- `normal`
Depends on the user agent. Desktop browsers (including Firefox) use a default value of roughly **`1.2`**, depending on the element's `font-family`.

#### 7.  [text-decoration](https://developer.mozilla.org/en-US/docs/Web/CSS/text-decoration)

Usage examples:
```css
text-decoration: underline;
text-decoration: underline dotted;
text-decoration: underline dotted red;
```

1 ) [text-decoration-line](https://developer.mozilla.org/en-US/docs/Web/CSS/text-decoration-line) property:
- underline
- overline
- line-through

2 ) [text-decoration-style](https://developer.mozilla.org/en-US/docs/Web/CSS/text-decoration-style) property:
- solid
- double
-  dotted
-  dashed
-  wavy

Tip:
`<a> tag` has a default decoration of "**underline**". 

3 ) [text-decoration-color](https://developer.mozilla.org/en-US/docs/Web/CSS/text-decoration-color)

#### 8. [text-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow)

Syntax:
**text-shadow: offset-x | offset-y | blur-radius | color**
For example:
```css 
text-shadow: 5px 5px 2px lightgray;

/* offset-x | offset-y | color */
text-shadow: 5px 5px #558abb;

/* color | offset-x | offset-y */
text-shadow: white 2px 5px;
```

<div id="shorthand"/>

### IV. Font Shorthand

Some properties are required, some are optional, but the order matters.
#### 1. required properties:
Syntax: **font: font-size | font-family;**
```css
font: 1.2rem "Roboto", sans-serif;
```
#### 2. add font-weight
Syntax: **font: font-weight | font-size | font-family;**
```css
font: 700 1.2rem "Roboto", sans-serif;
```
#### 3. add font-variant 
Syntax: **font: font-variant | font-weight | font-size | font-family;**
```css
font: small-caps 700 1.2rem "Roboto", sans-serif;
```

#### 4. add font-style 
Syntax: **font: font-style | font-variant | font-weight | font-size | font-family;**
```css
font: italic small-caps 700 1.2rem "Roboto", sans-serif;
```

#### 5. add line-height along with font-size

Syntax: **font: font-size/line-height**
```css
/* line-height 1.3 of font-size */
font: 1.2rem/1.3 "Roboto", sans-serif;
```

#### 6. Syntax and Order Summary
Syntax: 
font: font-style | font-variant | font-weight | font-size/line-height | font-family;

Tip:
- font-family is always the last one
- add optional properties at the beginning

#### 7. System Default shorthand
- `menu`
- `status-bar`
- `caption`
- `small-caption`
- `icon`
- `message-box`

Usage examples:
```css
{
	font: menu;
}
```

#### 8. Loading Performance of custom fonts & "[font-display](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-display)"

The **`font-display`** descriptor determines how a font face is displayed based on whether and when it is downloaded and ready to use. **It only affects when we import and create font faces.**

1 ) Understand the font display **timeline**
- block-period: 
time to wait the context to be loaded, it must render an _invisible_ fallback font face **(text won't be rendered)**, and space there should be reserved! 
- swap-period:
 Time here is **infinite**:
	 - if still handling: it shows the fallback font face.
	- if successfully loads, it shows our custom font faces.
- failure-period
If the font face is not loaded, the user agent treats it as a failed load causing normal font fallback.

2 ) values:
- swap: 
   no block-period, and infinite swap-period
- block: 
   a short block-period, and infinite swap period 
- fallback: 
  very short block-period, and **short swap-period**, if still cannot load, show the fallback font face again.
- optional:   
 extremely short block-period, and **NO swap-period**. the browser decides on the internet connection to show custom-font or fallback.
- auto: 
  browser default value

3 ) usage examples:
```css
@font-face {
	font-family: "MyRoboto";
	src: url("Roboto-Regular.ttf");
	font-display: optional;
}
```

4 ) browser support is **bad**
https://caniuse.com/?search=font-display