### How to Config Webpack?  - Step by Step
#### Install Webpack
only as development dependency:
```
npm install webpack --save-dev
```
#### Write own config file

#### 1.  webpack.config.js
create `webpack.config.js` in your project root folder, using nodejs syntax.

#### 2.  four core concepts in webpack
* `entry`: can have one or multiple entry point. eg: app.js
* `Loaders`:  file-type dependent transformations, eg: `babel-loader, css-loader`
* `Plugins`: global transformations, eg: `uglify`
* `output`:  correctly ordered, concatenated together into an output, eg: `dist/bundle.js`
#### 3. set the Entry and Output
3.1 set the entry at: 
```
{
	entry:  './src/js/index.js',
}
```
3.2 output need a absolute path, we need to include `a built in node module`. Then we join the current directory name with our own path, here we want to put it at `./dist/js`.
```
const path =  require('path');
{
	path:path.resolve(__dirname,  'dist/js'),
}
``` 
#### 4.  Add NPM script
4.1  in package.json:
```
"scripts":  {

	"dev":  "webpack"

}
```
4.2 install the `webpack-cli`
```
npm install webpack-cli --save-dev
```
4.3 launch NPM script
```
npm run dev
```
![image](../assets/bundleResult.png ':size=449x175')

#### 5. Use the bundle.js as Example
we can use this bundle.js file directly, because everything is set and done, we can create a HTML file and use it.
5.1 create a new `index.html file` with the  very simple skeleton html code. A tricky point is that if you are using `VSCODE`, you can have a simple snippet to generate this block of code automatically by typing `! + enter`.
```
<!DOCTYPE  html>
<html  lang="en">
<head>
<meta  charset="UTF-8">
<meta  name="viewport" content="width=device-width, initial-scale=1.0">
<title>Document</title>
</head>
<body>
</body>
</html>
```
5.2 add our bundle.js as script
```
<body>
    <script  src="js/bundle.js"></script>
</body>
```
5.3 open the html file in the browser, and see the result, it will be the same result in your project. Cool, right? :) 

#### 6.  Dev & Production mode scripts
```
"scripts":  {

	"dev":  "webpack --mode development",

	"build":  "webpack --mode production"

},
```
we can find that the `bundle.js` file is much smaller than dev mode. All of this was compressed to just this piece of code.
![image](../assets/bundleProd.png ':size=452x192')




