## 1. What is `Prisma`
---
`Prisma` is a DB agnostic, type safe `ORM`. It supports most DBs out there. It consists of the following parts
1. The `schema.prisma` file, where we are going to model our data
	- Allows developers to define their _application models_ in an intuitive data modeling language
	- It also contains the connection to a database and defines a _generator_.
2. `Prisma migration` system, which creates the actual database as per the model present in the `schema.prisma` file.
	- There is also the introspection system, where Prisma will write the data model automatically as per an already existing database
3. `Prisma-Client`, we have to generate the client, which is a` type-safe query builder` for Node.js & TypeScript
4. `Prisma studio` , it is a GUI to view and edit data in your database.

`Prisma` also comes with its own `CLI` engine, which we can access in the terminal using `npx prisma ..` 
`npx` comes with `npm` which allows us to use `CLI` of other npm packages.

### 1.1. Using `Prisma` with `TypeScript`
---
`TypeScript`  enables the generated `Prisma-Client` to become `type-checked`.
Knowledge of `TypeScript` is not required, we are going to write plain old JS, but converting our app to `TypeScript` will allow us the sweet auto-completions for our DB interaction through the generated `type-checked Prisma Client` 

It means that we can't accidentally access a property that doesn't exist (and any typos are caught at compile-time). It will save us from tabbing back and forth between our handler function and `schema.prisma` file

## 2. Installing `Prisma`
---

### 2.1. Installing the `npm` packages
---
We are going to install the below npm packages
- typescript --> `Transpiler` that converts `TypeScript` to `JavaScript`
- ts-node --> It is a TypeScript execution engine and REPL for Node.js.
- @types/node --> This package contains type definitions for Node.js runtime
- prisma --> The Prisma `CLI`
- @prisma/client --> The Prisma client

`npm i typescript ts-node @types/node prisma --save-dev`

#### Note
- TypeScript allows us to use `ES6` modules right out of the box
- Now we have to run our `.ts` files using `ts-node file_name.ts`

### 2.2. Creating the `tsconfig.json`
---
Then create a `tsconfig.json` file which is the config file for TypeScript. Add this to that file

```json
{
  "compilerOptions": {
    "sourceMap": true,
    "outDir": "dist",
    "strict": true, // Remove this key value pair
    "lib": ["esnext"],
    "esModuleInterop": true
  }
}
```

Please remove the `"strict": true` key value pair, so that we do not have to write all the `TypeScript` types.

### 2.3. Initialize `Prisma`
---
Next we have to initialize Prisma using `npx prisma init`. Here we are using `npx` to access the prisma `CLI`. The `init` command does the following
- Create a prisma folder
- Create the `schema.prisma` file in that folder
- Also creates a `.env` file in the root of the project

## 3. Prisma Schema 
---
The `schema.prisma` file contains the below info

```json
generator client {
	provider = "prisma-client-js"
} 

datasource db {
	provider = "mysql"
	url = env("DATABASE_URL")
}

model Model1 {

}
model Model2 {
	
}
```

`generator client` --> Refers to the client which we will use to query the database, once the models  are ready

`datasource db` --> It contains the `provider` or the name of the database we will be using for the project, and the `url` string for the database connection will be loaded from the `DATABASE_URL` variable located in `.env` file