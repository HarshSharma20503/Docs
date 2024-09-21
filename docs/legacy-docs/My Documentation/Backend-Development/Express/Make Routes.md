>[!SUMMARY]- Table of Contents
>- [[Make Routes#Inside App.js|Inside App.js]]
>- [[Make Routes#Inside user.routes.js|Inside user.routes.js]]
>- [[Make Routes#Inside User.controller.js|Inside User.controller.js]]
# Inside App.js

```javascript
// routes import
import userRouter from "./routes/user.routes.js";
//routes declaration
app.use("/api/v1/users", userRouter);
```

These lines of code import and declare a route for handling user-related API requests, specifying that requests to "/api/v1/users" should be directed to the userRouter module.

---
# Inside user.routes.js

```javascript
import { Router } from "express";
import { registerUser } from "../controllers/user.controller.js";

const router = Router();

router.route("/register").post(registerUser);

export default router;
```

This code snippet creates a router using Express, defines a route for registering users with a POST request, and assigns the registerUser function from the user controller to handle requests to this route. Finally, it exports the router module.

---
# Inside User.controller.js

```javscript
import { asyncHandler } from "../utils/asyncHandler.js";

const registerUser = asyncHandler(async (req, res) => {
    res.status(200).json({ message: "User registered successfully" });
});

export { registerUser };
```

This code exports a function named registerUser, wrapped with asyncHandler to handle asynchronous operations. When called, it sends a JSON response indicating successful user registration with a status code of 200.

---
# Templates

- [[Login-Logout-refresh]] - login, logout and refresh token endpoints
- [[Update User Details]] - update user Details like username etc
- [[Get Current User Details]] - get details of users