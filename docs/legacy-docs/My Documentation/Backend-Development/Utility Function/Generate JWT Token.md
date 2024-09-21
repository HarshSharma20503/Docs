Inside util function after installing jsonwebtoken package
```javascript
import jwt from "jsonwebtoken";

const generateJWTToken = (_id) => {
  return jwt.sign({ _id }, process.env.JWT_SECRET, {
    expiresIn: "30d",
  });
};

export { generateJWTToken };
```

To use to simply import the function and then use it by providing the id