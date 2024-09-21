Encapsulate information about the outcome of an API operation and provide a standardised format for returning responses.

---
# Create file in Utils folder

- Create a file name ApiResponse.js

```javascript
class ApiResponse {
    constructor(statusCode, data, message = "Success") {
        this.statusCode = statusCode;
        this.data = data;
        this.message = message;
        this.success = statusCode < 400;
    }
}

export { ApiResponse };
```

---
# How to use the ApiResponse class

```javascript
import { ApiResponse } from "../utils/ApiResponse.js";
...

const registerUser = asyncHandler(async (req, res) => {
    ...
    return res
        .status(201)
        .json(new ApiResponse(200, createdUser, "User registered Succesfully"));
});

export { registerUser };
```