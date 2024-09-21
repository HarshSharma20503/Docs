>[!SUMMARY]+ Table of Contents
>- [[Modelling Schema#Installation |Installation ]]
>- [[Modelling Schema#Creating Models Folder|Creating Models Folder]]
>- [[Modelling Schema#Creating Schema|Creating Schema]]
>- [[Modelling Schema#Properties of Schema|Properties of Schema]]
>- [[Modelling Schema#Create reference in Schema|Create reference in Schema]]
>- [[Modelling Schema#Create middleware (Hashing the password)|Create middleware (Hashing the password)]]
>- [[Modelling Schema#Create Schema Methods (Access Token and Refresh Token)|Create Schema Methods (Access Token and Refresh Token)]]
# Installation 

Install mongoose using following command.

```console
npm i mongoose
```

---

# Creating Models Folder

Create a Models folder and save file in a fix format
- models
	- user.model.js
	- video.model.js


---

# Creating Schema

Create schema by using following snippet
```javscript
import mongoose, { Schema } from "mongoose";
const userSchema = new Schema({}, { timestamps: true });
export const User = mongoose.model("User", userSchema);
```

Now add different arguments

```javascript
import mongoose, { Schema } from "mongoose";

const userSchema = new Schema(
    {
        username: {
            type: String,
            required: [true, 'Username is required'],
            unique: true,
            lowercase: true,
            trim: true,
            index: true,
        },
        email: {
            type: String,
            required: [true, 'Email is required'],
            unique: true,
            lowercase: true,
            trim: true,
        },
        fullname: {
            type: String,
            required: [true, 'Fullname is required'],
            trim: true,
            index: true,
        },
        avatar: {
            type: String, //cloudinary url
            default:
                "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
        },
        coverImage: {
            type: String, //cloudinary url
            default:
            "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
        },
        watchHistory: [
            {
                type: Schema.Types.ObjectId,
                ref: "Video",
            },
        ],
        password: {
            type: String,
            required: [true, "Password is required"],
        },
        refreshToken: {
            type: String,
        },
    },
    { timestamps: true }
);

export const User = mongoose.model("User", userSchema);
```

---

# Properties of Schema

| **Property** | **Meaning** | **Example** | Reference Links |
| ---- | ---- | ---- | ---- |
| type | This provides the datatype of the field. | type:String | [More Types](https://mongoosejs.com/docs/schematypes.html) |
| required | Is the field required | required:true |  |
| lowercase | Converts string into lowercase | lowercase:true |  |
| timestamps | provide timestamps to the document | {timestamps:true} | [About timestamps](https://mongoosejs.com/docs/timestamps.html) |
| default | give default values to the fields | default: true |  |
| ref | giving reference of other schemas | ref: Todo |  |
| enum | fix the number of inputs into a set of inputs | enum: ['yes', 'no'] |  |
| trim | remove space in the starting and ending if any | trim:true |  |
| index  | to increase the searching speed | index:true |  |


---

# Create reference in Schema

You can create reference of other schema by using property $'type'$ with value   $'mongoose.Schema.Types.ObjectId'$ and using property $'ref'$ with value of the exported value of the schema you want to refer

```javascipt
import mongoose, { Schema } from "mongoose";

const userSchema = new Schema(
    ...
        watchHistory: [
            {
                type: Schema.Types.ObjectId,
                ref: "Video",
            },
        ],
    ...
);
```

ref: 'User' in this User is the value take from the following line

```javascript
export const User = mongoose.model('User', userSchema); // right side User
```

---
# Create middleware (Hashing the password)

Install bcrypt package for hashing the password 
```
npm i bcrypt
```
Middleware (also called pre and post hooks) are functions which are passed control during execution of asynchronous functions. Middleware is specified on the schema level and is useful for writing plugins.

```javascript
import mongoose, { Schema } from "mongoose";

const userSchema = new Schema(
    {
        username: {
            type: String,
            required: [true, 'Username is required'],
            unique: true,
            lowercase: true,
            trim: true,
            index: true,
        },
        email: {
            type: String,
            required: [true, 'Email is required'],
            unique: true,
            lowercase: true,
            trim: true,
        },
        fullname: {
            type: String,
            required: [true, 'Fullname is required'],
            trim: true,
            index: true,
        },
        avatar: {
            type: String, //cloudinary url
            default:
                "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
        },
        coverImage: {
            type: String, //cloudinary url
            default:
            "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
        },
        watchHistory: [
            {
                type: Schema.Types.ObjectId,
                ref: "Video",
            },
        ],
        password: {
            type: String,
            required: [true, "Password is required"],
        },
        refreshToken: {
            type: String,
        },
    },
    { timestamps: true }
);

userSchema.pre("save", async function (next) {
    // don't use arrow function here, because we need to use this keyword to access the current user document
    if (this.isModified("password")) {
        this.password = await bcrypt.hash(this.password, 8);
    }
    next();
});

export const User = mongoose.model("User", userSchema);
```

This middleware ensures that before saving a user document, it checks if the password has been modified. If so, it hashes the password using bcrypt and then allows the save operation to continue. This is a common pattern for securing passwords in a database by hashing them before they are stored.

For more middlewares can visit -> [Mongoose Docs](https://mongoosejs.com/docs/middleware.html)

---
# Create Schema Methods (Access Token and Refresh Token)

Install jsonwebtoken library
```console
npm install jsonwebtoken
```

```javascript
import mongoose, { Schema } from "mongoose";
import jwt from "jsonwebtoken";
import bcrypt from "bcrypt";
const userSchema = new Schema(
    {
        username: {
            type: String,
            required: true,
            unique: true,
            lowercase: true,
            trim: true,
            index: true,
        },
        email: {
            type: String,
            required: true,
            unique: true,
            lowercase: true,
            trim: true,
        },
        fullname: {
            type: String,
            required: true,
            trim: true,
            index: true,
        },
        avatar: {
            type: String, //cloudinary url
            default:
                "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
        },
        coverImage: {
            type: String, //cloudinary url
            default:
                "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
        },
        watchHistory: [
            {
                type: Schema.Types.ObjectId,
                ref: "Video",
            },
        ],
        password: {
            type: String,
            required: [true, "Password is required"],
        },
        refreshToken: {
            type: String,
        },
    },
    { timestamps: true }
);
userSchema.pre("save", async function (next) {
    // don't use arrow function here, because we need to use this keyword to access the current user document
    if (this.isModified("password")) {
        this.password = await bcrypt.hash(this.password, 8);
    }
    next();
});
userSchema.methods.isPasswordCorrect = async function (password) {
    return await bcrypt.compare(password, this.password);
};

userSchema.methods.generateAccessToken = function () {
    return jwt.sign(
        {
            _id: this._id,
            email: this.email,
            username: this.username,
            fullname: this.fullname,
        },
        process.env.ACCESS_TOKEN_SECRET,
        {
            expiresIn: process.env.ACCESS_TOKEN_EXPIRY,
        }
    );
};

userSchema.methods.generateRefreshToken = function () {
    return jwt.sign(
        {
            _id: this._id,
        },
        process.env.REFRESH_TOKEN_SECRET,
        {
            expiresIn: process.env.REFRESH_TOKEN_EXPIRY,
        }
    );
};

export const User = mongoose.model("User", userSchema);

```

These methods provide a way to generate access and refresh tokens for a user, allowing for secure authentication and token-based authorization in a Node.js application. The secret keys and token expiration times are stored in environment variables (`process.env`) for security and configurability.
for more info -> [JWT npm package docs](https://www.npmjs.com/package/jsonwebtoken)
