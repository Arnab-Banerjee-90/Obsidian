## 1. What is Destructuring
---
Destructuring is `decomposing a structure` into some or all of its individual parts

So we can only destructure, only such data which have structure, such as `arrays` and `objects`

The purpose of destructuring as a feature is to assign individual parts from some larger structure, to individual variables. So we're taking a big thing, and assigning down some or all of its individual parts to some other locations. That's what this feature is for.

## 2. Array Destructuring
---
Arrays are a structure on which we can apply destructuring.

```js
var [
 {
   name: firstName,
   email: firstEmail = "first@email.com"
 },
 {
   name: secondName,
   email: secondEmail = "second@email.com"
 }
] = getRecords()
```

The **left hand side of the equals, its actually a pattern**. It is a syntax that is describing the value that is expected from the right-hand side, which in this case is an array of objects.

And the purpose of that pattern, is not just to describe it. The real purpose for describing it is so that we can assign those individual values off as we need them.

Using the `var` declaration we have actually declared 4 variables `firstName`, `firstEmail`, `secondName` and `secondEmail`

That is essentially saying, go make me a variable called `firstEmail`, that has the value that is in this particular location of the data structure, which is the email property of the first object in an array. If there is no such property in the first object of the array then create the property with the default value of `first@email.com`

So all in all, the left hand side of the assignment serves a dual purpose
- It describes the structure/pattern of value we expect from the right hand side
- It assigns some parts of the structure into the declared variables

### 2.1. Refactoring code - imperative to declarative
---
Let us have the following code

```js
function data(){
 return [1,2,3]
}

var tmp = data();
var first = tmp[0];
var second = tmp[1];
var third = tmp[2]

// Using destructuring we can refator the code as

var [
 first,
 second,
 third,
] = tmp = data()

```

One thing to note is that, the `[ ]` in the left hand side of the `=`  is the pattern which points towards the entire returned array from the right hand side. so we can also write something like this

```js
var tmp = [
 first,
 second,
 third,
] = data()
```

### 2.2. Providing a default value to L.H.S variable
---
If in some cases a variable present in the destructuring pattern in the left hand side is assigned `undefined` , we can provide a default value for it as well.

```js
function data(){
 return [1,2,3,4]
}

var tmp = [
 first,
 second,
 third,
 fourth,
 fifth = "No data Present" // there is no fifth element so fifth is assigned undefined, in place of that the default value which we provide is used
] = data()
```

### 2.3 Gathering up remaining values
---
If we don not know beforehand how many values will be present in the right hand side of the array, we can use the gather syntax  `...` to gather up the remaining values of the array as another array into a single variable 

```js
function data(){
 return [1,2,3,4]
}

var tmp = [
 first,
 second,
 ...third // third --> [3,4]
] = data()
```

The first two values of the array will be assigned to `first` and `second` respectively, and if there are any more values in the array those will be assigned to the `third` variable as an array. If there are only two values in the array to begin with then the `third` variable will be assigned an empty array `[ ]`

The gather syntax, has to show up at the end of the pattern. You can't gather in the middle and then have other things listed after it, it has to be at the very end of the pattern.

### 2.4. Pattern Declaration and variable declaration
---
The variables we are using in the destructuring pattern, need no be declared there itself, like any other variable declaration, we can declare variable at one place and then use it at another place, it is the same when it comes to the destructuring pattern.

We can use previously declared variables in our destructuring pattern

```js
function data(){
 return [1,2,3,4]
}
var tmp,first,second,third;

tmp = [
 first,
 second,
 ...third // third --> [3,4]
] = data()
```

### 2.5. Comma Separation to skip R.H.S  values
---
What if we did not care about the second value of the right hand side array, what we could do in that case is just put a comma `,` in place of the variable used to capture the said value.

```js
function data(){
 return [1,2,3,4]
}

var tmp = [
 first, // first --> 1
 ,
 ...third // third --> [2,3,4]
] = data()
```

We can skip over as many values of the right hand side array as we want using this `,` syntax.

### 2.6. Provide empty array `[ ]` in R.H.S as fallback
---
If for some reason the right hand side does come back as some `falsey` value such as `null` or undefined, then destructuring will not work, as we have said before destructuring can only be performed on structured data (arrays, objects, iterable).

We can avoid this by using a fallback such as 

```js
function data(){
 return undefined
}

var tmp = [
 first, // first --> 1
 ,
 ...third // third --> [2,3,4]
] = data() || []
```

Now if `data()` is falsey then the `[]` will be used instead.

### 2.7. Arrays as function parameter
---
When we are expecting a function parameter to be an array then we can use destructuring on that function parameter as well

Also when this function is called , if it is called using some primitive value such as `number`, `null,` `undefined` etc. we can use a `default parameter` of an `[ ]` as a fallback

```js
function data([
 first,
 second,
 ...third
] = []){
 console.log(first,second,third)
}
data([1,2,3]) // 1,2,[3]
```

### 2.8. Swapping variables
---
It is an interesting use case of array destructuring. 
- We first declare and assign values to the two variables
- construct an array out of them
- place the destructuring pattern in the L.H.S with the variables reversed
- place the array in the R.H.S

```js
var x = 10;
var y = 20;

[y,x] = [x,y]

console.log(x,y) // 20,10
```

