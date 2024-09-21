>[!SUMMARY]- Table of Contents
>- [[Getting Started#Initialising Package.json|Initialising Package.json]]
>- [[Getting Started#Installing Packages|Installing Packages]]
>- [[Getting Started#Create file and folders|Create file and folders]]
>- [[Getting Started#Making Changes in package.json|Making Changes in package.json]]
>- [[Getting Started#Further Things|Further Things]]
>- [[Getting Started#Basic Code|Basic Code]]
# Initialising Package.json

- Create a folder for your back-end project open terminal in vs code and write **npm init** in it.

```console
npm init
```

- It will ask various questions answer them and then create a Package.json file in you folder.
---
# Installing Packages

Write following line in your terminal.

```console
npm install express dotenv mongoose cors
```

```
npm i nodemon prettier -D
```

 This will install express, dotenv and mongoose in your dependencies and nodemon in your dev dependencies (Check your package.json to confirm).
 
 ---
# Create file and folders

Create folder such as follows
- public (folder) - to keep all public files
	- temp - to keep all the temporary files like images.
		- .gitkeep
- src (folder)- all source code
	- index.js (file) - first file to run
	- app.js (file) - 
	- constants (file) - to keep all the constants
	- controllers (folder) - to keep all the functionalities files
	- routes (folder) - to keep all the routes files
	- config (folder) - all the configuration (database and third party api) related files
	- middlewares (folder) - all the middlewares
	- utils (folder) - all the utility files and functions
	- models (folder) - to store all the schema and models of mongoose
- .env (file) - stores all the environment variables.
- .gitignore - keeps files names you do not want in GitHub repo.
- .prettierrc - to store prettier configuration
- .prettierignore - to store all the files that prettier should ignore

Create .env file and add your environment variables.

```
PORT = 3000
```

Create .gitignore and add the files you do not want in your GitHub repo.
You can also use gitignore generator to generate it.

```
node_modules
.env
```

Create .prettierrc and add the configurations

```json
{
  "singleQuote": false,
  "bracketSpacing": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "semi": true,
  "printWidth": 120
}
```

Create .prettierignore and add the configurations.
You can also use prettierignore generator to generate it.

``` 
*.env
.env
.env.*
/.vscode
/node_modules
./dist
```

---
# Making Changes in package.json

- Add Start and dev scripts in your package.json and also include type module in it.

```json
"type": "module",
"scripts": {
		"start": "node index.js",
		"dev": "nodemon index.js"
	}
```

This is how it will look

```json
{
	"name": "chaibackend",
	"version": "1.0.0",
	"description": "A backend project to learn Backend",
	"main": "index.js",
	"type": "module",
	"scripts": {
		"dev": "nodemon src/index.js",
		"start": "node src/index.js"
	},
	"keywords": [
	"Javascript",
	"Backend"
	],
	"author": "Harsh Sharma",
	"license": "ISC",
	"devDependencies": {
		"nodemon": "^3.0.3",
		"prettier": "3.2.4"
	},
	"dependencies": {
		"dotenv": "^16.3.2",
		"express": "^4.18.2",
		"mongoose": "^8.1.0"
	}
}
```

- removed test script and added start and dev script
- added type as module which will enable ES 6 imports

Now you can run the backend by writing

```console
npm run dev
```

---
# Further Things

## Project Setup 

- [[Create Express App]] - Create express server and set up some basic middlewares.
- [[Connect Database]] - Connect to MongoDB Database
- Add utility Function and classes
	- [[AsyncHandler]] - Utility function to get a generalised try catch function.
	- [[ApiError]] - Utility class to standardise the error shown 
	- [[ApiResponse]] - Utility class to standardise the response returned 
- [[Modelling Schema]] - Model your mongoose schema
- [[Handle File upload]] - Backend to upload file if any
- [[Setup Postman]] - setup postman for easy url testing

## Creating actual backend

- [[Make Routes]] - Start making routes
- 


---
# Basic Code

Write the following code in your main entry point file e.g. Index.js.

```javascript
import express from 'express'
import 'dotenv/config';
import cors from 'cors';

const app = express();
const port = process.env.PORT || 3000;

app.use(
    cors({
        origin: process.env.CORS_ORIGIN,
        credentials: true,
    })
);

app.get('/', (req, res) => {
res.send('Hello World!')
})

app.listen(port, () => {
console.log(`Example app listening on port ${port}`)
})
```

This code will create a server that will listen to port 3000 and has create a ' / ' route which will return Hello world in return.

