When working with credentials such as cookies that are set by the backend, it’s crucial to configure CORS (Cross-Origin Resource Sharing) correctly to ensure that the browser can send the cookies along with the request. Misconfiguring CORS can prevent the browser from sending cookies, leading to issues with authentication and session management.

**Avoid Using Wildcard (*) in CORS Configuration**
A common mistake is to set the origin option in the CORS configuration to "*", which allows any origin. When you also set credentials: true, this configuration will prevent the browser from sending cookies because the Access-Control-Allow-Origin header cannot be set to * if credentials are used.

Example of incorrect configuration:
```javascript
app.use(
  cors({
    origin: "*",
    credentials: true,
  })
);
```

**Why is this wrong?**
• The browser will not send cookies or other credentials along with the request.

• The Access-Control-Allow-Origin header cannot be set to "*" when credentials: true is used, resulting in a CORS error.

**Specify the Allowed Origin Explicitly**
To allow the browser to send cookies, you must specify the exact origin that is permitted to access the resource.

Example of correct configuration:
```javascript
app.use(
  cors({
    origin: "http://localhost:5173",
    credentials: true,
  })
);
```

**Why is this correct?**
• By specifying the exact origin (http://localhost:5173), you ensure that the browser will send cookies along with the request.

• This configuration adheres to CORS policy requirements, allowing the server to handle credentials properly.