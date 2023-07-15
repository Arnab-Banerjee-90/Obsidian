

# 1. What is NodeJS

NodeJS is a JavaScript runtime environment which is biult on top of the Chrome V8 engine. So, basically nodeJS is a JS runtime environement which lets us run JavaScript outside the browser, such as in our servers and other systems.

Node JS was developed by Ryan Dahl in 2009.

## NodeJS usecases

There are numeous usecases some of which are listed below

- API's and servers
- Databases (yes some DB's are built in Node)
- CLI's
- Build Tools — such as Babel, Web Pack etc.
- Automations
- Basic Scripting

## Installing Node

For Linux, MacOS and Windows with WSL 2, it is recommended to install node using nvm or node version manager.

[GitHub - nvm-sh/nvm: Node Version Manager - POSIX-compliant bash script to manage multiple active node.js versions](https://github.com/nvm-sh/nvm#installing-and-updating)

Once you have nvm installed, you need to install a Node version. You can download the latest LTS version with this command. `nvm install --lts`

## Executing node

### The Node REPL

just open the terminal and type the command `node` , this will fire up the node REPL (read evaluate print loop) which is a playground for writing code, and testing etc.

### File Execution

To do this first we need a file containing some JS source code saved under .js file extension. so lets make a file called `hello.js` and print someting out to the console

```jsx
console.log("Hello World");
```

Now to run hello.js in the terminal we just have to run the commad `node <path to hello.js>`

## Differences between Browser JS and NodeJS

### What node does not have, that browsers have

As node JS is running in our system, so we should not expect it to suppot features which are typically associated with browsers, a few examples are listed below

- functions such as `alert()`
- The entire DOM is absent in nodeJS, which is to be expected

### What we get in Node that we do not get in browser JS

We should be able to handle some things essential to a regular computer via node, because now via node JS is running as an OS level language

- File system
- Networking

# 2. Globals in Node

Like the Browser, Node.js comes with some practical globals for us to use in our applications.

### global

Think of this as like `window` object but for Node.js. If we console log it out we can see the global object has some familiar properties 

`clearInterval: [Function: clearInterval],
clearTimeout: [Function: clearTimeout],
setInterval: [Function: setInterval],
setTimeout: [Function: setTimeout]` 

these work exactly as their browser counterpart. 

The global also contains the `queueMicrotask: [Function: queueMicrotask]` which has to do with the event loop.

### __dirname

This global is a `String` value that points the the directory name of the file it's used in.

### __filename

`__filename` Like `__dirname`, it too is relative to the file it's written in. A `String` value that points the the file name.

### process

A swiss army knife global. An `Object` that contains all the context you need about the current program being executed. Things from env vars, to what machine you're on.

### exports, require, module

These globals are used for creating and exposing modules throughout your app.

# 3. Modules in NodeJS

There is no GUI in Node.js, no HTML or CSS. This also means there aren't any scipt tags to include JS files into our application. Node.js uses modules to to share your JavaScript with other JavaScript in your apps. No window or globals needed.

## The Common JS moduling pattern

The common JS moduling system uses the NodeJS globals such as require, module and exports in order to create and use modules

### using `module.exports` to export one or more functions

- If one function to be exported then we can use `module.exports = ask`
- If more than one function to be exported then we can either export an object or an array
    - `module.exports = {ask,ask2}`
    - `module.exports = [ask,ask2]`
- If a module file has more than one module.exports statement, then only the last one will be considered.

```jsx
//-----------The Module file index.js--------------------
var teacher = "Kyle"

function ask(question){
    console.log(question,teacher)
}
function ask2(question){
    console.log(question,teacher)
}

module.exports = {ask,ask2}; // exporting an object, with the functions as object properties
```

### using `require` to use the exported modules

require(<realtive path to the module file>) gives us the entire object/array which was exported

- We can either assign it to a single variable to get hold of the entire object

```js
//-----------------------File app.js-----------------
const mod = require('./index') // .js file extension not required
// mod refers to the entire object, and we can use mod.ask() or mod.ask2() to refer to the exported functions
```

- or we can use the dot operator to refer to different functions by different identifiers

```jsx
//---------------------File app.js-------------
const fun = require('./index').ask
const fun2 = require('./index').ask2
```

- Or we can also use object destructuring, in this case we have to use the same name, by which the functions were exported

```jsx
//---------------------File app.js-------------
const {ask,ask2} = require('./index')
```

## The ES6 Moduling pattern

Check out the below link for detailed discussion on the ES6 Modules.

[ES6 Modules ](https://www.notion.so/ES6-Modules-36f2935f014a470bb41da782aad2ca54) 

## Some internal node modules

Node.js comes with some great internal modules. You can think of them as like the phenonminal global APIs in the browser. Here are some of the most useful ones

- `fs` - useful for interacting with the file system.
- `path` - lib to assit with manipulating file paths and all their nuiances.
- `child_process` - spawn subprocesses in the background.
- `http` - interact with OS level networking. Useful for creating servers.