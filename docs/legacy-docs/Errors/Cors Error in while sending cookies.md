when using credentials like cookie that are begin set by backend
make sure to not use cors configuration like this:

```javascript
app.use(
  cors({
    origin: "*",
    credentials: true,
  })
);
```

The above approach will not let browser send the cookies

Instead be specific like this:

```javascript
app.use(
  cors({
    origin: "http://localhost:5173",
    credentials: true,
  })
);
```
