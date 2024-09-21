https://www.youtube.com/watch?v=NrO0W0rqFB0
# Steps

##### Google Console Part
1. Go to the console.cloud.google.com and create a project
2. Go to the left navigation and click on APIs & services and than Credentials
3. On the top panel click on the create credentials than Oauth client ID
4. Than configure consent screen
5. Choose External users and than click on create.
6. Enter app name and developer information
7. if this error comes `The request has been classified as abusive and was not allowed to proceed` than modify the app name and save again e.g. name QuickStart.
8. Add the scope to email, profile and also the openid and than click on update than save, no need to write the test users.
9. Now go back to create credentials same way and select the type of the application you are making.
10. Write the allowed java script origin request to both domain of your frontend and backend. e.g. `Authorised Javascript Origin` - http://localhost:5173 (or the deployed site link) 
	 `Authorised redirect URIs` - http://localhost:8000/auth/google/callback (or the deployed backend link)

##### Now Moving to the coding part
1. Install the following dependencies inside your backend 
```console
npm install passport express-session passport-google-oauth20
```
2. Now go to the file where you have initialised your express app.
3. Initialise the passport strategy
```javascript
passport.use(...);
passport.serializeUser(...);
passport.deserializeUser(...);
```
4. Initialise the middlewares to use sessions and passport
```javascript
app.use(session(...));
app.use(passport.initialise());
app.use(passport.session());
```
5. Now write the route that you will hit from frontend when someone click on the signup/login with google button
```javascript
.get('/auth/google'app, passport.authenticate('google', {scope: ['profile']}));
```
6. Now write the callback url that will be used to called when the authentication is success.
```javascript
app.get('/auth/google/callback', passport.authenticate('google', {failureRedirect : '/'}), (req, res) => {
	res.redirect('/profile');
})

app.get('/profile', (req, res)=>{
	if(req.isAuthenticated())
	{
		Send whatever data you want
	} else {
		res.redirect('/')
	}
})
```
7. Create logout route
```javascript
app.get('/logout', (req, res)=>{
	req.logout();
	req.session.destroy((err) => {
		if(err) {
			console.log(err);
			return res.status(500).send("internal server error);
		}
		res.clearCookie('connect.sid');
		res.redirect('/');
	})
})
```
8. Now in the frontend when the button of the google sign up is or login is clicked write the following code.
```javascript
onClick((e)=>{
	windows.location.href("http://localhost:8000/auth/google");
})
```