
## 1. What are Abstract Operations ?
---
Abstract operations are the fundamental building block that makes up how we deal with type conversion. In the JS spec, we have these abstract operations which perform, fundamentally, the task of type conversion. In JS we refer to type conversion as coercion.

By the way, the abstract operations are not a thing the JavaScript engine, they're not like a function that could somehow be called. When we call them abstract, we mean they're a conceptual operation.

So Basically Abstract Operations are algorithms which are defined in the JavaScript engine.

## 2. `ToPrimitive` Abstract Operation
---
So any time we have something that is not a primitive and it needs to become a primitive, conceptually, what we need to do is this set of algorithmic steps, and that's called `ToPrimitive`, as if it were a function that could be invoked.

The algorithm is invoked by JS like a function call, it takes an input argument (which it will try to turn into a primitive) and an optional argument `PreferredType` (which is also called type hint)

`ToPrimitive(input,PrefferedType)`
- If we are doing some numerical operation and `ToPrimitive` is invoked by JS, then type hint will be **number**
- If we are doing any string based operation and `ToPrimitive` is invoked, then type hint will be **string**
- If there is no such preference present then type hint is going to be **default,** it will then try to convert input into any primitive type it can.

### 2.1. `ToPrimitive` actually calls `valueOf()` and `toString()`
---

The way it works is that, there are two methods that can be available on any non-primitive. Any object, function, array, whatever. Any non-primitive, there are these two functions. And you've almost certainly seen these before. The `valueOf()` function and the `toString()` function.

- If the type hint is number, `ToPrimitive` algorithm will invoke `valueOf()` first, and then it invokes `toString()` after that, and if that also fails it is going to throw an error.
- for the type hint string, the order of invocation of the functions in reversed
- As the `valueOf()` function only returns the object itself without any modification, **in every case where `ToPrimitive` abstract operation is invoked by JS, the `toString()` operation is only applied to it.**

## 3. `ToString` Abstract Operation
---
The abstract operation `ToString` converts argument to a value of type String according to the below rules

- Argument `undefined` ⇒ return “undefined”
- Argument `null` ⇒ return “null”
- Argument `boolean` ⇒ return “true” or “false”
- Argument any `string` ⇒ return the argument
- Argument of `Object` type
	- calls `ToPrimitive` abstract operation with type hint of string
	- The result is string as the `toString()` is called on the object ⇒ returns the string
- Argument Number
	- If `NaN` ⇒ returns `“NaN”`
	- if 0 or -0 ⇒ returns “0”
	- For any other number, just the string representation of that number (wrapping the number in quotes)
- Argument `Symbol` ⇒ returns `TypeError` exception

### 3.1. `toString()` function for Arrays
---
If we try to convert an array into a string, then `ToString` abstract operation “invokes” the `ToPrimitive` abstract operation, and then `ToPrimitive` calls the `toString( )` method defined on objects, as we discussed previously.

```js
// Relying on ToPrimitive algo to call toString() 
String([1,2,3]) // '1,2,3'

//Calling toString() directly on the array 
[1,2,3].toString() // '1,2,3'
```

Here is a list to get an idea how `toString( )` function converts arrays to string

```js
String([ ]) // “” (the empty string)

String([null]) // “”

String([undefined]) // “”

String([null, undefined]) // “,”

String([[ ],[ ], [ ]]) // “,,”

String([1,2,3]) //"1,2,3"
```

### 3.2. `toString()` function for Objects
---
If we try to “Stringify” an object, JS again goes through ToString → ToPrimitive → toString( ) route

Stringifying any object results in the `‘[object Object]’` string

## 4. `ToNumber` Abstract Operation
---
The abstract operation `ToNumber` converts argument to a value of type number according to the below rules

- Argument `undefined` ⇒ returns `NaN` value
- Argument `null` ⇒ return 0
- Argument `Boolean` i.e true or false ⇒ returns 1 for true and 0 for false
```js
// An example where the above rule creates confusing and wrong result
// operands of comparison operator > and < are numbers & if not they are coerced to numbers
3 > 2 > 1 // evaluates to false as a result of the above coercion rule
```

- Argument `Symbol` ⇒ throw `TypeError` exception
- Argument of type `string`
	- Argument `“ ”` ⇒ 0 , this is due to the fact that when adding 0 to a number  we get the same number, likewise when we add ' ' to a string, we get back the same string (Root of all coercion evil in JS)
	- Argument ``“ \n\t”`` ⇒ 0 (Any and all white space characters become 0)
	- Argument “0” ⇒ 0
	- Argument “-0” ⇒ -0
	- Any string that cant be converted into a number ⇒ returns `NaN` value
	- Handles string representation of numbers in other bases and converts into decimal number
- Argument of Object type
	- calls `ToPrimitive` abstract operation with type hint of number
	- It calls `valueOf`( ) defined on objects to convert it into a number, but value of returns the object itself !!
	- So it then calls `toString( )` and it converts the object to string as discussed previously, then that string is converted to number using above rules

 ```js
 var c = "0b110"
 console.log(Number(c)) //6
```

Using the above rules we have some weird behavior

```js
Number([]) // "" => 0

Number([null,undefined]) // "," => NaN

Number([null]) // "" => 0

Number([undefined]) // "" => 0

Number([1,2,3]) // "1,2,3" => NaN

Number({a:2}) // "[object Object]" => NaN

Number(" \n\t") // 0 (strips all white spaces)

Number("   123   \n\t   ") // 123 (strips all white space characters)

Number(" 0 0 0 1 2 3              5   ") // NaN (whitespace in between the number characters
```

## 5. `ToBoolean` Abstract Operation
---
The abstract operation `ToBoolean` converts argument to a value of type Boolean according to the below table

- Undefined --> `false`
- Null --> `false`
- Number --> `false` if number is `NaN` , `0` or `-0`  otherwise `true`
- String --> `false` if string is empty `""` , otherwise `true`
- Symbol --> `true`
- Object --> `true`

## 6. `Boxing` (Accessing properties on primitive values)
---
It's a form of implicit coercion. It's not called out in the same way in the abstract operations. But I think it absolutely in spirit is an implicit coercion.

It is saying you have this thing that is not an object and you're trying to use it as if it is an object. Me, JavaScript I'm gonna be helpful and go ahead and make it into an object for you. The only other option would be for the JavaScript to throw an exception that said you're trying to access a property on an primitive value. And it turns out that things can behave as objects, but that doesn't make them an object. This is not an object.

For example a primitive string that has an optimization in it where you can access a property as if it was an object.

## 7. Anomalies of coercion
---
- `ToPrimitive` => `toString()`  on an empty array gives `""`  \[ ] => ""
- `ToNumber` on an empty String gives 0, "" => 0
- `ToBoolean` on an empty String gives `false`, "" => `false`





