>[!SUMMARY]+ Table of Contents
>- [[Connect Database#Set up MongoDB Atlas|Set up MongoDB Atlas]]
>- [[Connect Database#Configure .env|Configure .env]]
>- [[Connect Database#Add database name to Constraints|Add database name to Constraints]]
>- [[Connect Database#Connect to database |Connect to database ]]
>- [[Connect Database#Call connectDB from index.js|Call connectDB from index.js]]
# Set up MongoDB Atlas

- Create account on MongoDB Atlas
- Create New project
- Create a deployment
- Select free tier with cluster name
- put your username and password
- Select Local Environment and add 0.0.0.0/0 IP address so that you give full access.
**Note** - You can change IP address setting from network access, you can add user from database access
- Now click on connect to the cluster you selected
- you can now get the $Connection \space String$ from it.

---
# Configure .env

Put your connection string in you env file and change $<password>$ in it and remove last ' / '.

```
MONGODB_URI = mongodb+srv://<username>:<password>@cluster0.<project-id>.mongodb.net
```

---
# Add database name to Constraints

Add the name of database to constraints file

```
export const DB_NAME = "YoutubeDB"
```
---

# Connect to database 

Create index.js in db folder and connect to database.

```javascript
import mongoose from 'mongoose';
import { DB_NAME } from '../constants.js';

const connectDB = async () => {
	try {
		const connectionInstance = await mongoose.connect(`${process.env.MONGODB_URI}/${DB_NAME}`);
		console.log(`\n MongoDB connected: ${connectionInstance.connection.host} \n`);
	} catch (error) {
		console.log('MongoDB connection error: ', error);
		process.exit(1);
	}
};

export default connectDB;
```

---
# Call connectDB from index.js

```javascript
import dotenv from "dotenv";
import connectDB from "./db/index.js";
import { app } from "./app.js";

dotenv.config({
    path: "./.env",
});

const port = process.env.PORT || 8000;

connectDB()
    .then(() => {
        console.log("Database connected");
        app.listen(port, () => {
            console.log(`Server listening on port ${port}`);
        });
    })
    .catch((err) => {
        console.log(err);
    });
```
---

