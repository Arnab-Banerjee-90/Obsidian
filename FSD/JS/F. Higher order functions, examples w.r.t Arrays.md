
## 1. Function : Declaring Vs Calling
---
### 1.1 Declaring a function
---
```js
function square(num){
	return num*num
}
```

When JS `processes` this function declaration statement, it does the following
- It creates an `identifier/label`  named `square` 
- Saves the entire function body as strings of character in the memory with label square
The `num` identifier is called the `function parameter`, whenever we call the function we have to provide a value to be stored in the `num` identifier, in the function’s own local memory.

### 1.2. Calling a defined function
---
When we call a defined function we need to use the function name/identifier together with a set of parenthesis. We say `square(10)` .

In place of the identifier square, the entire function body is substituted, and the value 10 is stored in the function parameter. **The value we pass in the parenthesis when calling a function, is known as `argument`**. 

The **value of the function parameter, i.e. the argument** is stored inside the local memory of the function.

So the call `square(10)` is substituted as below.

```js
{return num*num}(num = 10)
```

## 2. Passing Functions as arguments
---
Not only we can pass values as arguments, we can also pass instructions (i.e. functions) as arguments to a function.

The function which was passed as an argument, can then be referenced by the parameter name used to capture it, and therefore can be executed inside the function to which it was passed.

```js
function copyArrayAndManipulate(array, instructions) {
 const output = [];
 for (let i = 0; i < array.length; i++) {
   output.push(instructions(array[i]));
 }
 return output;
}

function multiplyBy2(input) {return input * 2;}

const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
```

In the above example, we are passing the function `multiplyBy2` as an argument to the function `copyArrayAndManipulate` and inside the `copyArrayAndManipulate` function the reference to `multiplyBy2` function is saved under the label `instructions`

Instead of passing a named function as the argument, we can also pass an anonymous function expression as the argument as well

```js
const result = copyArrayAndManipulate([1, 2, 3], function (input){return input*2});
```

### 2.1. Higher Order Function
---
The outer function that takes in a function as an argument is our higher-order function, in this case the function `copyArrayAndManipulate` is our higher order function.

A function is also called a higher order function, if the said function returns a function as well.

### 2.2. Callback Function
---
The function which is inserted as an argument to the higher order function, is our callback function. The callback function is named as such, because the function is in essence called back/executed later from within the higher order function.

In our example the `multiplyBy2` function is the **callback** function, which is saved as the parameter `instructions` inside the higher order function `copyArrayAndManipulate`

### 2.3. Benefits of using Callbacks and Higher Order Functions
---
- It helps us write Declarative readable code: Map, filter, reduce - the most readable way to write code to work with data
- Callbacks are a core aspect of `async` JavaScript, and are under-the-hood of promises and `async-await`.
- Helps us to write more generalized functions, to write more DRY code.

## 3. The `Reducer` function
---
The Reducer function is one of the most versatile H.O.F function. 

The reducer function, reduces a list of values into a single value. the `copyArrayAndManipulate` function shown above is actually an example of a reducer function.

```js
function copyArrayAndManipulate(array, instructions) {
 const output = [];
 for (let i = 0; i < array.length; i++) {
   output.push(instructions(array[i]));
 }
 return output;
}

function multiplyBy2(input) {return input * 2;}

const result = copyArrayAndManipulate([1, 2, 3], multiplyBy2);
```
``
In order to become a reducer function we need to have the below components, lets check to see if the `copyArrayAndManipulate`  function checks all the boxes

- **A list of values** which will get reduced to a single value, here the list of values is the function parameter `array`
- **A callback function** which tells us what **operation will be carried out on each value** of the list, here this is the parameter `instructions`
- **An accumulator variable**, which will keep on accumulating the result obtained on execution of the callback function on each iteration. Here the accumulator is the empty array `output`
- **A reducer operation**, which will specify how the accumulation happens, here in each iteration, the `output value` of the callback function is `pushed` into the accumulator array `output`, so two values become a single value - hence the name reducer
- The Accumulator is returned at the end of all the iterations.

ideally the accumulator variable should also be a function parameter, so a more accurate reducer H.O.F will be as below

```js
const reduce = (array, howToCombine, buildingUp) => {
 for (let i = 0; i < array.length; i++){
 buildingUp = howToCombine(buildingUp, array[i])
 }
 return buildingUp
}
const add = (a,b) => a+b

const reducedValue = reduce([1,2,3],add,0)
```

### 3.1. The `map` function on an array
---
It pushes(accumulator operation) all elements of the input array into a new array (accumulator) one by one, after performing some operation on each of them

The `map` function defined on an array is functionally identical to the `copyArrayAndManipulate` function we have seen previously. 
- Here the accumulator variable is always an empty array, so it is not present in the list of arguments passed to `map`
- The list of values or in other words the array on which we are applying the `map` function get reduced to another array value.
- The only input to `map` is a function, which will be executed on each element of the input array .
- `map` function should be ideally called `mapReduce`
- It is called `map` as the input array is being mapped into another array

```js
let arr = [1,2,3]

let newArr = arr.map(e=>e*2) // [2,4,6]
```

### 3.2. The `reduce` function on an array
---
The differences between the `map` and `reduce` functions are as follows
- For `reduce` we have to provide the `accumulator` value as an argument
- The function argument should have two parameters --> the `accumulator` and the array element
	- The Function body should contain the reduction operation. For `map` it was always the push operation (so it was not necessary in map in the below example the accumulator is being added)
	- The Function should also contain the operation which should take place on each array element (in the below example no operation is being performed on each input array element)

```js
let arr = [1,2,3]

console.log(arr.reduce((acc,ele)=>{return acc+ele},0)) //6
```

### 3.3. The `filter` function on an array
---
It is similar to map in that, it also pushes those elements into a new array, which satisfies a boolean condition.

The `filter` function, reduces the list of values of the input array into another array, depending on whether the callback function passed to `filter` returns `true` or `false` for each input array value.

The callback function (input function) to `filter` must return a `boolean` value after operating on each value of the input array

```js
let arr = [1,2,3,4,5,6]

function greaterThan2(e){
	return e>2
}

console.log(arr.filter(greaterThan2)) // [3,4,5,6]
```

### 3.4. The `find` function on an array
---
The `find` function is used to search for or find objects within the input array containing a specific value of a certain property

```js
let users = [
{id: 1, name: "John"},
{id: 2, name: "Pete"},
{id: 3, name: "Mary"}
];
```

If we have the above users array, it is quite troublesome to figure out whether there is an object in said array with id property value of say 3. 

We have to provide `find` with a callback function in which will return a `boolean` depending on whether the specific property value is found or not. Suppose if we want to check if an object with id of 3 exists or not, the callback function will be

```js
function check(ele){
	return ele.id == 3
}

let user = users.find(check) // {id: 3, name: "Mary"}
```

- If in any iteration the callback returns `true` then the array element corresponding to that iteration in returned by `find`
- If for all the elements of the input array, the callback function returns `false`, then `find` returns `undefined`

### 3.5. The `findIndex` function on an array
---
It is almost the same as the `find` function, the difference being, that when for a particular input array element, the callback function returns `true` , the index of that element is returned not the element itself.

```js
let userIndex = users.findIndex(check) // 2
let nextIndex = users.findIndex((e)=>{e.id==10}) //-1
```

If for all iterations, the callback function returns `false`, then `findIndex` returns -1