## 1. `JSX` (JavaScript XML)

`React.createElement()`  creates HTML elements and React components composed of said elements. In a sense we are writing JavaScript, which then imitates HTML. So what if we could eliminate `React.createElement()` entirely and just use HTML directly in our `.js` files ?

Well, `JSX` allows us to do just that, using `JSX` , if we want to create an `h1` element, we can, in our `.js`  file say

```html
<h1 id="main-title">My Website</h1>
```

Which then translates to 

```js
React.createElement("h1",{id:"main-title"},"My Website")
```

As we will from now onwards be using `JSX` syntax in our `.js` files, we have to rename our `.js` files to `.jsx` for our build process to work correctly.

For Invoking a React Component instead of `React.createElement(App)` we can now just say `<App/>`

### 1.1 Writing `JavaScript Expressions` inside `JSX`

Inside `JSX` only JavaScript expressions are allowed, and they have to be written inside `{ }` 

A JavaScript expression is anything which comes to the R.H.S of an `=`  operator

Suppose we have a React Component called `Pet` which accepts `props` when invoked

```jsx
const Pet = (props) => {
	return(
		<div>
			<h1>{props.name}</h1>
			<h2>{props.animal}</h2>
			<h2>{props.breed}</h2>
		</div>
	)
}
```

In the above code we have JS values such as `props.name` , in order for those to get evaluated properly we have to use them inside `{ }`

### 1.2. Passing `Markup` as props

We have already seen previously, that when we invoke a React Component, we can do so by using HTML tags surrouding the markup and we can also send props to the said component by attaching said props as HTML attributes

in the above code for the component `Pet`, we can invoke it with props as follows

```jsx
<Pet name="Luna", animal="Dog", breed="Havanese" />
```

We have used a self closing tag, here, but what if we use a normal `opening` and `closing` tag for the `Pet` component, and write some Markup within it?

```jsx
<Pet name="Luna" animal="Dog" breed="Havanese">
	<h1>This is the Pet Component</h1>
</Pet>
```

The markup in between the `opening` and `closing`  `Pet` tags is also passed to the  `Pet` component as `props` and is accessible via `props.children` 

If `props.children` is used in the definition of the `Pet` component, then we can see the `h1` tag rendered to the output when we invoke `Pet` like as shown in the above code snippet.

In this way we can pass `Markup` as props

## 2. Hooks and Event Handlers

Suppose we have the below component

```jsx
export const SearchParams = ()=>{
	const location = "Kolkata"
	return(
		<div className="search-params">
			<form>
				<label htmlFor="location">
				Location
					<input id="location" value={location} />
				</label>
				<button>Submit</button>
			</form>
		</div>
	)
}
```


#### Note

- We can use normal JS syntax inside the body of the React functional component, before the `return` section begins, after that it is only `jsx` and using JS expressions inside `{ }`

- We cannot use any JS `keywords` inside `jsx` so notice that in the `jsx` we have used `className` and `htmlFor` instead of the `class` and `for` attributes, as those are reserved keywords in JS.

If we invoke and render the above `SearchParams` component , using `root.render(<SearchParams/>`, we will see that the value of the `input` field is fixed to the value of the variable `location` .

If we see closely, the `value` prop which is the contents of the input tag is tied to the variable `location` , which is not changing, we have not provided any functionality by which we can change the value of the location variable.

In react functional components, in order to handle change in our component variables (states) we use something called a `useState` Hook

#### So we need the following

- Something which will detect that we are trying to change something : This will be done by an event handler

- Handling the change in variable value once the handler is triggered : useState Hook

### 2.1 `useState` Hook (Keep track of change in variable value)

Let us modify the above code using `useState` hook and `onChange` event handler

```jsx
import { useState } from "react";
export const SearchParams = ()=>{
	const [location, setLocation] = useState("Kolkata");
	return(
		<div className="search-params">
			<form>
				<label htmlFor="location">
				Location
					<input 
						id="location" 
						value={location} 
						onChange={(e) => setLocation(e.target.value)}
					/>
				</label>
				<button>Submit</button>
			</form>
		</div>
	)
}
```

