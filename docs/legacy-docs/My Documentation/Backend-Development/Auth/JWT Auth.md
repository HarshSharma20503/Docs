# Installation

```
npm i jsonwebtoken bcrypt
```

# Steps

- Create generate token function in util [[Generate JWT Token]]
- Create user model
- Inside user model
```javascript
import mongoose, { Schema } from "mongoose";
import bcrypt from "bcrypt";

const userSchema = new Schema(
  {
    name: {
      type: String,
      required: true,
    },
    email: {
      type: String,
      required: true,
      unique: true,
    },
    password: {
      type: String,
      required: true,
    },
  },
  { timestamps: true }
);

userSchema.methods.matchPassword = async function (enteredPassword) {
  return await bcrypt.compare(enteredPassword, this.password);
};

userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) {
    next();
  }
  this.password = await bcrypt.hash(this.password, 12);
});

export const User = mongoose.model("User", userSchema);
```

- Create routes for registration and login
- Create user controllers for registerUser and login User
- Inside user controller
```javascript
import { asyncHandler } from "../utils/AsyncHandler.js";
import { ApiResponse } from "../utils/ApiResponse.js";
import { ApiError } from "../utils/ApiError.js";
import { User } from "../models/user.model.js";
import { generateJWTToken } from "../utils/GenerateToken.js";

const registerUser = asyncHandler(async (req, res) => {
  console.log("******** registerUser Function ********");

  const { name, email, password, pic } = req.body;
  if (!name || !email || !password) {
    throw new ApiError(400, "All fields are required");
  }

  const existingUser = await User.findOne({ email });
  console.log("Existing User", existingUser);
  if (existingUser) {
    throw new ApiError(400, "User already exists");
  }

  const user = await User.create({ name, email, password, pic });

  if (user) {
    return res.status(200).json(
      new ApiResponse(200, "User registered successfully", {
        _id: user._id,
        name: user.name,
        email: user.email,
        token: generateJWTToken(user._id),
      })
    );
  }

  throw new ApiError(500, "Failed to create User");
});

const loginUser = asyncHandler(async (req, res) => {
  console.log("******** loginUser Function ********");
  const { email, password } = req.body;
  if (!email || !password) {
    throw new ApiError(400, "All fields are required");
  }

  console.log("User details", email, password);

  const user = await User.findOne({ email });
  if (user && (await user.matchPassword(password))) {
    return res.status(200).json(
      new ApiResponse(200, "User logged in successfully", {
        _id: user._id,
        name: user.name,
        email: user.email,
        token: generateJWTToken(user._id),
      })
    );
  }
  throw new ApiError(401, "Invalid email or password");
});

export { registerUser, loginUser };
```

- now create a jwtVerify middleware to check every api call is authenticated or not
- inside JWTverify
```javascript
import { User } from "../models/user.model.js";
import { ApiError } from "../utils/ApiError.js";
import { asyncHandler } from "../utils/asyncHandler.js";
import jwt from "jsonwebtoken";

export const verifyJWT = asyncHandler(async (req, res, next) => {
  try {
    const token = req.cookies?.accessToken || req.header("Authorization")?.replace("Bearer ", "");
    if (!token) {
      throw new ApiError(401, "Unauthorized request");
    }

    const decodedToken = jwt.verify(token, process.env.JWT_SECRET);
    const user = await User.findById(decodedToken?._id).select("-password");
    if (!user) {
      throw new ApiError(401, "Invalid JWT Token");
    }

    req.user = user;
    next();
  } catch (error) {
    throw new ApiError(401, error?.message || "Invalid JWT Token");
  }
});
```