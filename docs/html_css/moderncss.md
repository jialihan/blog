## Modern CSS

I. [CSS Modules](#modules)

II. [Use CSS Variables](#css-variables)
- [syntax](#syntax)
- [browser support](#browser)

III. [Understand Vendor Prefixes](#prefixes)

- [what is the vendor prefix](#p3-1)
- [which prefixes should use?](#p3-2)
- [browser support](#p3-3)

IV. [Polyfills](#polyfill)

V. [eliminate cross-browser inconsistency](#browser)

VI. [choose proper CSS Class Names](#class-name)

VII. [Vanilla CSS vs. CSS Frameworks](#vanilla-css)
- [Vanilla CSS](#p7-1)
- [Component Frameworks](#p7-2)
- [Utility Frameworks](#p7-2)


<div id="modules" />

### I. CSS Modules

CSS is split up into modules, for examole we might have seen the **media queries, selectors, animations, ...** etc. 

Official docs:  [CSS modules & working groups](https://www.w3.org/TR/tr-groups-all#tr_Cascading_Style_Sheets__CSS__Working_Group)

<div id="css-variables" />

### II. Use CSS Variables

CSS variables are a relatively new feature which allow you to put that reused value into a variable which you defined with this strange looking syntax, --my-color.

<div id="syntax" />

#### 1. Syntax:
- Define a variable:
	```css
	:root {
		--my-color: #ffffff;
	}
	```
- Use the variable
	```css
	.item1 {
		color: var(--my-color);
	}
	/* fallback value defined */
	.item2 {
		color: var(--my-color, #ffffff);
	}
	```

<div id="browser" />

#### 2. browser support
- pretty good: https://caniuse.com/?search=css%20variables
- IE not supported

<div id="prefixes" />

### III. Understand Vendor Prefixes

<div id="p3-1" />

#### 1. what is the vendor prefix

 Browsers implement new Features Differently and at different speed, then we get [vender prefixes](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix).
 
This is a mechanism which allows browser vendors to implement an upcoming feature in an early version. And these new features still can be used on old browsers.

For example:

 - `--webkit-box` for older versions of safari  
 - `--webkit-flex` for new versions of safari  
 - `--ms-flexbox` for IE and Edge

```css
.container {
	display: -webkit-box;
	display: -ms-flexbox;
	display: -webkit-flex;
	display: flex;
}
```

If the browser understand the new one, it will use the new one, but if not, they will choose some older available prefixed feature.

<div id="p3-2" />

#### 2. which prefixes should use?

properties that may need to use prefixes:
http://shouldiprefix.com/

Use "AutoPrfixer" CSS tools on line:
https://autoprefixer.github.io/

<div id="p3-3" />

#### 3. browser support

* Use support queries:
	some features simply aren't implemented in some browsers, we can have a query use `@supports`, if not support, browser will not execute the code block.
	```css
	body {
		font-familuy: sans-serif;
		margin: 0;
	}
	@supports(display:grid)
	{
		body {
			display:grid;
		}
	}
	```

<div id="polyfill" />

### IV. Polyfills

A polyfill is a JavaScript Package which enables certain CSS Features in Browsers which would not support it otherwise.

- it basically implement a certain feature by falling back to other CSS features and kind of replicating
- Polyfiils **not available for every css feature, eg: Grid** - cannot easily be replicated by vanilla CSS and Javascript.
- Polyfills **has a cost**, JS has to be downloaded and parsed ! it will affect performance.

what features can be polyfilled in CSS?
Docs: 
[HTML5-Cross-Browser-Polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills)

For example:
- rem
- background-size
- ...

<div id="browser" />

### V.  eliminate cross-browser inconsistency

**Problem:**
browser use different defaults	
- padding
- margin
- box-sizing

**Solution:**
 1 ) use Reset-Library, eg: Normalize.css, which can be imported in your html file.
 
 2 ) do it manually  use `*` universal css selector.
```css
* {
	margin: 0;
	box-sizing: border-box;		
}
```

<div id="class-name" />

### VI. choose proper CSS Class Names

#### 1. Correct way:
- use **kebab-case**: because CSS is case-insensitive
- name by feature: eg: `.page-title`

#### 2. Wrong way:
- ~~Don't use **snake-case**~~
- ~~Don't name it by style~~: eg: `.title-blue`

#### 3. Follow the CSS style convention: 
[Block Element Modifier](http://getbem.com/introduction/) (BEM): a uniform and consistent way of naming the CSS classes.
**Syntax:**
`.` BLOCK_NAME `__`ELEMENT `--` MODIFIER
	
	For example:
	```css
	.menu-main__item--size-big {} /* B + E + M */
	.header__brand {}  /* B + E */
	.button--success {} /* B + M */
	.header__title--color{} /* B + E + M */
	```

<div id= "vanilla-css" />

### VII. Vanilla CSS vs. CSS Frameworks

<div id= "p7-1" />

#### 1. **Vanilla CSS** - write all styles and layouts on our own
- full control
- No unnecessary code
- name classes as you like
- ~~build everything from scratch~~
- ~~dander of "bad code"~~

<div id= "p7-2" />

#### 2. **Component Frameworks** - a rich suite of pre-styled components, eg: third-party libs [Bootstrap](https://academind.com/learn/css/bootstrap-4-tutorial/) package.
- rapid development
- follow best practices
- no need to be an expert
- ~~No or little control~~ 
- ~~unnecessary overhead code~~
- ~~all websites look the same~~

<div id= "p7-3" />

#### 3. **Utility Frameworks** - eg: [Tailwind](https://tailwindcss.com/) package.
- faster development
- follow best practives
- no expert knowledge needed
- ~~little control~~
- ~~unnecessary overhead code~~







