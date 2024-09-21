To serve static files such as images, CSS files, and JavaScript files, use the `express.static` built-in middleware function in Express.

```javascript
app.use(express.static('public'))
```

Now this public folder can be anything.

You can even build the react application and then move the build folder (dist in case of the vite) in the backend folder and then serve static folder.

Than you won't require the ' / ' route.
