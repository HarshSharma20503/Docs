>[!SUMMARY]+ Table of Contents
>- [[Getting Started#Installing React using vite bundler|Installing React using vite bundler]]
>- [[Getting Started#Removing Unnecessary files|Removing Unnecessary files]]
>- [[Getting Started#Setting up folders and files|Setting up folders and files]]
>- [[Getting Started#Further you can do |Further you can do ]]
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
```

Create a git repo and connect it with your GitHub and then start working with project.

```
git init
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/HarshSharma20503/CodeSphere.git
git push -u origin main
```

---
# Further you can do 

- [[My Documentation/Frontend-Development/React-Bootstrap/Getting Started|Connect React-Bootstrap]]
- [[Add environment variable in React-Vite]]
- [Use Axios](obsidian://open?vault=Obsidian&file=My%20Documentation%2FPackages%2FAxios)
- [[Create Routes]]
- 