#### 2.1.1 Some Basic Features of `useState`

- The argument given to `useState`  is the default value of the state variable

- `useState` returns to us an array with two elements in it: the current value of that state and a function to update that state. We're using a feature of JavaScript called destructuring to get both of those things out of the array.

#### 2.1.2 How it Works

We use the `setLocation` function in the `onChange` attribute of the input. Every time the input is typed into, the `onChange` event will trigger, which is going to call the callback function which calls `setLocation` with what has been typed into the input. When `setLocation` is called, React knows that its state has been modified and kicks off a re-render.

- An event is triggered (Here the event which gets trigerred is `onChange` )
- The callback function attached to the `onChange` event triggers
- That callback function then calls the updater function with some value (here `setLocation()` is called with `e.target.value`)
- As `setLocation` was called, React detects that a State was updated, hence it re-renders the component with the updated state value

#### 2.1.3 Hooks needs to be created in the same order everytime

An absolutely key concept for us to grasp is hooks rely on strict ordering, if we have 3 Hooks, 

```jsx
const [location, setLocation] = useState("Kolkata");
const [name, setName] = useState("Luna");
const [breed, setBreed] = useState("Havanese");
```

Then at each re-render of the component, in which these hooks are declared, their order of creation must be same each time, in this case first `location` , then `name` and then `breed`

So hooks should never be declared within `if` statements or loops as it might change the order of creation of these hooks on a certain re-render of the component.

### 2.2 `useEffect` Hook (Run a function when state changes)

The `useEffect()` hook essentially allows us to write functions which is invoked everytime there is some state changes in the component.

`useEffect()` takes two arguments, first is a callback function which contains the code to be run, and the second one is the data dependency array containing the states, the changes of any of which will execute the callback function. 

#### 2.2.1 Triggering `useEffect` only once - Mounting

We can set it up so the callback function in `useEffect()` runs when the component is loaded for the first time only (”Mounting”)

```jsx
useEffect(()=>{ 
	console.log("This is printed out only on first load" 
},[])
```

#### 2.2.2 Triggering `useEffect` everytime any state changes

We can also set it up so the callback function in `useEffect()` runs whenever any state of the component changes (which is not a good idea). For that we just have to remove the second argument of `useEffect`

```jsx
useEffect(()=>{ 
	console.log("This is printed out only on first load" 
})
```

#### 2.2.3 Triggering `useEffect` when some specific state changes

This is the way `useEffect` is generally set up.

```jsx
useEffect(() => {
	requestPets();
}, [animal]);
```

In this case the `requestPets()` function only gets triggered when the state `animal` updates (i.e its updater function provided by `useState` runs)

#### 2.2.4 Infinite triggering of a `useEffect`

If the function which is invoked by `useEffect` updates any of the states responsible for trigerring `useEffect` in the first place, we then have the infinite triggering problem.

```jsx
import {useState} from "react"
const [animal. setAnimal] = useState("")
useEffect(() => {
	setAnimal(//some value);
}, [animal]);
```

### 2.3 Custom Hook

A Custom hook is nothing but a JavaScript function, to which we feed state/states and get output state/states. Inside the function body, react hooks like `useState`, `useEffect` are used in order to generate the output states based on the input states.

#### 2.3.1 General `custom hook` pattern
- It is a function named like `useXXX(A)`
- Takes in one or more input state `A`
- In the custom hook function body
	- It defines some states using `useState`
	- Triggers `useEffect` based on changes to the input state/states
	- In the callback of the `useEffect` it ultimately sets the states which were defined in the function body and returns them

```js
//-------------inside seperate file useXXX,js------
import {useState, useEffect} from "react"
export function useXXX(A){
	const [B, setB] = useState("")
	useEffect(()=>{
		// some logic
		setB(//some value)
	},A)
	return B
}
//----------inside the component.jsx-----------
import useXXX from "./useXXX.js"
import {useState} from "react"
const [A,setA] = useState("")
const B = useXXX(A)
```