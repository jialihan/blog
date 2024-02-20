## Extra bites

### 1. use Command Prompt

- `ls`: list all files
- `pwd`: show current folder directory
- `clear`: clear the termial
- `cd /`: show root directory of the Mac
- `cd ~`: show the User directory
- `open .`: open the folder of current directory
- `open <file_name>`: open a specific file
- `open -a "Calendar"`: open an application
- `mkdir`: create a folder
- `touch`: create a file
- 🔥`say hello world`: enable the audio sound on mac

### 2. run script.js in Node

```command
node script.js
```

Example code

```js
setTimeout(() => {
  console.log("hello");
}, 3000);
console.log(__dirname);
```

### 3. Modules in Node

Node doesn't support ES6 `import` syntax yet.
Node is using [CommonJS_modules](https://nodejs.org/api/modules.html), the `require()` syntax.

Export in node:

```
const largeNum = 100;
module.exports = {
    largeNum,
}
```

Import

```js
const m = require("./script2.js");
console.log(m.largeNum);
```

🎉Congratulations:
Latest Version of Node.js you will notice ES6 imports might work for you! That means you have Node version `12.2.0 or higher`!

### 4. ES6 modules in Node

[ecma doc](https://nodejs.org/api/esm.html#modules-ecmascript-modules):

> (node:99773) Warning: To load an ES module, set "type": "module" in the package.json or use the .mjs extension.

option1:

```
script.mjs
script2.mjs
```

option2: in package.json`

```json
// "npm init" to generate the package.json
{
  "type": "module"
}
```

### 5. Types of Modules

Built-in modules in Node:

- `require('fs')`: [file stystem module](https://nodejs.org/api/fs.html)
- `require('http')`: [doc](https://nodejs.org/api/http.html)

Npm package modules

- a helper tool: [nodemon](https://www.npmjs.com/package/nodemon), can use the command: `npm nodemon`, it can watch the file changes.
