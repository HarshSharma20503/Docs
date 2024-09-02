# Installing React using vite bundler

Create a folder in which you want to create your project. Write following command in it.

```console
npm create vite@latest
```

Answer all the question related to it.

```console
 Project name: … CodeSphere-Frontend
✔ Package name: … codesphere-frontend
✔ Select a framework: › React
✔ Select a variant: › JavaScript

Scaffolding project in /home/berserk/files/Shiv-Nadar-Hackathon/CodeSphere-Frontend...

Done. Now run:

  cd CodeSphere-Frontend
  npm install
  npm run dev
```

Run $npm \space install$ to install node modules.

```console
npm install
```

Now to run the file write following command in terminal

```console
npm run dev
```

---
# Removing Unnecessary files

Remove the following files
- public/vite.svg
- assets folder

Remove the icon from index.html and the extra meta tags.

Clear the app.css, index.css and app.jsx file. Write rafce (used to create react arrow functional component) and start making changes.

---
# Setting up folders and files

- Create a $components$ folder inside the src to store the components.
- Create a $pages$ folder inside the src to store the pages files.

Create .prettierrc and add the configurations

```json
{
	"singleQuote": false,
	"bracketSpacing": true,
	"tabWidth": 4,
	"trailingComma": "es5",
	"semi": true
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