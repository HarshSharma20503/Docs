1. Create an npm account if you don't have one:

	```
	npm adduser 
	```

2. If you're using a scoped package (starting with @), configure npm:

	```bash
	npm config set scope your-username
	```

3. Make sure you're logged in:
	```bash
	npm login
	```

4. Test your package locally:

	```bash
	npm link 
	terminal-portfolio  # to test the command
	```

5. Publish to npm:

	```bash
	npm publish --access public
	```


Additional publishing tips:

- Use `npm version patch/minor/major` to update version numbers
- Test the package thoroughly before publishing
- Make sure your .gitignore and .npmignore are properly configured
- Include appropriate keywords in package.json for better discoverability

After publishing, users can install your package using:

`npx @yourusername/terminal-portfolio`

Remember to:

1. Choose a unique package name
2. Keep your npm account credentials secure
3. Update the version number when you make changes
4. Include appropriate documentation
5. Test the installation process on a clean system


To further update and publish add the following script to package.json and run any of them

```JSON
{
...
script: {
	...
		"version:patch": "npm version patch && git push && git push --tags && npm publish --access public",
	    "version:minor": "npm version minor && git push && git push --tags && npm publish --access public",
	    "version:major": "npm version major && git push && git push --tags && npm publish --access public",
	    "publish:patch": "git add . && git commit -m \"patch update\" && npm run version:patch",
	    "publish:minor": "git add . && git commit -m \"minor update\" && npm run version:minor",
	    "publish:major": "git add . && git commit -m \"major update\" && npm run version:major"
	...
}
...
}
```

Now you can use these commands:

For quick version updates (if changes are already committed):


```bash 
npm run version:patch  # For bug fixes (1.0.0 -> 1.0.1) 
npm run version:minor  # For new features (1.0.0 -> 1.1.0) 
npm run version:major  # For breaking changes (1.0.0 -> 2.0.0)
```

For complete updates including committing changes:

```bash
npm run publish:patch  # Commits changes and publishes patch update 
npm run publish:minor  # Commits changes and publishes minor update 
npm run publish:major  # Commits changes and publishes major update
```

These scripts will:

1. Update the version number
2. Push changes to git
3. Push tags to git
4. Publish to npm