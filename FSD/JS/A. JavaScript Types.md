
## 1. Primitive Types
---
In JavaScript variables don't have types, values do.

Most values in JavaScript can behave as objects, but in JS not everything is an object.

An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are 

**Boolean, Bigint, Object, Number, Null, Undefined, String, Symbol**. --> (B)O(N)U(S)

Out of these all are primitive types except **Object**.

An ECMAScript language value  is a value that is characterized by an ECMAScript language type.

### 1.1. The Boolean Primitive
---
This primitive type has only two values `true` and `false`

### 1.2. The Bigint Primitive
---

bigint is yet another primitive type in JS, it is different than regular number types as it can grow up to the system memory.

```js
var v = 42n;
// var v = BigInt(42)
```

### 1.3. The Number Primitive
---
This is the type given to all the JavaScript numbers, be it integer or floating point

### 1.4. The Null Primitive
---
The null type has only one value and that is the `null` value. The null type has some quirky behaviour, we will se later.

### 1.5. The Undefined Primitive
---
This type has one and only value and that is `undefined` . Any variable declared with keyword `var` , but not assigned any value, has the value undefined

undefined means that the primitive data type has currently no value associated with it

```js
var e;
console.log(e) //undefined
```

### 1.6 The String Primitive
---
The string literal in JS is of the string type, it can be either a single or double quoted string or a string template literal constructed using back-ticks

```js
var s1 = 'A single quoted string'
var s2 = "A double qupted string"
var s3 = `A template literal string ${a} any thing inside {} will be evaluated`
```


## 2. `typeof` Operator
---
`typeof` operator takes in a value or variable, and **returns a string**, indicating the type of the value of the operand,

`typeof` guarantees that it will return a string, which essentially is a short `enum` list

```js
var v;
typeof v; //"undefined"
v = "1";
typeof v; //"string"
v = 2;
typeof v; //"number'
v = true;
typeof v; //"boolean"
v = {};
typeof v; //"object"
v = Symbol();
typeof v; //"symbol"
v = 47n;
typeof v; //"bigint"
```

### 2.1 `typeof` null
---
`typeof` operator when applied to a `null` value, due to some historical reasons returns **“object”**. It may be due to the fact that in ES1 days null was used to unset an object reference.

```js
var v = null;
typeof v //"object"
```

### 2.2. `typeof` an array value
---
`typeof` operator when applied to an `array` value, returns **“object”**

```js
var v = [1,2,3];
typeof v //"object"
```

So how can we know for sure that the above v is an array and not a generic object type, for that there is the helper function `Array.isArray()`. 
`Array.isArray()` returns true only when the argument passed is an array, and false otherwise

```js
Array.isArray([1, 2, 3]);  // true
Array.isArray({foo: 123}); // false
Array.isArray('foobar');   // false
Array.isArray(undefined);  // false
```

### 2.3. `typeof` a function
---
`typeof` operator when applied to a function, returns **“function”**

```js
typeof(function (){}); //"function"
```

### 2.4. `typeof` an undeclared value
---
`typeof` operator when applied to an undeclared value returns **“undefined”**

```js
typeof undeclaredValue //"undefined"
```

the typeof is the only operator in JS which can operate on a undeclared variable and not throw an error.

## 3. Kinds of Emptiness
---
undefined and undeclared are NOT the same thing in programming.

### 3.1. undefined
---
`undefined` is the value given to a variable that <u>has been created/declared but does not contain a value</u>

### 3.2. undeclared
---
`undeclared` refers to not being created, or we can say something which <u>cannot be referenced by JS as it has not been declared yet.</u>

In the `typeof` section above, we see that, `typeof` returns “undefined” when it operates on an undeclared variable. This should not be the case, it should have returned “undeclared”, this is again a historical quirkiness of JS.

### 3.2. uninitialized (aka TDZ)
---
A `let` or `const` variable (i.e. block scoped variable) is said to be in a "temporal dead zone" (TDZ) from the start of the block until code execution reaches the line where the variable is declared and initialized.

```js
{ // TDZ starts at beginning of scope
  console.log(bar); // undefined
  console.log(foo); // ReferenceError
  var bar = 1;
  let foo = 2; // End of TDZ (for foo)
}
```

