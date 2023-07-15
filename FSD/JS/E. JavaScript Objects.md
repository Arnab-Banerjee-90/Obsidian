
## 1. What are JS Objects
---
We know that there are eight data types in JavaScript. Seven of them are called “primitive”, because their values contain only a single thing (be it a string or a number or whatever).

`Objects` are unique in the sense that they `can have properties`, properties are nothing more than `key value pairs`, where `key` is the `identifier` of the property represented as a `string` and the `value` is the property's corresponding value which can be `any JS type.

Properties and keys are therefore, somewhat synonymous.

### 1.1. Objects are indexed by its keys
----
Objects are indexed, which means that, each value present in an object has a specified location in memory, which is the key of the value.

Accessing a value, using its key is instant -> Like finding a person given his/her address

whereas, if we want to find the key corresponding to a value (called searching) takes time -> Like finding a person's address.

### 1.2. The keys of objects are unordered
---
The keys of an object are unordered, so a key can be anywhere within the object,

If we consider an object as a jar full of wrapped cookies, then the wrapper is the key and the cookie is the value. In such an arrangement, there is no particular order in which the wrappers are present within the jar

### 1.3. Objects are miniature JS memory
---
We know that, JS in its memory (be it local or global), stores identifiers as key value pairs if we have identifiers in code like `let i = 20` or `function foo(){}`
Those gets stored in JS memory like
```JS
i:20,
foo:[Function foo]
```

Objects are like miniature JS memory, hence inside objects `var` `let` or `const` declarations are not permitted.

To store functions as value we can do the below
- We can either use an anonymous function expression, if we do so, the word `function` will become the key and it will refer to the anonymous function
- Or we can `omit` the `function` keyword in a named function declaration, then the key will be the function name and it will refer to the function
- As arrow functions are anonymous and do not require the `function` keyword, in order for a key to refer to an arrow function we have to mention the key specifically

```js
var obj1 = {
	function (){
		console.log("hello")
	}
}
console.log(obj1) // { function: [Function: function] }

var obj2 = {
	named(){
		console.log("I am named")
	}
}
console.log(obj2) // { named: [Function: named] }

var obj3 = {
	fun:()=>{
		console.log("fun")
	}
}
console.log(obj3) // { fun: [Function: fun] }
```

## 2. Creating an Object
---

### 2.1. Using the `Object` Constructor function
---
we can use the `Object` function together with the `new` keyword to construct an object

```js
let obj = new Object()
```

### 2.2 Using Object literal
---
This is the more common way to creating objects.

```js
let obj2 = {}
```

Going forward we will mostly use the object literal to create new objects

## 3. Keys of an object
---
Let us assume we have the below object

```js
let obj3 = {
 "name" : "Arnab",
  "age" : 90,
   age : 80
}
```

Here we see that the above object has three key value pairs two are with quotes, and the other without. There are subtle differences we need to keep in mind while naming the keys of an object.

`JS saves the names of keys of an object as strings`, even though the keys `"age"` and `age` might seem different, in actuality they are the same key, and if we log the object we will have

```js
console.log(obj3) //{ name: 'Arnab', age: 80 }
```

In an object the `keys are distinct`, so the second assignment of the key `age` overrides the previous one.

### 3.1. Naming keys
---

#### 3.1.1. Without using the `""` quotes
---
If we are naming keys of an object without the quotes, then we need to keep in mind the below rules

- Unquoted keys can be given the name of any JS reserved/keyword
- All `unquoted key` names must `conform` to the `variable naming rules` of JS
- After the object is saved, all `keys` of an object are `converted to string`

```js
let obj4 = {
 for: "this is valid",
 9lives: "not valid" //9lives: "not valid"
}
```

#### 3.1.2. Naming keys using `""` quotes
---
If we are naming keys as a string literal then any `valid string literal` will do

```js
let obj4 = {
 for: "this is valid",
 "9lives": "now valid"
}
```

#### 3.1.3. Naming keys using `[ ]`
---
While creating objects using the object literal, we can use the `[]` notation for naming keys of the object

If JS expressions or identifiers are put inside `[]`, then they will be evaluated first, converted into a string, then used as the key name.

```js
var a = "An apple";

let obj = {
 [a]: "not really" // [a] is evaluated to "apple"
} 

console.log(obj) // { 'An apple': 'not really' }
```

### 3.2. Accessing the value of a key
---

#### 3.2.1. Using the dot notation
---
we can use the dot notation to access properties of an object

if we say `obj.key` then we are saying that it should look for in the object `obj` the property named `"key"`

```js
let obj5 = {
 name: "Arnab",
 age: 90,
 "9lives": false,
}
```

`obj5.name`  gives the value `"Arnab"`

#### 3.2.2. Limitation of dot notation
---
But there is a limitation while using the dot notation for accessing object properties. As we have seen above dot notation can only look for the exact key name in the object as we have used in the dot notation itself.

Using dot notation we can `only access such properties` on objects, the keys of which `conform to the variable naming rules`. In other words the key has to be a valid JS identifier.

