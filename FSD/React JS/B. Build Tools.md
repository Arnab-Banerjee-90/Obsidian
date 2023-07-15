## 1. Initializing `npm`

We will first initialize `npm` using the below command, npm is a must as we will use it to install different node packages and keep track of the dependencies of the project.

`npm` is ubiquitious in any node frontend or backend application.

```bash
npm init -y
```

## 2. Prettier - Code Formatter

It is a code formatter which will take care of all the indentation, semicolon insertion etc. automatically.

### 2.1 `Prettier` VS Code Installation
---

Below steps are required to use `Prettier` with Vs Code.

#### 2.1.1. Creating the `.prettierrc` config file

This file is used to set our own formatting rules which prettier should use, for us we are planning to use the default formatting style of prettier, so the file should be just an empty object.

#### 2.1.2. Install the `prettier` VS Code extension

Search and install the official `Prettier` Vs Code extension. It is the one with the most downloads.

#### 2.1.3. Change some VS Code Settings

Open VS Code settings using `Ctrl + ,` Set `prettier.requireConfig` to true, `editor.formatOnSave` to true and  `editor.formatter.default` to Prettier.


### 2.2 `Prettier` Installation for Non Vs Code Users
---

#### 2.2.1 Installing the `npm` package for `Prettier`

`i` is shorthand for  `-install` and `-D` is a shorthand for `--save-dev`. Here we are installing a specific version of the `prettier` package which is 2.7.1

```bash
npm i -D prettier@2.7.1
```

#### 2.2.2. Setting up an `npm` script in `package.json`

For non Vs Code users to run Prettier, we set up a script in the `package.json` file, 

```js
"format": "prettier --write \"src/**/*.{js,jsx}\""
```

When we want to format our code using prettier, we will run `npm run format` and it will format all the  files with `.js` and `.jsx` extensions, which are present in the `src` directory of our project.

## 3. `Vite` - React build Tool

The build tool we are going to be using today is called [Vite](https://vitejs.dev/). Vite (pronounced "veet", meaning quick in French) is a tool put out by the Vue team that ultimately ends up wrapping [Rollup](https://www.rollupjs.org/) which does the actual bundling. The end result is a tool that is both easy to use and produces a great end result.

We want the following from our build tool

-  We can separate files out for code organization and have a tool stitch them together for us.
-  We can include external, third-party libraries from npm (like React!)
- The tool will optimize the code for us by minifying and other optimizing techniques

### 3.1 Install the `npm` package for `vite`

First, let's install the things we need for Vite.

```bash
npm install -D vite@3.1.4 @vitejs/plugin-react@2.1.0
```

The former is the tool itself and the latter is all the React specific features we will need.

### 3.2 Instruct the browser to use `ES6 Modules`

We need to add module to the script tag so that the browser knows it's working with modern ES6 modules that allows you in development mode to use modules directly. Instead of having to reload the whole bundle every time, your browser can just reload the JS that has changed.

```html
<script type="module" src="./App.jsx"></script>
```

In order for `vite` to do the `jsx` transpilations, we have to whenever we are using `jsx` , save that file with the `.jsx` extension.

### 3.3 Create the `vite` config file

Make a file in the root of your proejct called `vite.config.js` and stick this in there:

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  root: "src",
});
```

By default, Vite will find the index.html file in where-ever the root is and treat it as the head of a source graph. It'll crawl all your HTML, CSS, and JavaScript you link to from there and create your project for you. We don't have to do any more configuration than that. Vite will take care of the rest.

## 4. Installing the `npm` package for `React`

Okay, let's _actually_ install React to our project

```bash
npm install react@18.2.0 react-dom@18.2.0
```

We did not include the `-D` because React is not a development tool, it's a production dependency

### 4.1 Setup the build scripts in `package.json`

Now let's set up our scripts to start Vite. In package.json, put:

```json
// inside scripts
"dev": "vite",
"build": "vite build",
"preview": "vite preview"
```
