When building a web application with React.js, you may need to request a backend server. However, if the backend server does not allow Cross-Origin Resource Sharing (CORS), you may encounter an error when requesting from the frontend application. In this case, you can use the proxy server to forward requests from the front-end application to the back-end server.

# Vite Configuration 

- Go inside vite.config.js
- add server configuration 
```javascript
server:{
	proxy:{
	'/api' : 'https://localhost:3000', 
	}
}
```

This will append 'https://localhost:3000' in front of any api call which is made to the route having /api in it. This will make server believe that the api call is made from 'https://localhost:3000'. 

