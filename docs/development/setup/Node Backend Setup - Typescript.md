**1. Project Setup:**

- **Create a new directory:**
```bash
mkdir my-ts-project
cd my-ts-project
```

- **Initialize npm:**
```bash
npm init -y
```

- **Configure `package.json` file:**
```JSON

```

- **Install dependencies:**
```bash
npm install express dotenv mongoose cors jsonwebtoken bcryptjs cookie-parser
npm install @types/express typescript ts-node @types/node nodemon prettier -D
```

**2. Create TypeScript Configuration:**

- **Initialise Typescript default config**:
```console
npx tsc init
```

- **Configure `tsconfig.json` file:**
```JSON
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  }
}
```

**3. Create the File and Folder structure:**

- **Create folders**
```bash
mkdir src public
cd public
mkdir temp %% for temproary storage%%
cd ../src
mkdir controllers models config utils middlewares routes
```

- **Create files**
```bash
touch .gitkeep %% inside the  %%
```

**4. Create the Server File:**

- **Create a `src/index.ts` file:**
```typescript
import express from 'express';

const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello from TypeScript Express server!');
});

app.listen(port, () => {
  console.log(`Server listening on port ${port}`);
});
```

**4. Start the Server:**

- **Execute the server:**
```bash
ts-node src/index.ts
```
