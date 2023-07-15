## 1. Creating an Express App
---
Before creating the express app, we first have to initialize the project using
`npm init`

In order to create a back-end application with express JS, first we need the express npm package, we can do so with `npm i express`

Now to create an express application, which represents the entire app, we have to import or require the express package and then  invoke the express method `express()`

```JS
import express from "express"

const app = express()
```

## 2. Create an HTTP Server with Express
---
The `app`  by itself wont do anything, we need to create a server first, which does the following
- Receive incoming request from the client.
- Do some operation (e.g query database) based on the received request
- Respond back to the client with some meaningful data or message
We can create an HTTP server by using the `listen()` method on the `app` 

```js
app.listen(3000,()=>{
 console.log("Listening on Port 3000")
})
```

It returns an HTTP server and starts listening for any incoming http requests to the specified port (here 3000)

## 3. `IncomingMessage` and `ServerResponse` Objects
---
Once the server receives any request, i.e we go to the url `http://localhost:3000/` and press `enter` (it does not matter what comes after the /), [[NodeJS Intro]], instantaneously generates two objects `IncomingMessage` and `ServerResponse` 

These two objects can be passed through several functions all of which can access these two objects, 
Usually `IncomingMessage` and `ServerResponse` are referred to as `request` and `response` objects respectively.
The functions through which `request` and `response` objects pass are called `middleware` functions or `RequestHandler` functions

## 4. Using `app.use()` to create middle-ware functions
---
`app.use('sub-route',f1,f2,...)`  Mounts the specified function or functions, called middle-ware functions at the specified `sub-route` 
The middle-ware function is executed when the  `sub-route` present in `app.use()` is a sub-route of the requested route. 
e.g if the requested route is `/api/user/signin` then all the sub-routes are
- `/api/user/signin`
- `/api/user`
- `/api`
- `/`
So all the above four sub-routes will match for the requested route of `/api/user/signin`

