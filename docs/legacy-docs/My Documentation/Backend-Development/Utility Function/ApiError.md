create a custom error class, `ApiError`, specifically tailored for handling errors that may occur in the context of API operations. This custom error class extends the built-in JavaScript `Error` class and includes additional properties and logic to capture relevant information about API-related errors.

# Create file in Utils folder

- Create a file name ApiError.js

```javascript
class ApiError extends Error {
    constructor(
        statusCode,
        message = "Something went wrong",
        errors = [],
        stack = ""
    ) {
        super(message);
        this.statusCode = statusCode;
        this.data = null;
        this.message = message;
        this.success = false;
        this.errors = errors;

        if (stack) {
            this.stack = stack;
        } else {
            Error.captureStackTrace(this, this.constructor);
        }
    }
}

export { ApiError };
```

---
# How to use the ApiError case

```javascript
import { ApiError } from "../utils/ApiError.js";
...

const registerUser = asyncHandler(async (req, res) => {
    ...
    const { fullname, email, username, password } = req.body;
    console.log(
        "Inside registerUser Controller:\n Details recieved",
        fullname,
        email,
        username,
        password
    );
    if (
        [fullname, email, username, password].some(
            (field) => field?.trim() === ""
        )
    ) {
        throw new ApiError(400, "ALL fields are required");
    }
	...
});

export { registerUser };
```