The term "temporal" is used because the zone depends on the order of execution (time)

## 4. `NaN` , `isNaN()`, & `Number.isNaN()`
---
`NaN` is supposedly an acronym for “Not a Number”. It comes in JS from the IEE 754 spec which is a set of technical standards for how number are represented.

But a more correct approach would be to say that `NaN` represents an **invalid number**.

### 4.1 Some Features of `NaN`
---
- Whenever through coercion, or through a mathematical operation we arrive at an invalid number, it is represented in JS as a special value called `NaN`.

- `NaN`s are not equal to each other, this is as per IEEE 754 as it says that `invalid numbers are not equal to each other`, NaN is the only value in JS which do not have the identity property.

	```js
var myAge = 40; 
var myCatsAge = Number("n/a") //NaN

myCatsAge === myCatsAge // false Oops!!!

```

- The `typeof(NaN)` is of course "number", as NaN is a part of the IEEE 754 standard

### 4.2. The `isNaN()` utility
---
The `isNaN()` is a function which first `coerces` the argument passed to it to a number, and then `checks` whether it is the NaN value

```js
isNaN(30) //false
isNaN("80") //false
isNaN("my name") //true (as the string coerces to NaN value)
```

### 4.3. The `Number.isNaN()` utility
---
The `Number.isNaN()` just `checks` whether the value passed to it is an invalid number or not, it `does not coerce` the argument to a number

So it essentially is a `===` comparison with the `argument` and the `NaN` value

```js
Number.isNaN("80") //false
Number.isNaN("my name") //false
```


## 5. The negative zero `-0`
---
IEEE 754 specs requires for there to be a negative zero, it is essentially the 0 value with the sign bit on.

JS goes to great lengths to hide from us the fact that -0 exists in the language, In almost all the cases it is very hard to tell 0 apart from -0 in JS.

```js
// In all the below checks -0 is indistinguishable from 0

var rate = -0;

rate.toString() // "0"

rate === -0; //true
rate === 0   //true ???

rate > 0;  //false
rate < 0;  //false 

```

### 5.1. Using `Math.sign()` to check for `-0`
---
Math.sign( ) returns -1 if the number passed as argument has the sign bit on (if the number is negative), and it returns 1 for a positive number

But Math.sign( ) behaves in an awkward way when 0 or -0 is the argument to it, it just returns the argument 

```js
Math.sign(3)   //1
Math.sign(-3)  //-1
Math.sign(0)   //0 ???
Math.sign(-0)  //-0 ???
```

### 5.2. Using `Object.is()` to check for `-0`
---
Object.is( ) is able to differentiate between -0 and 0 (its like a quadruple equals 😄)

```js
var rate = -0

Object.is(rate,-0) //true
Object.is(rate,0)  //false
```

### 5.3. Using the `-Infinity` value to check for `-0`
---
A more simple way to check for `-0` is to `divide` any positive number by `-0` and check for `equality` with `-Infinity` value

```js
(1/-0) == -Infinity //true
(1/0) == -Infinity //false (it will be Infinity)
```


## 6. Fundamental Objects
---
These were previously called as Built-in objects or Native functions. These are responsible for the bolted-on object orientedness of JavaScript. The fundamental objects in JS are as follow (REDOFA)

- RegExp
- Error
- Date
- Object
- Function
- Array

### 6.1. Constructing fundamental objects using `new`
---
The `new` keyword along with the suitable `constructor` function should be used to create fundamental objects.

```js
new Date(); // compulsory to create Date object, no Date literal in JS
new Error();
new Array();
new RegExp();

new Object();
new Function(); //?? Debugger error
```

Apart from `Date` all other objects can be created using a declarative literal 

### 6.2. Primitives should not be constructed using `new`
---
The `String()`, `Number()` and `Boolean()` should only be used as functions which coerces the argument passed to them as string, number and boolean respectively.

If these functions are used to create primitives, they actually create objects, we have to further coerce them using the same function to get the primitive

```js
let b = new Number(80)
console.log(typeof b) // "object"
let c = Number(b)
console.log(typeof c) // "number"
```

