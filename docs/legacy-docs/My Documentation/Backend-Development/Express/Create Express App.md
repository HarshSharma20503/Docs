>[!SUMMARY]+ Table of Contents
>- [[Create Express App#Set up server |Set up server ]]
>- [[Create Express App#Set up middlewares|Set up middlewares]]
# Set up server 

Inside the app.js file in src folder create the express app and export it.

```javascript
import express from "express";

const app = express();

export { app };
```

---
# Set up middlewares

install and set up cors and cookie parser.

```console
npm i cors cookie-parser
```

Enter all the accepted Origins in environment variable

```
CORS_ORIGIN = *
```

You can save multiple origins and than while using cors you can split them via space or comma in an array than use it.

Set up all the necessary origins:

```javascript
import express from "express";
import cors from "cors";
import cookieParser from "cookie-parser";

const app = express();

app.use(
    cors({
        origin: process.env.CORS_ORIGIN,
        credentials: true,
    })
);

app.use(express.json({ limit: "16kb" })); // to parse json in body
app.use(express.urlencoded({ extended: true, limit: "16kb" })); // to parse url
app.use(express.static("public")); // to use static public folder
app.use(cookieParser()); // to enable CRUD operation on browser cookies

export { app };
```
Setting `credentials: true` in a CORS (Cross-Origin Resource Sharing) middleware configuration allows the inclusion of credentials like cookies, HTTP authentication, and client-side SSL certificates in cross-origin requests. This means that when a request is made from a different origin domain, the browser will include cookies and other credentials associated with the request, allowing the server to determine if the request should be accepted or rejected based on those credentials.