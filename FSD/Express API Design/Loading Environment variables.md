## 1. What are Environment variables
---
Environment variables are dynamic values that can affect the behavior of an application. 
They are essentially named values that can be used by programs to access various configuration settings, system paths, or other important information.
Environment variables are loaded into the application's runtime so that we do not need to hard-code those values in our application itself

```js
app.listen(3000,()=>{
	console.log("Server started in port 3000")
})
```

Here the port number 3000, is hard coded in our app, during deployment or in some other cases we may need to change the same. Every time we require changing the port number, we have to edit the code itself.

It would be better if we make the port number an Environment variable, and load the same into our runtime, so that if we need to change it in future we can just change the Environment variable, rather that messing with the actual code

```js
const port = process.env.PORT || 8080
app.listen(port,()=>{
	console.log(`Server stated on port ${port}`)
})
```

Here the port is being read from `process.env.PORT` . In nodeJS all Environment variables are read from `process.env`. But we do not have any `PORT` variable in `process.env` by default, we have to do the following
- Make a file containing all the variables we want
- somehow make `nodeJS` load those variables into `process.env`

## 2. creating the `.env` file
---
The `.env` file should be present in the `root` of our project, and whatever variables we want to be loaded into `process.env` , we place those here

```.env
PORT = 3000
```

## 3. Loading variables present in `.env` into `process.env`
---
In order to do this we use an npm package called `dotenv`. 

- First we have to install the package using `npm install dotenv --save`
- Whatever script file we are using to run the application (e.g app.js), we have to tell that file to load `.env` contents into `process.env` at runtime, we do that by saying`require("dotenv").config({path:'path_to_env'})` in the `app.js` file
- The object in the argument of `.config({})` is required only if the main server file and the .`env` file are not in the same directory.
- This will ensure that during the running of the application `process.env` will have access to all the variables present in the `.env` file

If using something like `nodemon` to start the server via a `dev` or `start` script in `package.json`, we should tell node to use the dotenv config like so

```json
{
	"scripts": {
		"start": "NODE_ENV=prod node -r dotenv/config src/server",
		"dev": "nodemon -r dotenv/config src/server"
	},
}
```

## 4. `.env` file not to be uploaded
---
When you are uploading your application code to `github` or some such other place, remember to always remove the `.env` from the list of files to be uploaded. This is to be done as the `.env` file contains sensitive information which you do not want others to access, such as DB_URL etc.