_Middleware_ functions are functions that have access to the [request object](http://expressjs.com/en/4x/api.html#req) (`req`), the [response object](http://expressjs.com/en/4x/api.html#res) (`res`), and the `next` middleware function in the application’s request-response cycle. The next middleware function is commonly denoted by a variable named `next` and invoked from the current executing middleware function using `next()` 

### 4.1 Global middleware functions
---
If no sub-route is specified for `app.use()` then the default sub-route is `/`  which is a sub-route of any route requested by the client, thus such call to `app.use(f1,f2,...)` are triggered always, and the functions inside the argument of such a call are called global middleware functions.

These are functions which gets triggered irrespective of the route requested by the client, once the client initiates the request by hitting send or pressing enter, the `request` and `response` objects go through these global middleware functions in a sequential order (which ever is encountered first).  Each middleware function also has access to a function called `next` which points to the next middleware function in line.

#### 4.1.1 Sequential `app.use()` calls
---
We can have multiple `app.use()` calls one after the other, and they will be executed in the order in which they appear in the code (as shown below)


```js
// This is triggered first
app.use((req,res,next)=>{
  // me first
  next() // go to the next function
})

app.use((req,res,next)=>{
  // then me
  res.send("Hello") // responds back to the client
  next() // go to the next function
})

app.use((req,res,next)=>{
  // then me
  // server hangs...... 
})
```

#### 4.1.2 Several functions as arguments to `app.use()`
---
We can pass as many functions as we want in the argument of a single `app.use()` call, as a matter of fact, we can pass an array of functions, and they will be executed in the order in which they appear in the list of arguments, after all of them are done executing, express will turn to any other `app.use()` calls

```js
function f1(req,res,next){
 // some code
 next()
}
function f2(req,res,next){
 // some code
 next()
}
function f3(req,res,next){
 // some code
 next()
}
app.use(f1,f2,f3)

app.use((req,res,next)=>{
 // then me
 next()
})
```

### 4.2 Each middleware function must call `next()`  for execution to pass to the next function in line
---
Whenever  `next()` appears inside the body of a middleware function, execution passes to the next middleware function in the sequence. If `next` is not called, then the current function becomes the last middleware function.

### 4.3 Only one middleware function must send response back to client
---
Even if we have a number of middleware functions lined up one after the other, only one of them can ever send response back to the client, and any one middleware function in the chain must do so, otherwise the client will not receive a response from the server, and it will hang as the request-response cycle remains incomplete
- `res.send()`
- `res.json({})`
- `res.status(status_code).json({})`
Any of the above methods on the response object can be used to send response back to the client, if multiple middleware functions attempts to call the above methods, then express will throw an error.

A middleware function can call `next` after sending response to the client, but any subsequent middleware functions then cannot send response, they can only call the next middleware function (if there is any) by invoking `next()` 

### 4.4 Route Specific Middleware functions
---
Such middleware functions are executed if the path present in `app.use()` is a sub-route of the route requested by the client
`app.use('sub-route',[array of middleware functions])` 

```js
function f1(req,res,next){
 console.log("f1")
 next()
}

function f2(req,res,next){
 console.log("f2")
 res.status(200).json({message:"done"})
 next()
}

// Requested route is /api/user/signin

app.use("/api",f2)

app.use("/api/user/signin",f1)
```

In the above snippet, if the requested route is `/api/user/signin` , then both the sub-routes present in both `app.use()` functions match with the requested route.
First `f2` will be executed as it comes first in sequence, and as it calls `next()` hence the following `f1` will be then executed, but `f1` cannot send back any data as response as `f2` has already done so.

## 5. Basic Routing using `app.METHOD()`
---
Routing refers to determining how an application responds to a client request to a particular endpoint or route and a specific HTTP request method (GET, POST, and so on).

### 5.1  Responding to various HTTP method and route combinations using `app.METHOD(ROUTE, HANDLER)`
---
There are several methods defined on the express app instance (app), which can be used to `respond to various HTTP Route and Method combinations`

- `app.get('route',f1,f2..)` --> Responds to `get` requests on the `'route'`
- `app.post('route',f1,f2..)` --> Responds to `post` requests on the `'route'` 
- `app.put('route',f1,f2..)` --> Responds to `put` requests on the `'route'` 
- `app.delete('route',f1,f2..)` --> Responds to `delete` requests on the `'route'` 

Unlike `app.use()` the routes of the above methods have to strictly match whatever route is requested along with the http request type sent by the client in order to trigger. Also there can be as many handler functions as we require in the argument of the above functions, these so called `handler` functions are very much identical to the `middleware` functions we discussed for `app.use()` , as a matter of fact only the last function present in the argument of the `app.METHOD()` function is called the `handler` functions, all functions preceding it are called `middleware` functions. Typically there is nothing after the `handler` function so, the `handler` usually has no need of `next` and does not invoke it (Although there is an exception to this)

### 5.2 Flow of Routes
---
Below is a diagram demonstrating the flow of `request` , `response` objects through middleware function calls via `app.use()` and `app.METHOD()` 

- `app.use()` and `app.METHOD()` can come in any order
- Only `one` middleware function out of all the executed ones, can ever send a `response` back to the client

 ![[req_res_flow.jpg]]

## 6. Accepting Client Input
---
Apart from specifying the HTTP method and route, the client can send us data along with the request, those inputs are attached to the `request` object, and we can extract the same in any middleware function and act upon it as we see fit.

### 6.1 Input via `request` body
---
The Client can send request via what is known as the request body. But express cannot by itself read such input, the input needs to be parsed properly and attached to the `body` property of the `request` object.

#### 6.1.1. using `express.json()` to parse body input
---
`express.json()` returns a function, which can parse json passed in the body of the  client request, and attach it to the `body` property of the `request` object.

```js
// Json passed in the body of the request sent by client
{
	"name" : "Time"
}

//----------------------App.js-----------------

app.use(express.json())

app.use((req,res,next)=>{
  console.log(req.body)
})

```

`express.json()` is used as the first middleware within a global `app.use()` call, `express.json()` calls `next()` internally so we don't need to call it, and thus all subsequent middleware functions have access to the client json input inside `request.body`

#### 6.1.2 using `express.urlencoded({extended:true})` to parse url-encoded body input
---
`express.urlencoded({extended:true})` parses url-encoded inputs passed to the body of the request by the client, it cannot parse json.
The inputs are parsed into an object, and then put inside the `body` property of the `request` object, hence this too should be used as a middleware inside of `app.use()`

#### 6.1.3 Data to be sent via body either as json or as url-encoded
---
If the client sends data in request body via both json and url-encoded, then, whichever of the middleware `express.json()` or `express.urlencoded({extended:true})` comes last will overwrite the body property set by the previous one. 

### 6.2 Input via `url params`
---
Client can sent input data via the requested route or url, it is done by passing any value after the end of the route
`localhost:3000/api/users/9/6`
Here 9 and 6 are the url parameters. 

#### 6.2.1 `url params` to be caught by the `url` of the `app.METHOD('')`
---
`url params` are to be caught by the `app.METHOD()` containing the matching HTTP method and route combination

e.g if `localhost:3000/api/users/9/6` is a `GET` request , the `url params` are 9 and 6. Then to capture the `url params` we should have a call to `app.METHOD()` such as
`app.get('/api/users/:id1/:id2',m1,m2,h)` , the `:` represents that `id1` and `id2` are variables which will be used to capture incoming `url params` . The captured `url params` are accessible via the `params` property of the `request ` object

### 6.3 Input via `query params`
---
`query param` input is also attached at the end of the requested route by a `?`
e.g let us have a request url such as 
`localhost:3001/api/user/signin/90/?name=busby&age=900` and the HTTP method is `GET` 
If we have the `app.get()` like below
`app.get('/api/user/signin/:id1',m1,m2,h)` , then we already know that the variable `id` will capture the value `90` in the `params` property of the `request` object , but also whatever in the url that comes after `?` is considered as a `query string`  and each property value pair of the query string separated by `&` is stored inside the `query` property of `request` object. Here `req.query` will contain `{name:"busby", age:"900"}` 

### 6.4 Input via `request.headers`
---
Sometime the client might send sensitive information through the `headers` of the `request` object, one such example is bearer token, which is used for authorization

Bearer token is passed via `request.headers.authorization`. It is a string which looks like this
`Bearer ef9y......er80`
It is a String but we only need to slice out the Bearer part, we can do something like this
`let bearer = request.headers.authorization.split(" ")[1]`

Tokens can also be passed using `x-access-token` property of the `request.headers` object
`let token= req.headers['x-access-token'];`

## 7. Using Express Router
---
So far we have seen that in order to respond to client requests having a specific route and HTTP method we have used specific `app.METHOD()` calls.

### 7.1 Create the `Routes` folder
---
_Each and every route and HTTP method combination we allow the client to send the request to, is known as an API endpoint_, and we should have n number of `app.METHOD()` functions to deal with those n number of API endpoints. So in order to not clutter our main JS file which starts the server, we are going to create a `routes` folder and create all our `routes` in a file within this folder

### 7.2 Create the instance of the express router
---
```js
// Inside routes.js
import {Router} from "express"

const router = Router()

```

the identifier `router` is an instance of the express router

### 7.3 Define all the HTTP method route combinations on the created instance
---
```js
// continuing from previous snippet
router.get("/product/:id", (req, res) => {//route handler});

router.post("/product", (req, res) => {//route handler});

router.put("/product/:id", (req, res) => {//router handler});

router.delete("/product/:id", (req, res) => {//route handler});
```

### 7.4 Mount the express router instance as a middleware via `app.use()`
---
In the main JS file, we will mount the express router instance which contains all our routes, to the main `app` as a middleware via `app.use()` but in order to do so we will need to first export the router from its file 
```js
// continuing from above snippet
export default router
```

Now we have to import the same and use it
```js
// In app.js
import router from "./routes/routes.js"

app.use("/api/v1",router)
```

so any route containing `/api/v1` will trigger the router function (express router instance) and as a consequence any one of the routes defined within `routes.js` will be triggered if the route and HTTP method matches

e.g a route `/api/v1/product/1` with a HTTP `get` method will go via the above `app.use()` and trigger the first route in the `routes.js` file

## 8. Handling errors in Handler/middleware functions
---
Express provides us with an error handler, which we can use in order to let express handle the errors, which may occur in our handler or middleware functions.

Just like we have hooked up global middleware functions, express routers via `app.use` , this error handler function also needs to be hooked up like that only

But the most important thing is, that this handler function must be hooked up after all the routers have been hooked up already

### 8.1. Handling Synchronous Errors
---
Any synchronous error occurring in the handler/middleware function  is automatically sent to our error handler function, present at the bottom of the router stack

We have tried to illustrate the error handler in action, assume that the `userRouter`and `productRouter` have been imported from their respective files.

```js
// app.js
//--code before
app.use('/api/user'userRouter)
app.use('/api/product'productRouter)

app.use((err,req,res,next)=>{
	console.log(err.message)
	res.status(500).json({message:oops!!!})
})
```

If there is any synchronous error arising from any of the handlers present in `userRouter` or `productRouter`, this error handler function will run automatically, and we can send some response to the client

### 8.2. Handling asynchronous errors
---
Here is the exception of the fact that handler functions do not call next. Handler functions can call next, but only to handle asynchronous error.

Express cannot catch asynchronous errors automatically (obvious). So for any potential error prone handler function, we have to catch the error and within the catch block call next with the error as argument.

```js
// in some handler function
function handler(req,res,next){
	setTimeout(()=>{
		// the entire expression is being sent, so that
		// next(new Error("Async error")) can be triggered
		// After the call stack is empty
		next(new Error("Async error"))
	},1000)
}

// app.js
app.use('/api/user'userRouter)
app.use('/api/product'productRouter)

app.use((err,req,res,next)=>{
	console.log(err.message)
	res.status(500).json({message:oops!!!})
})

```

The above example contained an asynchronous callback function, but most of the asynchronous functions we are going to deal with returns Promises, so we can easily deal with them using the `async await` structure

```js
// A controller file
const bcrypt = require("bcrypt")

let handler = async(req,res,next)=>{
  try{
    let hashedPass = await bcrypt.hash(req.body.password, 4)
    console.log(hashedPass)
    res.status(200).json({message:"ok"})
  }catch(err){
    console.log("From handler controller")
    next(err)
  }
}

module.exports = handler

// app.js
app.use((err,req,res,next)=>{
	console.log(err.message)
	res.status(500).json({message:oops!!!})
})
```

Also one thing to be noted is that `synchronous errors are not caught automatically when it gets thrown inside async functions`, so whenever we are using an `async` function the `try catch` block becomes a must to catch all errors.