## HTML basics
### 1. what is html?
HTML: **H**yper **T**ext **M**arkup **L**anguage
So, technically, it's not a programming language, it's a `markup language`.
### 2. html file structure
* `<head>`: contains web page's title, CSS styles,  some information for the browser or for search engines and more.  
* `<body>`: body element is where all the visible stuff of your web page goes, like all the content such as text, links, images, lists, and many more elements.
* `<!DOCTYPE hmtl>`:  The doctype declaration must be the very first thing in your HTML document, even before the HTML tag.

Then, we got our very basic structure of HTML document:
```
<!DOCTYPE hmtl>
<html>
	<head>  
	    <title></title>   
	</head>
	<body>  
	</body>
</hmtl>
```
### 3. Basic html tags and attributes
What is ab attribute? It means the additional information about an html element.
* `<title>` tag:  the title in browser's tab
* `<h1>-<h6>`tag: 
HTML has text for headings starting with the `<h1>` element for the main headings, all the way down to `<h6>` for less important headings.
* `<p>` tag
* `<strong>` tag:  make the text to be bold
* `<em>` tag: The HTML `<em> tag`marks text that has stress emphasis which traditionally means that the text is displayed in `italics` by the browser.
* `<u>` tag :  underline text tag

![image](../assets/texttag.png ':size=478x55')

* `<img>` tag:
   attributes:
    - src:  specify the path to the image
    - alt: specify an alternate text for the image, if the image for some reason cannot be displayed
    - width
    - height
 
  Code example
    ```
    <img src="author.jpeg" alt="author photo" width="140" height="180">
    ```
* `<a>` link tag: 
	
   **attributes**:
   - `href`:  can be an external link, link to a file, link to phone number, link to email, an so on.
   Examples:
	   ```
	  // an external link
	  <a href="https://www.google.com/">Link to Profile</a>
	  // link to phone number
	  <a href="tel:+11112223333"> call me at: +1 1112223333</a>
	  // link to email
	  <a href="mailto:someone@example.com">Send email</a>
	   ```

   - `target`:
    
  ![image](../assets/target_attr.png ':size=487x224')