for the above object we cannot access the `9lives` key using dot notation as it is not a valid JS identifier.

```js
obj5.9lives // Syntax Error
```

#### 3.2.3. Using the `[ ]` notation
---
If `[]` notation is used, we can use it in the below two ways

##### 3.2.3.1. using a literal value within the `[ ]`
---
If we use a literal value within the `[ ]` then that value will be converted to a string , and then that key will be searched in the object

```js
let obj6 = {
 1:"one"
}

console.log(obj6[1]) // converted to "1" --> outputs "one"
```

##### 3.2.3.2. using an identifier or expression in `[ ]` notation
---
We can use a JS identifier or expression, inside `[ ]` . If we do so, then that identifier or expression will first be evaluated into a value, that value will be converted into a string and then that key will  be searched within the object.

```js
let obj = {
 undefined: "not really"
} 

var a;

console.log(obj[a]) // "not really"
```

- The identifier a is evaluated to `undefined`
- It is then converted into the string `"undefined"`
- the object `obj` contains a key `"undefined"`, (all keys are saved as strings) so the value corresponding to this key is printed.

### 3.3. Adding new properties
---
Assigning new keys or properties is very much like accessing key values, only difference is here we will be using either the dot or `[]` notation to assign values to new keys

```js
let obj = {}
obj.name = "Arnab" // name key added with value "Arnab"
obj["age"] = 90   // age key added with value 30
```

### 3.4 Removing properties, the `delete` operator
---
We can delete any existing value (along with its key) within an object using the `delete` operator, we just have to use the notation for accessing key values and put a `delete` in front of it

the `detele` operation returns a `boolean` based on whether the deletion was successful

```js
console.log(obj) // { name: 'Arnab', age: 90 }

console.log(delete obj.age) // true

console.log(obj) // { name: 'Arnab' }
```


## 4. Property Values
---
Property `value` of an object, can be any `variable/expression/literal value`

### 4.1. Property value shorthand
---
In real code, we often use existing variables as values for property names.

```javascript
function makeUser(name, age) {
  return {
    name: name,
    age: age,
    // ...other properties
  };
}

let user = makeUser("John", 30);
```

In the example above, `properties have the same names as variables`. The use-case of making a property from a variable is so common, that there’s a special _property value `shorthand` to make it shorter.

```javascript
function makeUser(name, age) {
  return {
    name, // same as name: name
    age,  // same as age: age
    // ...
  };
}
```

So here the properties are the variables `name` and `age` , the values are the values of these variables respectively

## 5. Checking for a property
---
A notable feature of objects in JavaScript, compared to many other languages, is that it’s possible to access any property. There will be `no error` if the `property doesn’t exist!`

Reading a `non-existing` property just returns `undefined`

So we can check whether or not a `property` exists in an object, by just `comparing` with the `undefined` value right ?

It works most of the time, but what if the property itself holds the value `undefined`

```js
let obj3 = {
	prop1 : undefined,
}

console.log(obj3.prop1 == undefined) // true ?? but prop1 exists!!
```

To avoid this gotcha there is the `in` operator

### 5.1 The `in` operator
---
The syntax is 

```javascript
"key" in object // returns a boolean
```

for example 

```javascript
let user = { name: "John", age: 30 };

console.log( "age" in user ); // true, user.age exists
console.log( "blabla" in user ); // false, user.blabla doesn't exist
```

### 5.2. The `hasOwnProperty` method
---
We can use the `hasOwnProperty` method on any object, and pass the `property string` as an argument, and it will return a boolean depending on whether the said property is present or not

```js
let user = { name: "John", age: 30 };

console.log(user.hasOwnProperty("name")) // true
```


## 6. The `for..in` loop
---

To walk over all keys of an object, there exists a special form of loop --> `for..in`

```javascript
for (let key in object) {
  // executes the body for each key among object properties
}
```

Note that all `“for”` constructs allow us to declare the looping variable inside the loop, like `let key` here

The below code is used to iterate over all the keys in an object, and print the keys as well as its values

```javascript
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for (let key in user) {
  // keys
  console.log( key );  // name, age, isAdmin
  // values for the keys
  console.log( user[key] ); // John, 30, true
}
```

### 6.1. Ordering of keys in an object
---
We know that keys of an object are not ordered, but if we loop over an object, do we get all properties in the same order they were added? Can we rely on this?

Keys in objects are ordered in a special fashion
- `Keys` which can be converted to in `integer`, they are `sorted`
- Order of all other keys are in the same order as they are created

```js
let codes = {
 Japan: 81,
 "49": "Germany",
 "41": "Switzerland",
 "44": "Great Britain",
 India: 91,
 "1": "USA"
};

for (let code in codes) {
 console.log(code);
}
```

In the above code, the Integer coercible keys will come first in a sorted order
So -> 1,41,44,49
Then the keys "Japan" and "India" will be displayed in the order they are created,

```js
1
41
44
49
Japan
India
```


## 7. Object reference and copying
---
