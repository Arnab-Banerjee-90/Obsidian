
## 1. Processing of JavaScript code before execution
---
In JavaScript we may not run a compiling step, **but before the code is executed some processing/parsing occurs; memory is allocated to all the variables and functions present in this step**. 

We may think well, JavaScript can't be processed because we don't run a compiler on our development machine and then run the compiled code right?

So we think about this difference between code being processed or not as the distribution model for the binary. But that's not really the right axis to be thinking about. 

The right axis is, **is the code processed before it is executed or not?** We do have languages that exist which generally don't get processed before execution. For example, Bash script.

Running JavaScript code is a two pass system
- The **Processing - Parsing and Memory Allocation** phase
- The **Execution** phase

The Scopes are created during this processing phase.
- Scopes are created for global scope and functions (and blocks)
- The function parameters, and the function and variable identifiers live in said scopes

## 2. Scoping Phase - Parsing and memory allocation
---
In this phase only those lines are considered where there is 
- A variable declaration
- A function declaration
- A function expression

1. The Scoping plan is prepared
	- Scopes are created for global as well as all the function scopes
2. Scopes are assigned to the following
	- Declared variable identifiers
	- Function identifiers
	- Function parameters
3. Memory allocation and default value assignment
	- Memory is allocated for all the above identifiers and parameters.
	
### 2.1. `var` declared variables
---
- For variables declared with var, they are assigned the value `undefined` 
- They are assigned the function scope in which they are declared.

### 2.2. Function Declarations
---
- The function identifiers refers to the function from compilation phase itself
- The `function identifiers` are given the `enclosing scope` of the function itself

### 2.3. Function Expressions
---
- Function expressions are any function, which `do not have` the `function` keyword as the starting word of a line of code
- For named `Function Expressions`, function `identifiers` are in their `own function scope`
- The function identifiers refers to the function from compilation phase itself

### 2.4. Function Parameters
---
- For all `function parameters`, their scope is the `function scope` itself

### 2.5. Auto Global
---
If we have an identifier which is assigned some value but there is no `var` declaration, then that identifier will be completely ignored and will not be assigned any scope in the processing stage

`auto globals` occur only in the `non strict mode` and the identifier must be in `target` position (target of some assignment operation)

### 2.6 An example of scoping phase
----

#### 2.6.1. Scope creation and scope allocation of variables
---
Below is an illustrative example of scopes in a piece of code, here colored bars are used to indicate scopes, and matching colored circles are used to indicate identifiers , e.g. red circles belong to the red scope and so on.

The variable and function (declaration and expression) keywords are shown in bold

![[scope1.png]]

Note, that the identifier `topic` inside the blue scope is not a `var` declared variable hence it won't be assigned a scope or memory

#### 2.6.2. Memory allocation and default value allocation
---
After default values and memory is allocated to the above identifiers,  the scopes, the identifiers in said scopes and their default assigned values together can be represented as shown below

![[scopes2.png]]

Also one more thing to note is that, a scope, apart from its own identifiers, has access to all identifiers present in its enclosing scopes. for example
- The green scope has access to identifiers of the blue and red scopes
- The yellow scope has access to identifiers of the red scope only

### 2.7. Block Scopes and `let` and `const` declared variables
---
The Block scope was introduced in `ES6`
In JavaScript, a block is denoted by curly braces { }. The space between the curly brackets is known as a block. For example, the `if...else`, `do...while` and `for` loop statements create blocks.

#### 2.7.1. Conditions for creating block scope
---
- We need a `{ }` which indicates a block.
- We also need at least one `let` or `const` declared variable inside said block, to make it a Block Scope
- `let` or `const` declared variables attaches them with the global or function scope in which they are declared, if they are not declared inside inside a `{ }`

```js
var a = 10
{
	let a = 20
	console.log(a) // 20
}
console.log(a) // 10
```

#### 2.7.2. Processing `let` , `const` declared variables - The `TDZ`
---
- They are assigned as uninitialized at compile time.
- They cannot be used as a source or target until execution reaches the let/const declaration line and is initialized with a value on or after the declaration line.
- This is known as `TDZ error` for let/const declared variables, it was meant for const only but it was incorporated in let declared identifiers as well

```js
var x = 20;
{
	a = 20; // a cannot be used as a source or target until execution reaches the let declaration and a is initialized.
    let a = 10
    console.log(a) 
}

```

#### 2.7.3. `var` vs `let` vs `const`
---
- `var` keyword signals to the reader that this variable is accessible all across the global scope or a certain function scope, so var declarations signals that this variable will be `used throughout a large section of the program`
- `let` declaration signals `highly localized variables`, variables available to be accessed for only a couple of lines of code.
- `In a particular Scope`, once we have declared a variable using `let`, we `can reassign` it, but the variable `cannot be re-declared`
- var and let is supposed to coexists as they serve different functions.
- `var` can be `re-used (declared)` twice inside a scope, and it essentially is the same variable.
- `const` is similar to `let` , but it differs in the fact that `const` declared variables `cannot` be `re-assigned` or `re-declared`

#### 2.7.4. `let` and `for` loops
---
So what happens when we use a let declared variable to initialize a for loop ??

