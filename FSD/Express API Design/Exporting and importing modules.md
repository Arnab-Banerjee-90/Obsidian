## 1. The `common JS` module system
---
`CommonJS` is a module system used in `Node.js`, which allows developers to organize and reuse code in their applications. `CommonJS` modules provide a way to encapsulate code by defining modules and exporting functionality for use in other modules.

A module is essentially a _separate file that encapsulates code and data_. Each module has its own scope, meaning the variables and functions defined within a module are not accessible outside unless explicitly exported. Modules can import functionality from other modules to use in their own code.

`CommonJS` modules follow a synchronous loading model, where modules are loaded and evaluated synchronously in the order they are required. This is suitable for server-side applications but may cause performance issues in browser-based JavaScript environments, which often prefer asynchronous module loading systems like `ECMAScript Modules (ESM) or ES6 modules`.

### 1.1. Exporting a module
---
We can export modules in `commonJS` using the below two ways

#### 1.1.1. Exporting using the `exports` object
---
We can use multiple `exports.[property_name]` in a single module file. In this method, always an object gets exported --> The `exports` object gets exported as anonymous object

```js
// File module.js
exports.a = 20

exports.b = 21

exports.c = 22

exports.d = 23

exports.e = 24
```

Using this above code we are trying to export the values of the variables a,b,c,d and e
but what gets exported is an object `{ a: 20, b: 21, c: 22, d: 23, e: 24 }`

We can either catch this object under any name then use the dot operator to access the properties, or we can use destructuring to get access to the variables we want

```js
// In App.js

let importedObject = require("./module")
console.log(importedObject) // { a: 20, b: 21, c: 22, d: 23, e: 24 }

let {a,b,c} = require("./module")
console.log(a,b,c) // 20 21 22
```

So we can summarize he above discussion in one line as 
--> Exporting using `exports.[property_name] = "Value"`, will export the variables as properties of an object.

#### 1.1.2. Using `module.exports`
---
We can only use one `module.exports` in a single file, and that has to be present in the end of the file.
The main difference being that we are not limited to exporting an object, if we want to export a single primitive, or an array or a function we can export it, but we can only export one thing

**Exporting a single primitive**

```js
//module.js
let a = 20
// we want to export a only
module.exports = a
```

```js
//app.js
let myNum = require("./module")
console.log(myNum) // 20
```

**Exporting an object**

```js
///module.js
let toBeExported = {
	a: 20, b: 21, c: 22, d: 23, e: 24
}

module.exports = toBeExported
```

```js
//app.js
let imported = require("./module")
console.log(imported) // {a: 20, b: 21, c: 22, d: 23, e: 24}
```

## 2. ECMA Script Module (ESM) or `ES6 modules`
---
Ensure that you are using a `Node.js` version that supports ECMAScript Modules. Starting from `Node.js` version `13`, ESM support is available by default. However, it's recommended to use a more recent version to leverage the latest features and improvements.

### 2.1. Loading `ES6 modules`
---
In order to use `ES6 modules` we need to make some changes. We can use `ES6 modules` in our project using any one of the below methods

#### 2.1.1 Loading `ES6 modules` using `.mjs` files
---
Rename your JavaScript files from `.js` to `.mjs`. This file extension is used to indicate that the file contains ECMAScript Modules.

#### 2.1.2. Update `package.json` file
---
In the `package.json` file add the `"type"` field and set it to `"module"`. This informs `Node.js` that your project is using ECMAScript Modules.

```json
// package.json
{   
	"name": "my-project",
	"version": "1.0.0",   
	"type": "module",   
	... 
}
```

### 2.2. Anonymous object export using the `export` keyword
---
This is just like using the `exports.propertName`  in `CommonJS`

We can use as many `export` keyword as we want, all the identifiers will be exported together as properties of a single object, So just like using the `exports` object in `CommonJS` we are forced to export only an object using the `export` keyword in `ES6 Modules`

```js
// File module.js
export let a = 20

export let b = 21

export let c = 22

export let d = 23

export let e = 24
```

As we have exported an anonymous object we cannot capture it into a variable directly. So we have to use the below syntax

```js
// File app.js
import * as myVar from "./module.js"

console.log(myVar) // { a: 20, b: 21, c: 22, d: 23, e: 24 }
```

As it is fixed that we can only export an anonymous object, hence rather than catch that anonymous object using `import * as obj` we can use object destructuring to capture the variables which we want

The variables which are exported as properties of the anonymous object, are named hence we have to use the same variable names when we are using object destructuring.

```js
// app.js
import {a,b,c} from "./module.js"
console.log(a,b,c) //20 21 22
```

### 2.3. `default` variable export using  `export default`
---
This is just like using `module.exports` in `CommonJS`. Using this method we can export whatever datatype we want, array, primitive, object etc. But be can have only one `export default` in one module file.

```js
// module.js
let i = 30;
let j = 40;
export default {i,j} //using shorthand object syntax
// we can also export the variables i,j as an array if we wanted
// export default [i,j]
```

The exported variable here an object, exported under the name `default`, hence 

We can catch this `default` variable under any name, here we cannot use destructuring as we are exporting a single variable `default`

`default` export cannot be destructured while importing

```js
// app.js
import value from "./module.js"
console.log(value) // { i: 30, j: 40 }
```

