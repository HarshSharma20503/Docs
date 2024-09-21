>[!SUMMARY]- Table of Contents
>- [[Google Oauth 2.0 + JWT#Step 1 - Create Project in Google Console|Step 1 - Create Project in Google Console]]
>- [[Google Oauth 2.0 + JWT#Step 2 - Implementation code|Step 2 - Implementation code]]
# Step 1 - Create Project in Google Console
Create a project in google console
- Follow the link and create a project [Google Console](https://console.cloud.google.com/)
- From the top navigation menu click on API and services and than Oauth Screen (You are allowed only one Oauth screen per project).
- Create the Oauth Screen which will require you to fill the following details:
	- App Name : Name of the App e.g. `SkillSwap`
	- User Support Email : For users to contact you with questions about their consent e.g. `harshsharma20503@gmail.com`
	- App Logo: any app logo
	- App domain : terms and services link etc (You can leave empty)
	- Authorised domain:   domain is used on the consent screen e.g. `localhost.com` in development and another domain `yourCustomDomain.com`
	- Move on to the next step
	- In add Scopes select userinfo.email, userinfo.profile and openid
- You also have to select test users 
- In the From the top navigation menu click on API and services and than Credentials.
	- Click on Create Credentials
	- Select Application type
	- Authorised JavaScript Origins : e.g. `http://localhost:5173`
	- Authorised redirect URIs : e.g. `http://localhost:8080/auth/google` This is the backend route where you want the authorisation to redirect to.
	- Click on Create

# Step 2 - Implementation code
Create a project and initialise express server
- Inside app.js
```javascript
//imports
import passport from "passport";
//passport middleware
app.use(passport.initialize());
//routes
app.use("/auth", authRouter);
```

- Inside auth.routes.js
```javascript
import { Router } from "express";
import { googleAuthCallback, googleAuthHandler, handleGoogleLoginCallback } from "../controllers/auth.controllers.js";

const router = Router();

router.get("/google", googleAuthHandler);
router.get("/google/callback", googleAuthCallback, handleGoogleLoginCallback);

export default router;
```

- inside auth.controller.js
```javascript
import generateJWTToken from "../utils/generateJWTToken.js";
import passport from "passport";
import { Strategy as GoogleStrategy } from "passport-google-oauth20";
import { User } from "../models/user.model.js";
import dotenv from "dotenv";

dotenv.config();

passport.use(
  new GoogleStrategy(
    {
      clientID: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
      callbackURL: "/auth/google/callback",
    },
    async (accessToken, refreshToken, profile, done) => {
      done(null, profile);
    }
  )
);

export const googleAuthHandler = passport.authenticate("google", { scope: ["profile", "email"] });

export const googleAuthCallback = passport.authenticate("google", {
  failureRedirect: "http://localhost:5173/login",
  session: false,
});

export const handleGoogleLoginCallback = async (req, res) => {
// here you handle the things you want to do after authentication
  const existingUser = await User.findOne({ email: req.user._json.email });

  if (!existingUser) {
    await User.create({
      email: req.user._json.email,
      name: req.user._json.name,
    });
  }

  const jwtToken = generateJWTToken(req.user._json);
  const expiryDate = new Date(Date.now() + 24 * 60 * 60 * 1000);
  res.cookie("accessToken", jwtToken, { httpOnly: true, expires: expiryDate, secure: false });
  res.redirect("http://localhost:5173/");
};

```

This is a way in which you get the google authentication and then use jwt token to verify each and every subsequent request using the jwt middleware.