```js
for (let i=1;i<=5;i++){
	console.log(i)
}
```

We know that `let` declared variables cannot be re-declared in the same scope, but it seems that in each iteration `i` is being re-declared

What is happening is, at every iteration when we declare and re-declare the variable i, actually a new scope is being created for the same. so at every iteration of the for loop we are actually creating a new i variable, bound to a new block scope.

## 3. The execution phase
---
This phase begins once the processing phase is done.
At this phase JS knows all the scopes, what identifiers are in and accessible in said scopes. Hence is this phase the below happens
- All the `var` and `function` keywords are **ignored**.
- `operations` and `function calls` are **considered**
- If JS comes across an identifier, JS checks in memory  and corresponding scopes and retrieves the value if it is already there, otherwise JS throws a `ReferenceError`
	- If said identifier is in **source mode** --> Its value is retrieved from memory
	- If said identifier is in **target mode** --> the value assigned to it is stored in memory and the previous value is overwritten
	
### 3.1. An example of the execution phase
---
Let us go through the execution phase of the code, the processing phase of which was discussed in [[#2.6 An example of scoping phase]]

![[scopes3.png]]

- The first line is an assignment operation. The `teacher` identifier is encounter in global scope having value `undefined`, it is assigned the value `"Kyle"`
- Next we encounter assignment of the `newTeacher` identifier which has the value `undefined` in memory, It is assigned the reference to the function `newClass`, assignment happens from right to left, hence there is no scope access issue (yellow scope identifiers have access to the red scope identifiers)
- Next, the function associated with identifier `newTeacher` is executed with argument `"hello"`. So in the yellow scope the value function parameter `q2` is overridden from `undefined` to `"hello"`
- Last Line contains a function invocation, the function referred to by the identifier `otherClass` is invoked
	- Inside the Blue scope, we encounter `teacher` identifier, which is present in `global` scope, the value `"Suzy"` is assigned to it
	- Next we encounter `topic`  it is being assigned a value, this identifier is not present in memory in any scope. If JS is not running in `sctrict mode` then, JS will now allocate memory for this identifier and assign it the global scope. This is called `Auto Global`, and it only happens in `non-strict` mode in `target` position
	- Then we have the function definition of `ask` which is ignored
	- Next the function associated with identifier `ask` is invoked
		- Inside the Green scope, first the `argument` "why" is assigned to the parameter `question`, which was having the default value of `undefined`
		- then we are accessing the values of the identifier `teacher`, its value can be accessed as it resides in the `global` scope which is an enclosing scope, 
		- We are then accessing the value of `question` , which is present in the current scope so its value can be accessed as well

## 4. Order of processing and execution of each function
---
In the previous sections, for the sake of simplicity we have discussed the processing and execution of all JS code in one fell swoop.

In actuality `processing and execution` happens for `each function separately` as and `when they are executed`.

Identifiers in the global scope are processed and the global code is executed by default as we execute the script. But if we declare a function in global scope but never execute it, even if that function contains several variables and other functions within it
only memory will be allocated for the said function in global , but the processing and execution of the contents of said function will never take place.

```js
var a = 10;

function foo(){
	b = 20 // b is auto-global
	function foo1(a){
		console.log(a)
	}
	//foo1(10) //if foo1 isn't executed then the parameter variable a is  not created
}
//foo()
console.log(b) // b is 20 if foo is executed above otherwise ReferenceError
```

## 5. `IIFE`
---
An `IIFE` stands for Immediately Invoked Function Expression. The aim of an `IIFE` is to create a scope where we can write experimental code, and as it is immediately invoked, the identifiers within the `IIFE` does not pollute the global scope

```JS
var teacher = "Kyle";

// The below function expression is immediately invoked
(function anotherTeacher(){
	var teacher = "Suzy";
	console.log(teacher); //Suzy
})();

// The function expression identifier anotherTeacher does not pollute the global scope as function the scope of expression identifier is the IIFE itself 
// in the above line because of the parens after the definition 
console.log(teacher) //Kyle
```

### 5.1. Steps to form an `IIFE`
---
- Wrap the entire function in parenthesis, the motive is to making the opening parenthesis as the first character in the statement, converting the function declaration into a function expression
- next is to immediately invoke (execute) it using another set of parenthesis.
- One thing to note is, that we should use `;` as the statement terminator of the previous statement before beginning our `IIFE`

### 5.2. Benefits of using an `IIFE`
---
- The function expression is invoked on the same line it was defined, so we get the scope which we were looking for to avoid naming collision
- And also after the expression is done executing, the scope disappears as well.
- `IIFEs` can also be used to transform a JavaScript statement into an expression
- We can return functions from within `IIFE`s, which along with the concept of `closures` play a key role in creating what is called the `module` pattern 

## 6. Anonymous function expressions
---
We have already discussed named function expressions in the above sections, we have seen `IIFEs` and functions assigned to variables as named function expression. These function expressions can be anonymous as well.

```js
let func = function (params){
	return params*2
}; // terminator ; is required in this case
(function(data){
	console.log(data)
})(20)
```

The above function expressions are anonymous, they do not have an immediate identifier associated with the function definition

### 6.1 Arrow functions
---
