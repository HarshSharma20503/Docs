# Deploying Node.js and TypeScript Backend Applications to Vercel

## Table of Contents
- [Node.js Backend Deployment](#nodejs-backend-deployment)
- [TypeScript Backend Deployment](#typescript-backend-deployment)
- [Troubleshooting](#troubleshooting)

## Node.js Backend Deployment

### Prerequisites
1. Node.js installed on your local machine
2. A Vercel account
3. Git installed on your machine
4. Your Node.js application code

### Project Setup
1. Ensure your project has a proper `package.json` file with all dependencies listed
2. Your main entry file should be `index.js`
3. Make sure your application listens on `process.env.PORT` for compatibility:
```javascript
const port = process.env.PORT || 3000;
app.listen(port, () => {
    console.log(`Server running on port ${port}`);
});
```

### Vercel Configuration
1. Create a `vercel.json` file in your project root:
```json
{
  "version": 2,
  "builds": [
    {
      "src": "index.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "index.js",
      "methods": ["GET", "POST", "PUT", "DELETE", "PATCH", "OPTIONS"]
    }
  ]
}
```

### Deployment Steps

1. Initialize Git repository (if not already done):
```bash
git init
```

2. Create `.gitignore` file:
```
node_modules
.env
.env.local
```

3. Push your code to GitHub:
```bash
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin your-github-repo-url
git push -u origin main
```

4. Deploy using Vercel UI:
   - Go to [Vercel Dashboard](https://vercel.com/dashboard)
   - Click "Add New..." → "Project"
   - Select your GitHub repository from the list
   - Vercel will automatically detect your Node.js project
   - Configure your project settings:
     - Build Command: `npm run build` (if needed)
     - Output Directory: Leave empty for Node.js
     - Environment Variables: Add any required env variables
   - Click "Deploy"

5. After deployment, you'll get:
   - Production URL (your-project.vercel.app)
   - Dashboard URL for monitoring
   - Automatic deployments for future GitHub pushes

## TypeScript Backend Deployment

### TypeScript Prerequisites
1. All Node.js prerequisites
2. TypeScript installed: `npm install -g typescript`
3. `ts-node` for development: `npm install --save-dev ts-node`

### TypeScript Project Setup
1. Initialize TypeScript configuration:
```bash
tsc --init
```

2. Update `tsconfig.json` with recommended settings:
```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

3. Structure your project:
```
your-project/
├── src/
│   └── index.ts
├── dist/
├── package.json
├── tsconfig.json
└── vercel.json
```

### TypeScript Vercel Configuration
1. Create `vercel.json` with TypeScript configuration:
```json
{
  "version": 2,
  "builds": [
    {
      "src": "src/index.ts",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "src/index.ts",
      "methods": ["GET", "POST", "PUT", "DELETE", "PATCH", "OPTIONS"],
    }
  ]
}
```

2. Update `package.json` scripts:
```json
{
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

### TypeScript Deployment Steps

1. Build your TypeScript project locally to test:
```bash
npm run build
```

2. Push your code to GitHub following the same steps as Node.js deployment

3. Deploy using Vercel UI:
   - Go to [Vercel Dashboard](https://vercel.com/dashboard)
   - Click "Add New..." → "Project"
   - Select your GitHub repository
   - Vercel will automatically detect TypeScript
   - Configure project settings:
     - Build Command: `npm run build`
     - Output Directory: `dist` (or as specified in tsconfig.json)
     - Environment Variables: Add any required env variables
   - Click "Deploy"

4. Vercel will automatically:
   - Install dependencies
   - Build your TypeScript project
   - Deploy the compiled JavaScript
   - Set up automatic deployments for future pushes

## Troubleshooting

### Common Issues and Solutions

1. **CORS Issues**
   - Verify the headers in `vercel.json` are correct
   - Ensure your application code isn't overriding CORS settings

2. **Build Failures**
   - Check if all dependencies are listed in `package.json`
   - Verify TypeScript configuration in `tsconfig.json`
   - Look for syntax errors in source files

3. **Runtime Errors**
   - Check environment variables are properly set in Vercel dashboard
   - Verify file paths in imports are correct
   - Check for platform-specific code that might not work on Vercel
