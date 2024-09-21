A utility function which takes function as a parameter and runs it as a async await and also handles the error. Mostly used for middlewares.

Two ways it can be done:
1) Using try and catch
2) Using promise resolve and catch
---
# Try and Catch

```javascript
const asyncHandler = (fn) => async (req, res, next) => {
    try {
        await fn(req, res, next);
    } catch (error) {
        res.status(error.statusCode || 500).json({
            success: false,
            message: error.message || "Internal Server Error",
        });
    }
};

export { asyncHandler };
```

```javascript
const asyncHandler = (fn) => async (req, res, next) => {
  try {
    return await fn(req, res, next);
  } catch (error) {
    console.log("******** Inside AsyncHandler ********");
    console.log("Error: ", error);
    res.status(error.statusCode || 500).json({
      success: false,
      message: error.message || "Internal Server Error",
    });
  }
};

export { asyncHandler };
```

The version you should use depends on your specific use case. If you need to use the result of fn after asyncHandler is called, you should use the second version. If you don't need the result of fn, you can use the first version.

---
# Promise, Resolve and Catch

```javascript
const asyncHandler = (requestHandler) => {
    return (req, res, next) => {
        Promise.resolve(requestHandler(req, res, next)).catch((err) =>
            next(err)
        );
    };
};

export { asyncHandler };
```
---
# How to use the function

```javascript
import { asyncHandler } from "../utils/asyncHandler.js";

const registerUser = asyncHandler(async (req, res) => {
    res.status(200).json({ message: "User registered successfully" });
});

export { registerUser };
```
---
