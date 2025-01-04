>[!SUMMARY]- Table of Contents
>- [[Handle File upload#Installation|Installation]]
>- [[Handle File upload#Concept|Concept]]
>- [[Handle File upload#Multer Setup|Multer Setup]]
>    - [[Handle File upload#Create multer.middleware.js file in middlewares|Create multer.middleware.js file in middlewares]]
>    - [[Handle File upload#User Multer upload|User Multer upload]]
>- [[Handle File upload#Cloudinary Setup|Cloudinary Setup]]
>    - [[Handle File upload#Create Cloudinary.js file in util|Create Cloudinary.js file in util]]
>    - [[Handle File upload#Insert the keys in .env file|Insert the keys in .env file]]
>    - [[Handle File upload#Use uploadOnCloudinary Function|Use uploadOnCloudinary Function]]

# Installation

Install cloudinary and multer

```javascript
npm i cloudinary multer
```

---
# Concept

We will be first uploading the file to a temp folder inside the public folder to save the file on server temporarily and then from there we will upload the file to cloudinary.
So you need to create multer middleware which stores the file in the temp folder and a cloudinary function which takes the file from the temp folder and upload it on cloudinary.

---
# Multer Setup
## Create multer.middleware.js file in middlewares

Write the following code

```javascript
import multer from "multer";

const storage = multer.diskStorage({
    destination: function (req, file, cb) {
        cb(null, "./public/temp");
    },
    filename: function (req, file, cb) {
        cb(null, `${Date.now()}-${file.originalname}`);
    },
});

export const upload = multer({ storage });
```

This module sets up a multer middleware configuration for handling file uploads. When integrated into a route or endpoint in a Node.js application, it allows users to upload files, which will be stored in the "./public/temp" directory with unique filenames based on the current timestamp and the original filename. This is a common setup for handling file uploads in web applications.

for more knowledge -> [Multer docs](https://github.com/expressjs/multer)

## User Multer upload

```javascript
import { Router } from "express";
import { registerUser } from "../controllers/user.controller.js";
import { upload } from "../middlewares/multer.middleware.js";

const router = Router();

router.route("/register").post(
    upload.fields([
        { name: "avatar", maxCount: 1 },
        { name: "coverImage", maxCount: 1 },
    ]),
    registerUser
);

export default router;
```

---

# Cloudinary Setup

## Create Cloudinary.js file in util

write the following code

```javascript
import { v2 as cloudinary } from "cloudinary";
import fs from "fs";
import dotenv from "dotenv";
dotenv.config();

cloudinary.config({
    cloud_name: `${process.env.CLOUDINARY_CLOUD_NAME}`,
    api_key: `${process.env.CLOUDINARY_API_KEY}`,
    api_secret: `${process.env.CLOUDINARY_API_SECRET}`,
});

const uploadOnCloudinary = async (localFilePath) => {
    try {
        if (!localFilePath) {
            throw new Error("File path is required");
        }
        const response = await cloudinary.uploader.upload(localFilePath, {
            resource_type: "auto",
        });
        console.log("File uploaded successfully on Cloudinary", response.url);
        fs.unlinkSync(localFilePath); // remove the locally saved temporary file as the upload operation got successfull
        return response.url;
    } catch (error) {
        console.log("Error inside Cloudinary upload function: ", error);
        fs.unlinkSync(localFilePath); // remove the locally saved temporary file as the upload operation got failed
        return null;
    }
};

export { uploadOnCloudinary };
```

This module provides a convenient function for uploading files to Cloudinary, handling errors gracefully, and logging the success message upon a successful upload. It's commonly used in web applications to manage and serve media files in the cloud.

## Insert the keys in .env file

```javascript
CLOUDINARY_CLOUD_NAME = 
CLOUDINARY_API_KEY = 
CLOUDINARY_API_SECRET = 
```

insert your own new keys from cloudinary

for more knowledge -> [Cloudinary docs](https://cloudinary.com/documentation/node_quickstart)

## Use uploadOnCloudinary Function

```javascript
import { uploadOnCloudinary } from "../utils/cloudinary.js";

const registerUser = asyncHandler(async (req, res) => {
    ...
    const avatarLocalPath = req.files?.avatar[0]?.path;
    const coverImageLocalPath = req.files?.coverImage[0]?.path;
    if (!avatarLocalPath) {
        throw new ApiError(400, "Avatar file is required");
    }

    const avatar = await uploadOnCloudinary(avatarLocalPath);
    const coverImage = await uploadOnCloudinary(coverImageLocalPath);

    if (!avatar) {
        throw new ApiError(400, "Avatar file is required");
    }
    ...
});

export { registerUser };
```

---

# Call from frontend

```javascript
<Input type="file" p={1.5} accept="image/*" onChange={(e) => postDetails(e.target.files[0])} />


const postDetails = async (pics) => {
      ...
      const data = new FormData();
      data.append("avatar", pics);
      console.log("Data: ", data);
      try {
        const response = await axios.post("http://localhost:5000/api/auth/uploadPic", data);
        setPic(response.data.data.url);
        console.log("Pic url:", response.data.data.url);
	  ...
    }
  };
```