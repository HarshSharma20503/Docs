
1. Download the script `fullstack_template.sh`
2. Give the execute right to the script `chmod +x fullstack_template.sh`
3. run the script `./fullstack_template.sh`

```.env
# for port
PORT = 8000
SERVER_URL = http://localhost:8000

# for sending mail
EMAIL_ID = <your-email>
APP_PASSWORD = <your-app-password>

# for cross origin resource sharing
CORS_ORIGIN = http://localhost:5173

# for mongodb connection
MONGODB_URI = mongodb+srv://<username>:<password>@cluster0.<>.mongodb.net/mern_template

# for jwt secret
JWT_SECRET = example1234
```