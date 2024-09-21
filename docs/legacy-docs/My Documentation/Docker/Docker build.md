```
docker build
 --build-arg PORT=8080
 --build-arg CORS_ORIGIN=*
 --build-arg MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.<project-id>.mongodb.net
 --build-arg CLOUDINARY_CLOUD_NAME=<project-id>
 --build-arg CLOUDINARY_API_KEY=<api-key>
 --build-arg CLOUDINARY_API_SECRET=<api-secret>
 --build-arg GOOGLE_CLIENT_ID=<client-id>
 --build-arg GOOGLE_CLIENT_SECRET=<client-secret>
 --build-arg GOOGLE_CALLBACK_URL=http://localhost:8000/auth/google/callback
 --build-arg JWT_SECRET="harshsharma20503123123123123123131321231"
 --build-arg VITE_LOCALHOST=http://localhost:8000
 -t testimage . ; docker run -p 8080:8080 -p 5173:5173 testimage
```

where this is the build without env file
```
docker build -t testimage .
docker run -p 8080:8080 -p 5173:5173 testimage
```


```
sudo docker-compose up
sudo docker-compose down --rmi all
```