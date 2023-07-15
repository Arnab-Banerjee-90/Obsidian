## 1. Hello World App using vanilla React
---
Let's start by writing pure React, no compile step, no JSX, no Babel, no build step . Just a JS file and an HTML file

### 1.1. The ```index.html``` file
---
This file is where React will be rendered onto. It has the below structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="./style.css">
    <title>Adopt Me</title>
</head>
<body>
	// Root div to be rendered using react 
    <div id="root">not rendered</div>

	// Loading React libraries using CDN
    <script src="https://unpkg.com/react@17.0.2/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.development.js"></script>
    
    // Script containing Vanilla React code
	<script src="./App.js"></script>
</body>
</html>
```

the HTML file contains a root div. We'll render our React app here in a sec. It doesn't have to be called root, just a common practice.

We are loading the react libraries using the below script tags

-   The first is the React library. This library is the interface of how to interact with React; all the methods (except one) will be via this library. It contains no way of rendering itself though; it's just the API. This Package is shared between React DOM, React Native, React ART, React 3D, React 360 etc.

-   The second library is the rendering layer. Since we're rendering to the browser, we're using React DOM. There are other React libraries like React Native, React 360 (formerly React VR), A-Frame React, React Blessed, and others. You need both script tags. The order is not important.

-   The last script tag points to the file where we will be writing the vanilla react code, which will be rendered by the above HTML file in the root div.

### 1.2. The  ```App.js``` file
---

#### 1.2.1. React Functional Component
---
React deals with what is called **React Components**. The first type of component we will be working with is called **React Functional Component** . Every functional component has the below structure

```js
const App = ()=>{
	return(React.createElement())
}
```

App is the name of the component, it is a convention to always capitalize the first letter of React components

Every React Functional Component must return a call to ```React.createElement()``` (this is for vanilla React)

The first Element created within any React Functional Component ( via `React.createElement()` ) must be an HTML element, which contains all other elements/components.

Every React Functional Component is made up of several other child React Functional Components and/or HTML elements. 

#### 1.2.2. The  `React.createElement()` function
---

The ```React.createElement()``` function creates the HTML elements or the React Functional components, it is equivalent to invoking a JS function.

Each and every React functional Component, has only ONE main ```React.createElelment()``` call, all other calls must be children of this call. This call to ```React.createElement()``` can be thought of as the ```<body></body>``` tag of an HTML document. (In actuality it is mostly a `<div></div>`)

This function creates one instance of some HTML element or React component, it takes 3 arguments

- First argument is compulsory, we can pass it a string, it will create an HTML tag which we passed as the string. If instead of a string we passed text, then it would treat it as a React component and try to create that.

- Second argument is an optional JS object, here we pass in the element's attributes and their values as key value pairs, or incase we are invking a React Component, then this object contains so called props data.

- Third argument is either the content of the DOM element of the first argument, or an array of child elements which the current element/component will contain, in that case every element within this array will also be a  ```React.createElement()``` call

```js
const App = ()=>{
	return(
		React.createElement("h1",{},"Welcome to React!!")
	)
}
```

The ```App``` component is defined, all we have to do now is create it using ```React.createElement(App)``` and then render the same.

#### 1.2.3. Creating and Rendering the React Component
---

In the previous section we have defined the ```App``` component, now we need to invoke it and render it, we do it in the below steps

-  Get a handle of the HTML element where React will be rendered, we had already created a root div in `index.html` for this purpose

	```js
const container = document.getElementbyID("root")
```

-  Make this container the root of the react App

```js
const root = ReactDOM.createRoot(container)
```

-  Create the App component and render it within the created root, here we are passing text not as a string, so we are telling React that ```App``` is not an HTML element but a react component, so React will create the said component based on the code we have written previously

```js
root.render(React.createElement(App))
```

#### 1.2.4. The ```App``` component with child elements
---

As we have mentioned before that a functional React compoent can have only one main ```React.createElement()``` function call. So if we want to have more than one element, those elements will have to be its children.

```js
const App = ()=>{

	return(
		// Invoking the container HTML element
		React.createElement("div",{},[
			// Invking other HTML elements which are its children
			React.createElement("h1",{},"Welcome"),
			React.createElement("div",{},"Intro to React")
		])
	)
}

const container = document.getElementById("root")
const root = ReactDOM.createRoot(container)
root.render(React.createElement(App))
```

From an HTML point of view the  ```App``` component is as follows, the first div is the mandatory container element, which contains an h1 and another div element.

```html
<div>
	<h1>Welcome</h1>
	<div>Intro to React</div>
</div>
```

## 2. Props and data flow
---
In the above section we have defined a functional component ```App```  and then invoked and rendered it.

```js
// Creating the App function
const App = ()=>{
	return(React.createElement())
}
// Invoking said function via Reat.createElement()
root.render(React.createElement(App))
```

The definition of any React functional component is equivalent to defining a JS function, and calling ```React.createElement()``` on said function is equivalent to invoking the created function.

### 2.1. Props are function arguments
---
Just like we can define JS functions which takes in arguments, and then we can invoke said functions with different arguments, We can do the same with React functional components, these arguments are called props.

```js
// Defining a compoent which can accept arguments
const App = (props)=>{
	return(
		React.createElement("div",{},[
			React.createElement("h1",{},props.name),
			React.createElement("div",{},props.place)
		])
	)
}
// Invoking said component with arguments
root.render(React.createElement(App,{name:"Arnab",place:"Kolkata"}))
```

The ```props``` parameter in the definition of the functional component signifies that this component can receive arguments when it is called via React.createElement like --> ```React.createElement(App,{name:"Arnab",place:"Kolkata"})```. The arguments are passed as key value pairs of the second argument (object) of the createElement function. This object is referenced by the props identifier in the App function definition.

As we can only invoke React functional components from either the render function, or from the definition of another React functional component (parent component), thus props flows from parent to child component.

### 2.2. Data flow from parent to child

In the below code we have multiple ```Student``` components which is "invoked" by the main ```App``` component (App is invoked by ```root.render()```).

React component invocations always start from whatever is invoked by `root.render()`

- First `App` is invoked by `root.render(React.createElement(App))`
- `Student` component is invoked with some data, from within the body of the `App` component. It is somewhat similar to a function invoked from inside another function.

```js
const App = ()=>{
return(
	React.createElement("div",{},[
		React.createElement("h1",{},"Welcome"),
		// Invoking another React component with some data
		React.createElement(Student,{name:"Arnab",branch:"Science",roll:"01"}),
		React.createElement(Student,{name:"Modak",branch:"Commerce",roll:"02"}),
		React.createElement(Student,{name:"Khudi",branch:"Arts",roll:"03"})
		])
	)
}

const Student = (props) => {
return(
	React.createElement("div",{},[
		React.createElement("h2",{},props.name),
		React.createElement("div",{},props.branch),
		React.createElement("div",{},props.roll)
		])
	)
}

const container = document.getElementById("root")
const root = ReactDOM.createRoot(container)
root.render(React.createElement(App))
```
