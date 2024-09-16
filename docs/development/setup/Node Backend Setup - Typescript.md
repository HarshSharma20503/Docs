## 1. Project Setup:

- Create a new directory:
```bash
mkdir my-ts-project
cd my-ts-project
```

- Initialize npm:
```bash
npm init -y
```

- Install Dependencies:
```bash
npm install express
npm install typescript ts-node @types/express --save-dev
npm install nodemon --save-dev
```

## Configure Typescript

- Initialise typescript configuration
```bash
npx tsx --init
```

- Update tsconfig.json file
For using the `require` syntax
```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

For using the import syntax
```json
{
  "compilerOptions": {
    "target": "ES2020",        // Ensure compatibility with modern JavaScript
    "module": "ESNext",        // Use ES Modules
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "moduleResolution": "node",  // Ensure module resolution works correctly
    "resolveJsonModule": true,   // If you plan to import JSON
    "forceConsistentCasingInFileNames": true
  }
}
```

- **Configure `package.json` file:**
For using `import` syntax
```JSON
{
  "name": "my-ts-server",
  "version": "1.0.0",
  "type": "module",  // This enables ESM
	"scripts": {
	  "start": "node dist/index.js",
	  "dev": "nodemon --watch src --exec 'ts-node --esm' src/index.ts",
	  "build": "tsc"
	}
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "typescript": "^4.9.5",
    "ts-node": "^10.9.1",
    "nodemon": "^2.0.19",
    "@types/express": "^4.17.14"
  }
}
```

For using `require` syntax
```json
"scripts": {
  "start": "node dist/index.js",
  "dev": "nodemon --watch src --exec ts-node src/index.ts",
  "build": "tsc"
}
```

## Setup Folder structure

- Create src folder and than internal folders
```bash
mkdir src public
touch .env .env.dev
cd public
mkdir temp
cd temp
touch .gitkeep
cd ../../src
cd src
touch index.ts app.ts 
mkdir controllers models routes middlewares config scripts utils
```
.env to store environment variables and .env.dev to store the enviornement variable names so that it could be pushed to GitHub

## Basic Server code 

```typescript
import express, { Request, Response } from "express";

const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.json());

app.get("/", (req: Request, res: Response) => {
  res.send("Hello, TypeScript with Node and Express!");
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

## Run in development

- Run the command `npm run dev` to run the project
`Note`: There might be error `TypeError [ERR_UNKNOWN_FILE_EXTENSION]: Unknown file extension ".ts"`, this error can be resolved by installing node version 19 because ts-node is not updated to the latest node version. Use NVM to install node verison 19

## Run in production

- `npm build` and then `npm start`