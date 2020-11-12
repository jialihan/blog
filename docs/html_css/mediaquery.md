
# Responsive HTML - Viewport & Media Query

## I. hardware pixels and software pixels

### 1. absolute length on pixels

DOCS on W3C:
 [6.2. Absolute lengths: the  cm, mm, Q, in, pt, pc, px](https://www.w3.org/TR/css-values-4/#absolute-lengths) 

```
inches: 1in = 2.54cm = 96px
```

### 2. Physical Pixel vs CSS Pixel
* Physical height/width
* CSS height/width
* Device Pixel Ratio:
	The **device** manufacturer determines how many hardware **pixels** equals one software **pixel**, a concept known as the **device pixel ratio**.

Computation: 
**Device Pixel Ratio = Physical size / CSS size**

For example:
With Apple's Retina display, a single CSS **pixel** contained four hardware **pixels** (2 **pixels** wide and 2 **pixels** tall), meaning that the **device pixel ratio** is 2.


### 3. Browser recognize CSS pixels on mobile device

At first, when we open our dev tools and choose a mobile device, browser might consider the actual ` hardware pixels` so that our page looks good but actually is not.
After add the `<meta> tag for viewport`,  the browser is able to identify the real css logical pixels on that device simulator.

```html
<meta  name="viewport"  content="width=device-width, initial-scale=1.0">
```

**Reason** to use Viewport:

we will not be able to display our websites correctly or in a convenient and appropriate way on mobile devices.

Therefore in addition to the actual pixels, we need to take into account the device width and with that, automatically apply a specific **pixel ratio** which will then allow us to translate the hardware pixels into these software, the CSS pixels.

## II. Tools for  Responsive Design

### 1. viewport and media queries
| Viewport |  Media Queries |
|--|--|
| meta tag is required to be able to adjust our site to the device viewport | Change design depending on size |
| No design changes | Design changes defined by us | 


### 2. Understand Viewport

Attributes on `<meta> tag`:
```html
<meta  name="viewport" 
content="width=device-width, initial-scale=1.0, user-scalable=yes, maximum-scale=2.0, minimum-scale=1.0">
```

- `name`
- `content`: let the browser know that our device is a smaller device, it should **apply the pixel ratio conversion**. then it will consider the css pixel rather than actual hardware pixel.
	- `initial-scale`: can zoom in/out, 1.0 is original
	- `user-scalable`: default value is "yes"
	- `maximum-scale`: it simply restricts the maximum zoom level that is available.
	- `minimum-scale`: it simply restricts the minimum **zooming out** level that user is allowed to apply.


**Tip**:  
* i) **Never set the `maximum-scale=1.0`**
Because you are disabling the functionality to use pinch zoom on certain mobile devices, forcing people to view your page a certain way.

* ii) If  you don't want to show your mobile design page to your user or you don't have a responsive design page on mobile page, the safer or cleaner way is to remove the `<meta name="viewport">` tag in HTML.


### 3. Design your website "Mobile First"
- write your css for your mobile screen first
- then adjust for larger screens

## III. Add Media Queries

### 1. Basics to write media query

#### i) use **"min-wdith"** property: media query should kick in on desktop larger screens

This scenario happens when you design your mobile page at first, but solve the larger screens on desktop problem.

For example: 
It means when screen width larger than 640px, the h1 font-size will increase to 36px.
```
#product-overview  h1 {
	font-size: 20px;
}
@media(min-width: 640px)
{
	#product-overview  h1 {
		font-size: 36px;
	}
}
```

#### ii) use **"max-width"** property: media query should kick in for smaller mobile devices

This scenario happens when you design your desktop page at first, but solve the small screens problem.

#### iii)  media query is like the "if" statement

#### iv) multiple media queries

Here we have 640px, and 960px two different scenarios. Due to the cascading nature , the later will override the previous css code. Therefore, **you should specify your order properly.**
```
/* 16px = 1rem, when width >= 640px */
@media(min-width: 40rem){}

/* when width >= 960px */
@media(min-width: 60rem){}
```
Importance of the order on media queries ! 
~~Wrong way:~~
```
@media(min-width: 60rem){}
@media(min-width: 40rem){}
```

#### v) find the correct breaking points
For example:
- phone width
- tablet width
- desktop width

### 2. working with logic operator

- @media (min-width: 40rem) and (min-height: 60rem) {}
- @media (min-width: 40rem) and (orientation: portrait) {}
- @media (min-width: 40rem) and (orientation: landscape) {}


