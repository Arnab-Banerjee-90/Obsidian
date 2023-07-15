
## 1. Mongoose Schema
---
[Mongoose v6.8.0: Schemas](https://mongoosejs.com/docs/guide.html#definition)

### 1.1. Defining a Mongoose Schema  — Invoking the Schema constructor
---
Everything in Mongoose starts with a Schema. Each schema maps to a MongoDB collection and defines the structure of the documents within that collection.

`Schema` is a class within the mongoose library, 

We first need to import or require it. 

```js
// Using CommonJS module syntax
const mongoose = require('mongoose')
// Destructuring only the required Schema Class
const {Schema} = require('mongoose')

```

Create a “new instance” of the Schema class which will be a Schema Object , we do it by invoking the constructor using the new keyword

```js
const blogSchema = new Schema()
```

If we call the `Schema` constructor as shown above then By default, Mongoose adds an `_id` property to your schemas.

The argument to be passed to the `Schema` constructor can be written as shown below, which can contain multiple object literals, the first of which is called the `Document Structure Object` , and the others are called the `options` objects. 

Using these objects we can specify the Stucture of the documents of the collection mapped the the `Schema` in question.

```js
const blogSchema = new Schema(/*{document structure}*/,/*{options..}*/)
```


### 1.2. The `Document Structure` argument of the `Schema` constructor
---
The first argument of the `Schema` constructor is what I call the `Document Structure` . This is an object literal where we define the key value pairs which defines the structure of each document of the collection mapped to the schema in question.

```js
import mongoose from 'mongoose';
const { Schema } = mongoose;

const blogSchema = new Schema({
  title: String, // String is shorthand for {type: String}
  author: String,
  body: String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs: Number
  }
});
```

Each key in this object literal defines a property in the yet to be created documents. 

#### 1.2.1. `SchemaType` - Configuration object of each property
---
<mark style="background-color:gray"> The value for any field or key in the Document Structure object is called the <strong>SchemaType Object</strong></mark>

The value can be either an object literal or an array literal

A `SchemaType` is a configuration object for an individual property.

For example the `title` field has the value `String` , this is actually a shorthand for the object `{type:String}` , this object is a `SchemaType` object

<mark style="background-color:gray">The key and its value (SchemaType object) together forms what is called a <b>Schema Path</b> or simply called Path.</mark>

A `SchemaType` says what type a given path should have, whether it has any getters/setters, and what values are valid for that path. 

Even though `SchemaType` is a Mongoose class, its constructor is never invoked with the new keyword.

##### 1.2.1.1. The `type` property of a `SchemaType` object
---
`type` is a special property of a SchemaType Object. When Mongoose finds a nested property named `type` in our schema, Mongoose assumes that it needs to define a SchemaType with the given type.

#####  1.2.1.2 The Shorthand Notation for a `SchemaType Object`
---
If a `SchemaType` object has only the `type` property, then we can use the shorthand notation

```js
// Title is a key while the value (SchemaType Object) contains only the type property
{
	title: {type: String},
}
// Shorthand notation of the above
{
	title: String
}

```

##### 1.2.1.3.  `type` property is mandatory to creating a valid `Schema path`
---
If a field of a Document Structure Object is assigned an object literal, then for that Schema Path to be valid, the field's `SchemaType` Object must contain the `type` property.

```js
meta: {
    votes: Number,
    favs: Number
  }
blogSchema.path('meta') // undefined
```

In the above example we see that we have a field/key meta, with an assigned (Object literal) `SchemaType` object which contains two properties, but there is no `type` property in this `SchemaType` object, hence the field meta will not be a valid path  

However the nested keys `meta.votes` and `meta.favs` in their assigned SchemaType Objects do contain the `type` property (shorthand notation) hence those Schema Paths are created.

If a field of a Document Structure Object is assigned an array literal, then that field and value (array) combination becomes a Schema Path without the need of any `type` property

```js
const testSchema = new Schema({try:[]})

testSchema.path("try") // path of type SchemaArray
```

In the above example the field try is assigned an empty array, but if such an array contains another `Document Structure Object` within it, then such a path is called a `Document Array path`, we say that the comments field contains an array of Document Structure Objects

```js
comments: [{ body: String, date: Date }],
```

here, the `comments` path is an array containing another `Document Structure Object`, hence the comments path is a `Document Array Path.`

##### 1.2.1.4.  Allowed values of the `type` property
---
There are 12 allowed values for the `type` property, check out the below link for details

[Allowed values for type property of SchemaType Object](https://mongoosejs.com/docs/schematypes.html#what-is-a-schematype)


#### 1.2.2. Additional properties of a `SchemaType` object
---
[Mongoose Docs for SchemaType Object Properties](https://mongoosejs.com/docs/schematypes.html#schematype-options)

In addition to the `type` property, we can specify additional properties for a path. 

##### 1.2.2.1. Properties for `SchemaType`  objects having any `type`
---
Below are the properties available for a `SchemaType Object` with any `type` property

- required --> boolean or function, if true adds a required validator for this property
- default --> Any or function, sets a default value for the path.
- select --> boolean, specifies whether the field is displayed when querying

- validate --> function, adds a [validator](https://mongoosejs.com/docs/validation.html#built-in-validators) function for this property 
- get --> function, defines a custom getter for this property
- set --> function, defines a custom setter for this property
- alias --> Defines a [virtual](https://mongoosejs.com/docs/guide.html#virtuals) with the given name that gets/sets this path.

- index --> boolean, whether to define an [index](https://www.mongodb.com/docs/manual/indexes/) on this property.
- unique --> boolean, whether to define a [unique index](https://www.mongodb.com/docs/manual/core/index-unique/) on this property.
- sparse --> boolean, whether to define a [sparse index](https://www.mongodb.com/docs/manual/core/index-sparse/) on this property.

```javascript
const numberSchema = new Schema({
  integerOnly: {
    type: Number,
    get: v => Math.round(v),
    set: v => Math.round(v),
    alias: 'i'
  }
});

const Number = mongoose.model('Number', numberSchema);

const doc = new Number();
doc.integerOnly = 2.001;
doc.integerOnly; // 2
doc.i; // 2
doc.i = 3.001;
doc.integerOnly; // 3
doc.i; // 3
```

##### 1.2.2.2. Properties for  `SchemaType Objects` having  `type` of `String`
---
- lowercase --> boolean, whether to always call `.toLowerCase()` on the value
- uppercase --> boolean, whether to always call `.toUpperCase()` on the value
- trim --> boolean, whether to always call [`.trim()`] on the value
- enum --> Array, creates a [validator](https://mongoosejs.com/docs/validation.html) that checks if the value is in the given array
- minLength --> Number, creates a validator that checks for min length
- maxLength --> Number, creates a validator that checks for maxlength 
- match --> RegExp, creates a validator that checks for matching Regular Expression

##### 1.2.2.3. Properties for  `SchemaType Objects` having  `type` of `Number`
---
- max -->Number, creates a validator that checks if the value <= given number
- min --> Number, creates a validator that checks if the value >= given number
- enum --> Array, creates a validator that checks if the value is in the given array

##### 1.2.2.4. Properties for  `SchemaType Objects` having  `type` of `Date`
---
- max -->Number, creates a validator that checks if the value <= given number
- min --> Number, creates a validator that checks if the value >= given number
- expires --> Number or String, TTL index with the value expressed in seconds


#### 1.2.3. The `Schema.prototype.path()` function
---
The `schema.path()` function returns the instantiated schema type for a given path.

```javascript
const sampleSchema = new Schema({ name: { type: String, required: true } });
console.log(sampleSchema.path('name'));
// Output looks like:
/**
 * SchemaString {
 *   enumValues: [],
  *   regExp: null,
  *   path: 'name',
  *   instance: 'String',
  *   validators: ...
  */
```


#### 1.2.4 The `Schema.prototype.add()` function
---
Adds key path / schema type pairs to this schema.

```js
const ToySchema = new Schema();
ToySchema.add({ name: 'string', color: 'string', price: 'number' });

const TurboManSchema = new Schema();
// You can also `add()` another schema and copy over all paths, virtuals,
// getters, setters, indexes, methods, and statics.
TurboManSchema.add(ToySchema).add({ year: Number });
```


### 1.3.  Other object arguments of the `Schema Constructor`
---
Apart from the `Document Constructor` argument, which is the first argument, the `Schema` `constructor` can have multiple other objects as arguments in any order. These object arguments have a single unique property names such as 

- methods --> [Mongoose Docs](https://mongoosejs.com/docs/guide.html#methods)
- statics --> [Mongoose Docs](https://mongoosejs.com/docs/guide.html#statics)
- query --> [Mongoose Docs](https://mongoosejs.com/docs/guide.html#query-helpers)
- virtuals -->  [Mongoose Docs](https://mongoosejs.com/docs/guide.html#virtuals)

and others

Just like we can add the `Document Constructor` object either using the `Schema` constructor or via `Schema.prototype.add()` function, the above objects can also be added to the Schema either via the `Schema constructor` or via a method  named after the above properties.

In this section we will be discussing the `index` property, in later sections we will discuss the `virtual` and the `hooks` property as well

#### 1.3.1. Schema Level Indexes
---
MongoDB supports secondary indexes. With mongoose, we define these indexes within our Schema at the path level or the schema level. Defining indexes at the schema level is necessary when creating `compound indexes`.

```javascript
const animalSchema = new Schema({
  name: String,
  type: String,
  tags: { type: [String], index: true } // path level
  // same as tags: [{ type: String, index: true }]
});

animalSchema.index({ name: 1, type: -1 }); // schema level
```

#### Compund Index
---
A compund index is basically an index with multiple fields

```js
school.index({
 district: 1,
 name: 1
},{unique:true})
```

The above code creates a compound index, which means that for every district, name must be unique, so we cannot have same names for any particular district. 

For compound indexes the order of the fields matter, here name is under district

### 1.4. The  `options` object argument of the `Schema Constructor`
---
Schemas have a few configurable options which can be passed to the constructor or to the `set` method:

```javascript
new Schema({ /* ... */ }, options);

// or

const schema = new Schema({ /* ... */ });
schema.set(option, value);
```

[Check Mongoose Docs for all the Schema Options](https://mongoosejs.com/docs/guide.html#options)


## 2. Mongoose Models
---
Mongoose Models are fancy document constructors compiled from `Schema` definitions, an instance of a model is called a document.

Models are used by Mongoose in order to read / write documents from the underlying MongoDB database.

### 2.1. Creating  a Model from a Schema Object
---
```js
const {Schema, model} = require('mongoose')

// constructing a schema
const schema = new Schema({name: String, size: String})

// creating a model based on said schema
const Tank = model('Tank',schema)
```

the `model` function is not invoked via the new keyword, 

#### 2.1.1 Arguments of the `model` function
---
-  First Argument — A String containing the name of the Collection to which the created model will correspond. 
	- Mongoose while searching/creating the collection, will lowercase and pluralize the string
	- ‘Tank’ is passed so the collection will be tanks, and if the tanks collection is found in the database then that collection will be used, otherwise it will be created
-   Second Argument — The schema object which contains the structure of the documents of the collection.

### 2.2 Model Specific Database Connection
---
Every Mongoose model needs a connection to MongoDB in order to read from or write to MongoDB, in Mongoose every model can have an associated connection to MongoDB, if none is provided, then that model will use the default connection

We will cover this in detail when we discuss Mongoose connections


## 3. Constructing Mongoose Documents
---
A Document is an instance of a Model, in order to create a Mongoose Document object, we have to invoke the model which was created with a particular schema and corresponding to a particular collection.

```js
const tank1 = new Tank({name:"myTank", size:"Large"})
```

### 3.1 Argument of the Document Constructor
---
As we are constructing a Mongoose document, so the input argument to the document constructor function needs to be a MongoDB document, which conforms to the schema on which the model is based.

### 3.2 Saving the created document to the DB
---
Now that we have created a Mongoose Document object, we should save the document in the collection mapped to by the model.

#### 3.2.1 Invoking the `save()` method on the created Mongoose Document
---
In the above example we created the Mongoose document object `tank1` , in order to save it as a document in MongoDB in the collection specified in the model function, we have to invoke the `save()` function on `tank1` . It will save the document object in MongoDB asynchronously.

##### 3.2.1.1. Thenable functions in Mongoose
---
Most Mongoose functions which interacts with MongoDB such as saving data to it, fetching data etc. are functions which can be used in either of the two below mention ways

- A function which returns a Promise so it has a `.then`
- A callback based asynchonous function

In the previous snippet the created Mongoose Object `tank1` can have `save()` applied to it in the following ways

Promise Based approach (it can also be used in async/await functions)

```js
tank1.save().then((data)=>{
	console.log(data)
})

```
 
Callback Based Approach

```js
tank1.save((error,data)=>{
	if (error) console.log(error)
	if (data) console.log(data)
})
```

### 3.3 Mongoose Document Fields
---
We can access and modify the fields of a Mongoose document, constructed using the model contructor.

```js
const blogSchema = new Schema({
  title: String, // String is shorthand for {type: String}
  author: String,
  body: String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean
});

const Blog = model("blog",blogSchema)

const newBlog = new Blog()
```

#### 3.3.1. The `id` field of a Mongoose Document
---
In the above example, we have constructed a Mongoose document `newBlog` , we have not passed any of its properties in the constuctor, but still it gets created with an `id` field, mongoose does this automatically

the `id` field of a mongoose document, is converted into a Mongo `ObjectID` when the document is saved in the database.

#### 3.3.2. Other fields
---
We can provide values to the fields to the `newBlog` document as below, the date field is added automatically because it recieves a default value as per the Schema.

```js
newBlog.title = "My FIrst Blog"

newBlog.body = "Awesome Body"

// We can use push here as newBlog.comments is an array
newBlog.comments.push({body:"Awesome Comment", date: Date.now()})
```

If we print it out we have the below, as the comments field is an array of Document Structure objects (embedded schema), hence each member of the comments array contains an `id` field inserted by mongoose

```json
{
  _id: new ObjectId("642a94fad6f9711bd7246419"),
  comments: [
    {
      body: 'Awesome Comment',
      date: 2023-04-03T08:57:30.384Z,
      _id: new ObjectId("642a94fad6f9711bd724641a")
    }
  ],
  date: 2023-04-03T08:57:30.383Z,
  title: 'My FIrst Blog',
  body: 'Awesome Body'
}
```


## 4.  Schema Associations
---
### 4.1 Referenced Schema
---
#### 4.1.1. One to One Relation
---
We can say that a collection conforming to a particular Schema is associated with one and only one other record of any other collection if a path which is an object literal  of said schema has the below properties

- A path of the Schema has a `type` property of `Schema.Types.ObjectId` 
- That same path has a `ref` attribute pointing to another mongoose model

```js
const storySchema = new Schema(
{ author: { type: Schema.Types.ObjectId, ref: 'Person' }, 
  title: String, });
```

Here we can see that the `author` path of schema `storySchema`  is an object literal which has a `type` property `Schema.Types.ObjectID` and also it has a `ref` property pointing to the model `Person`

Hence we can say that the collection conforming to the schema `storySchema`, in its `author` field contains and ObjectID, which is present in the `_id`  field of any of the document in the collection created from the model `Person`

#### 4.1.2. One to Many Relation
---
We can say that a collection conforming to a particular Schema is associated with multiple other record of any other collection if a path of the former Schema is an array (embedded schema) and it is the following properties

- The embedded Schema has a `type` property of `Schema.Types.ObjectId` 
- The embedded Schema has a `ref` property pointing to another mongoose model

```js
const personSchema = new Schema(
{ name: String, 
  age: Number, 
  stories: [{ type: Schema.Types.ObjectId, ref: 'Story' }] });
```

Here we can see that the `stories` path of schema `personSchema`  is an array of object literal which has a `type` property `Schema.Types.ObjectID` and also it has a `ref` property pointing to the model `Story`

Hence we can say that the collection conforming to the schema `personSchema` is associated with multiple records of a collection pointed towards by the model `Story`

#### 4.1.3. Many to Many Relation
---
Let us assume we have two collections namely `c1` and `c2` , if `c1` has a one to many relationship with `c2`, and at the same time `c2` has a one to many relationship with `c1`, then we can say `c1` and `c2` have a many to many relationship

or example, a `group` can have many `user` and a `user` can belong to many `group`

```js
// Creating the group schema object
const groupSchema = new Schema({
  name:String,
	// users path will contain multiple ObjectID from users collection
  users:[{type:Schema.Types.ObjectId, ref:'User'}]
})
// Creating the user schema object
const userSchema = new Schema({
  name:String,
  age:Number,
	// groups path will contain multiple ObjectID from groups collection
  groups:[{type:Schema.Types.ObjectId, ref:'Group'}]
})

// Creating Group model (document constructor) associated with collection 'groups'
const Group = model('Group',groupSchema)
// Creating User model (document constructor) associated with collection 'users'
const User = model('User',userSchema)

// Creating new Group documents without users fields
const normalAccess = new Group({name:'Normal Access'})
const admin = new Group({name:'Admin'})

// Creating new User documents without groups fields
const arnab = new User({name:"Arnab",age:90})
const modak = new User({name:"Arindam",age:80})

// Adding group id to a user
arnab.groups.push(admin.id)
// Adding user id to a group
admin.users.push(arnab.id)
```

### 4.2 Embedded Schema
---
#### 4.2.1. One to One
---
We can have relationship between schemas as one Schema embedded in another, this is called the embedded schema structure.

```js
const storySchema = new Schema({
  title: String,
  description: String
});

const personSchema = new Schema({
  name: String,
  age: Number,
	// story path will contain a dcoument conforming to the schema storySchema
  story: {type: storySchema}
});
```

We are storing the `storySchema` as an embedded schema within the story field of `personSchema`

```js
const Person = model('person',personSchema)

const person1 = new Person();

person1.name = "Arnab";

person1.age = 90

person1.story = {title: "My story", description: "The meaning of living"}

console.log(person1)
```

Each document created from the `Person` object will, in its `story` field, contain an embeded document conforming to  `storySchema` in its `stories` field.

```json
// The contents of person1 Mongoose document
{
  _id: new ObjectId("642ae97d4e77afbbd919beba"),
  name: 'Arnab',
  age: 90,
  story: {
    title: 'My story',
    description: 'The meaning of living',
    _id: new ObjectId("642ae97d4e77afbbd919bebb")
  }
}
```

#### 4.2.2. One to Many
---
Instead we can store multiple embedded schemas in a field of another schema, we just have to declare the said field as an array

```js
const personSchema = Schema({
  name: String,
  age: Number,
	// story path will contain an array of dcouments conforming to the schema storySchema
  story: [{type: storySchema}]
});
```


## 5. Mongoose Connection
---
### 5.1. Default Connection
---
We have to use the `connect` method defined on the `mongoose` object in order to connect to MongoDB

It takes only an uri string as an argument

```js
connect('mongodb://username:password@host:port/database?options...')
```

Connecting to the DB is an asynchronous operation, thus in mongoose we have options

#### 5.1.1. Invoking `connect()` with a callback function
---
We can use a callback function as a 2nd argument to the  `connect()` function, this function will be triggered once the connection is either established or failed

The callback function takes two arguments

- `error` --> It is an object which contains the error information returned by mongoose regarding why mongoose could not connect to the DB. If connection is established then error is null, error.message contains a suitable message about the error

- `data` --> It is also an object which contains the connection object returned by mongoose, if connection is not established then data is null

```js
const {connect} = require('mongoose')

connect('mongodb://localhost/mydb',(error,data)=>{
	if(error){
		console.log(error.message)
	}
	else{
		console.log("connected")	
	}
})
```

#### 5.1.2. Invoking `connect()` as a promise
---
If we do not use the 2nd argument, then `connect` returns a promise which we can handle as usual. or if we wanted we could call and `await` the `connect` function inside an `async` function.

```js
connect('mongodb://localhost/mydb').
then(()=>{
  console.log("Connected")
})
.catch((error)=>{
  console.log("Could not connect ",error.message)
})
```

#### 5.1.3. Connection Buffer Time
---
Ideally we should be able to do a query only once the `async` operation for `connect` is completed and the promise is resolved, but mongoose lets us query the DB even without connection being established.

The Queries are kept in a buffer for certain time, and only execute once the promise returned by connect is resolved (Or the callback is executed), but the queries will not wait inside the buffer forever, by default it will wait for 10000 ms (10 sec), and if by then the promise returned by connect is not resolved or the callback passed to connect is not executed, then the query present in the buffer will throw error.

So it might happen that, if buffer time for queries is less than the time it takes for connecting to the DB, any query will throw a buffer timeout error first and then the connection gets established. So we should be careful about the order in which connection and query execution occurs, and set the buffer time accordingly. 

##### 5.1.3.1. Setting the Buffer time manually
---
We can set the buffer time like below

```js
mongoose.set('bufferTimeoutMS',8.9);
```

##### 5.1.3.2. Setting Buffer off
---
```js
mongoose.set('bufferTimeoutMS',8.9);
```

So if we disable the buffer, we either have to use async/await, or query in the .then of the connect like as shown below

```js
const myConnection = connect('mongodb://localhost/adoption')

const findResult = myConnection.then(()=>{
  return Pet.find({})
})

findResult.then((data)=>{
  console.log(data)
})
```

### 5.2. Model Specific Connection
---
Instead of using the above discussed default connection, we can make individual connection for individual models.

- We use `createConnection(’uri string’)` to create a connection Object
- The above functon returns a connection Object, we invoke the `.model()` on this returned connection Object to create the model
- Now the constructed model, while interacting with the DB, will use the connection object we created in the first step

```js
// Creating connection object
const petConnection = createConnection('mongodb://localhost/adoption',(error,data)=>{
	if(error){
		// Handling initial connection error
		console.log(error.message)
	}
	else{
		console.log("connected")	
	}
)

const pet = new Schema({},{collection:'pets'})

// calling .model con the connection object
const Pet = petConnection.model('Pet',pet)

Pet.find({})
.then((data)=>{
  console.log(data)
})
```

### 5.3 Handling Connection Errors
---
We can use the `connection` function on the `mongoose` object to handle Database connection errors

we should listen for `error` event on the `connection` function

```js
const {connect, connection} = require('mongoose')

connect('mongodb://localhost/mydb',(error,data)=>{
	if(error){
		// Handling initial connection error
		console.log(error.message)
	}
	else{
		console.log("connected")	
	}
})
// Handling 'error' event
connection.on('error',(err)=>{
	console.log(err)
})
```


## 6. Mongoose Query
---
Queries are always executed on a Mongoose model, Mongoose models provide several static helper functions for CRUD operations. 

Mongoose query functions always contain the term `find` or `delete`, or `replace` or `update`

Each of these functions returns a mongoose Query object.

[List of Mongoose Query Functions](https://mongoosejs.com/docs/queries.html)

### 6.1 Executing a Query Object
---
A mongoose query object can be executed in one of two ways. First, if you pass in a `callback` function, Mongoose will execute the query asynchronously and pass the results to the `callback`.

A query object also has a `.then()` function, and thus can be used as a promise, this is not an actual promise object

A query object also has a `.exec()` function, and thus can be used as a promise, this is an actual promise object

#### 6.1.1. Executing Query using a callback
---
When executing a query with a `callback` function, you specify your query as a JSON document. The JSON document's syntax is the same as the [MongoDB shell](http://www.mongodb.com/docs/manual/tutorial/query-documents/).

One major point to note is that, when we use a callback in a Mongoose query function, the query is **actually executed** by Mongoose and the result is passed to `callback`

A mongoose query function always returns a mongoose query object, even after it has been executed via using a callback function, 

in the below example we are not storing the returned query object in a variable

```javascript
const Person = mongoose.model('Person', yourSchema);

// find each person with a last name matching 'Ghost', selecting the `name` and `occupation` fields
Person.findOne({ 'name.last': 'Ghost' }, 'name occupation', function(err, person) {
  if (err) return handleError(err);
  // Prints "Space Ghost is a talk show host".
  console.log('%s %s is a %s.', person.name.first, person.name.last,
    person.occupation);
});
```

#### 6.1.2. Executing Query with its `.then()`
---
These returned Query objects are not promises, even though they contain a `.then()` so  query objects can be directly executed as a promise, 

calling a query object's `.then()` can execute it multiple times.

```javascript
const q = MyModel.updateMany({}, { isDeleted: true }, function() {
  console.log('Update 1');
});

q.then(() => console.log('Update 2'));
q.then(() => console.log('Update 3'));
```

This query is executed 3 times, once by using the callback and then twice by its `.then()`

#### 6.1.3. Executing Query object using `.exec()`
---
After we have filterd and fine-tuned the query object, then we are ready to execute the query, here we will use the 3rd way to execute the query object, which is using its `.exec()` method. 

This method returns an actual promise, not like the pseudo promise returned by the query object's  `.then()` method's

```javascript
const result = Person.
  find({
    occupation: /host/,
    'name.last': 'Ghost',
    age: { $gt: 17, $lt: 66 },
    likes: { $in: ['vaporizing', 'talking'] }
  }).
  limit(10).
  sort({ occupation: -1 }).
  select({ name: 1, occupation: 1 }).
  exec(); // result becomes an actual promise
  
result.then((data)=>{
	console.log(data)
})
```

### 6.2. Query Helper functions
---
A query function returns us a Query object, once we get a hold of a Query object, then there are several query helper functions available which helps us to fine-tune and filter the query object, until we are ready to execute the same. 

The `.then()` and `.exec()` functions are also query helper functions as they are called on a query object, but their purpose are different from other helper functions.

```javascript
// here query is the Qery Object
const query = Person.findOne({ 'name.last': 'Ghost' });

// chaining query helper functions to filter and fine-tune the query object
query
.where('name.last').equals('Ghost')
.select('name occupation')
.limit(10)
.sort({occupation: -1});

// now to chain with either a .then() or .exec()
```

[List of all the query helper functions](https://mongoosejs.com/docs/api/query.html)


### 6.3 The `.populate()` method
---
The `.populate()` method is a helper function for a Query object

If we have a Collection containing objectIDs from another collection as reference, then we can use the `.populate()` on the model of the former collection and pass the field name where the objectID s are stored, and the `.populate()` method will instead of showing only the referenced objectIDs will fetch the entire document from the referenced collection.

```js
User.findOne({}).populate('groups').then((data)=>{
	console.log(data)
})
```

Here the user collection contains a field groups which contains an array of objectID referenced by the group collection.

so the output of this query is given below

```json
{
  _id: new ObjectId("642a907ae173779565d356b9"),
  name: 'Arnab',
  age: 90,
  // groups instead of being displayed as an array of only Object IDs,
  // is populated as an array of objects
  // with the details from the group collection correcponding to the 
  // objectIDs
  groups: [
    {
      _id: new ObjectId("642a907ae173779565d356b8"),
      name: 'Admin',
      users: [Array],
      __v: 0
    }
  ],
  __v: 0
}
```


## 7. SchemaVirtuals, Hooks
---
### 7.1 Mongoose Schema Virtuals
---
A virtual or more precisely a Virtual field, is a field of a Mongoose document, which is not persisted to the Database, 

So I can `get` or `set` a property that doesn't exist in a database, but when I actually `get` or  `set` it, there is a function that runs 

- For a virtual `get`  --> It computes the virtual field from real fields in the database
- For a virtual `set`  --> It sets real properties in the database, based on the value of the virtual property passed

So, the getters are useful for formatting or combining fields, while setters are useful for de-composing a single value into multiple values for storage.

#### 7.1.1. Virtual getter
---
First we have to define a virtual getter, one way is to add it to the schema options

```javascript
// Again that can be done either by adding it to schema options:
const personSchema = new Schema({
  name: {
    first: String,
    last: String
  }
}, {
  virtuals: {
    fullName: {
      get() {
        return this.name.first + ' ' + this.name.last;
      }
    }
  }
});
```

Or we can invoke the `.virtual()` method on the Schema itself

```javascript
personSchema.virtual('fullName').
  get(function() {
    console.log("In Virtual")
    return this.name.first + ' ' + this.name.last;
  })
```

In order to use this virtual `fullName` getter we first create the model and a sample Mongoose document for testing it

```javascript
const Person = mongoose.model('Person', personSchema);

// create a mongoose document
const axl = new Person({
  name: { first: 'Axl', last: 'Rose' }
});
```

If we now say `axl.fullName` , the function associated with `get` will run 

```javascript
console.log(axl.fullName); 
// In Virtual
// Axl Rose
```


#### 7.1.2. Virtual getter and setter
---
We can set either getter or setter or both on a single virtual field, now let us create both get and set for the `fullName` virtual

```javascript
personSchema.virtual('fullName').
  get(function() {
    return this.name.first + ' ' + this.name.last;
  }).
  set(function(v) {
    this.name.first = v.substr(0, v.indexOf(' '));
    this.name.last = v.substr(v.indexOf(' ') + 1);
  });
  
const Person = mongoose.model('Person', personSchema);

// create a document
const axl = new Person({});
```

Now if we say `axl.fullName = "Axl Rose"`, the `set` function associated with the virtual `fullName` will run and in this case it will set the actual field `name.first` and `name.last`

These set and get functions must be ordinary JS functions, not arrow functions, because we require the proper`this` binding


### 7.2 Schema Middleware or `pre()` and `post()` Hooks
---
[Mongoose Docs](https://mongoosejs.com/docs/middleware.html)

what you can do is you can add middleware for different `events` that can happen on the collection. An event is any operation that could happen, a find, a find one and delete, a save, a remove, all those different types of operations you can do on a model, you can either tie into them `before` they happen or `after` they happen, and it can be synchronous or asynchronous.

In other words, we can say that a middleware is like, for a certain event, we can fire a certain function, before or after that event triggers. (pre or post)

The middleware is configured at the `Schema` level on the created `Schema Object`

For almost any opration we can do on a model there is a middleware or hook for it

```js
// Suppose we have a school schema object

// This function will run before each document in saved to the Database
school.pre("save", function(){
 console.log(`${this} document will be saved`)
})
```

Use regular functions to make sure the `this` binding is the correct one.

#### 7.2.1. Usecases of Middlewares
---
-  Complex validation
-   Removing dependent documents (removing a user removes all their blogposts)
    
	```js
orgSchema.post("remove",async function(doc, next){
	await Project.deleteMany({org:doc._id})
})
```

This is an example of of removing dependent documents `asynchronously`, here after a `org` document is removed then all the `project` document associated with it are also removed

-   Asynchronous defaults
-   Asynchronous tasks that a certain action triggers

