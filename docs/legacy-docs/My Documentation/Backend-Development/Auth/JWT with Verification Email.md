# Create Modals

1. unverified user schema
```javascript
import mongoose, { Schema } from "mongoose";
import bcrypt from "bcrypt";

const unverifiedUserSchema = new Schema(
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

unverifiedUserSchema.methods.matchPassword = async function (enteredPassword) {
  return await bcrypt.compare(enteredPassword, this.password);
};

unverifiedUserSchema.pre("save", async function (next) {
  if (!this.isModified("password")) {
    next();
  }
  this.password = await bcrypt.hash(this.password, 12);
});

export const UnverifiedUser = mongoose.model("UnverifiedUser", unverifiedUserSchema);
```

2. user schema
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
    companies: [
      {
        type: Schema.Types.ObjectId,
        ref: "Company",
      },
    ],
  },
  { timestamps: true }
);

userSchema.methods.matchPassword = async function (enteredPassword) {
  console.log("Entered Password:", enteredPassword);
  const temp = await bcrypt.hash(enteredPassword, 12);
  console.log("Temp: ", temp);
  console.log("Password: ", this.password);
  return await bcrypt.compare(enteredPassword, this.password);
};

userSchema.pre("save", async function (next) {
  if (!this.isModified("password") || this.isNew) {
    next();
  }
  this.password = await bcrypt.hash(this.password, 12);
});

export const User = mongoose.model("User", userSchema);
```

# Create Nodemailer file
- inside util create file named send mail.js
```javascript
import nodemailer from "nodemailer";
import dotenv from "dotenv";

dotenv.config();

const transporter = nodemailer.createTransport({
  service: "gmail",
  host: "smtp.gmail.com",
  port: 465,
  auth: {
    user: process.env.EMAIL_ID,
    pass: process.env.APP_PASSWORD,
  },
});

const sendConfirmationMail = async (to, id) => {
  const mailOptions = {
    from: process.env.EMAIL_ID,
    to: [to],
    subject: "Email Verification for MERN Chat App",
    html: `<h1>Click <a href=http://localhost:5000/api/auth/confirmEmail/${id}>here</a> to confirm your email</h1>`,
  };

  try {
    await transporter.sendMail(mailOptions);
    console.log("Verification Email sent succesfully");
    return true;
  } catch (error) {
    console.log("Error while sending verification email", error);
    return false;
  }
};

export { sendConfirmationMail };
```
- Don't forget to put things in .env
# Create Routes
- create auth.routes.js and auth.controller.js with /api/auth/login /api/auth/register etc
# Create Controllers
```javascript
import { asyncHandler } from "../utils/AsyncHandler.js";
import { ApiResponse } from "../utils/ApiResponse.js";
import { ApiError } from "../utils/ApiError.js";
import { User } from "../models/user.model.js";
import { UnverifiedUser } from "../models/unverifiedUser.model.js";
import { generateJWTToken } from "../utils/GenerateToken.js";
import { uploadOnCloudinary } from "../config/cloudinary.js";
import { sendConfirmationMail } from "../utils/sendMail.js";

const registerUser = asyncHandler(async (req, res) => {
  console.log("******** registerUser Function ********");
  console.log("Request Body", req.body);
  const { name, email, password } = req.body;
  console.log("User details", name, email, password);
  if (!name || !email || !password) {
    throw new ApiError(400, "All fields are required");
  }

  const existingUser = await User.findOne({ email });
  console.log("Existing User", existingUser);
  if (existingUser) {
    throw new ApiError(400, "User already exists");
  }

  const existingUnverifiedUser = await UnverifiedUser.findOne({ email });
  console.log("Existing Unverified User", existingUnverifiedUser);
  if (existingUnverifiedUser) {
    throw new ApiError(400, "Already registered. Please verify your email");
  }

  const unverifiedUser = await UnverifiedUser.create({ name, email, password });

  if (!unverifiedUser) {
    throw new ApiError(500, "Failed to create User");
  }

  const confirmationMail = await sendConfirmationMail(email, unverifiedUser._id);

  if (!confirmationMail) {
    await UnverifiedUser.deleteOne({ _id: unverifiedUser._id });
    throw new ApiError(500, "Failed to send confirmation mail");
  }

  return res.status(200).json(
    new ApiResponse(
      200,
      {
        name: unverifiedUser.name,
        email: unverifiedUser.email,
      },
      "Please verify your email to continue"
    )
  );
});

const confirmEmail = asyncHandler(async (req, res) => {
  const { id } = req.params;
  if (!id) {
    throw new ApiError(400, "Invalid request");
  }

  const unverifiedUser = await UnverifiedUser.findById(id);
  if (!unverifiedUser) {
    throw new ApiError(404, "User not found");
  }

  const user = await User.create({
    name: unverifiedUser.name,
    email: unverifiedUser.email,
    password: unverifiedUser.password,
  });

  if (!user) {
    throw new ApiError(500, "Failed to create User");
  }

  await UnverifiedUser.deleteOne({ _id: unverifiedUser._id });

  return res.status(200).send("Email confirmed. You can now login");
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
      new ApiResponse(
        200,
        {
          _id: user._id,
          name: user.name,
          email: user.email,
          token: generateJWTToken(user._id),
        },
        "User logged in successfully"
      )
    );
  }
  throw new ApiError(401, "Invalid email or password");
});

export { uploadPicOnCloudinary, registerUser, confirmEmail, loginUser };

```

# Create generate  JWT token function
[[Generate JWT Token]]

# Create verifyJWT middleware
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


