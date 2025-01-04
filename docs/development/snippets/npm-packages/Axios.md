# Axios Snippet

## Installation

```console
npm install axios
```

---

## Basic Code

```javascript
const getUser = async () => {
  try {
    const response = await axios.get('/user?ID=12345');
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

---

## Set default URL

- In the starting file of react application write the following

```javascript
if (import.meta.env.DEV) { //for vite application
    console.log("Running in development mode");
    axios.defaults.baseURL = import.meta.env.VITE_LOCALHOST;
} else {
    console.log("Running in production mode");
    axios.defaults.baseURL = import.meta.env.VITE_SERVER_URL;
}
```

Reference Link -> [Vitejs.dev](https://vitejs.dev/guide/env-and-mode)

You do not require to create .Dev environment variables in .env, it is default for vite application
For other react application you can use  following :

```javascript
if (process.env.NODE_ENV === "development") {
    console.log("Running in development mode");
    axios.defaults.baseURL = process.env.REACT_APP_LOCALHOST;
} else {
    console.log("Running in production mode");
    axios.defaults.baseURL = process.env.REACT_APP_SERVER_URL;
}
```

---

## Set protected call function

### Method 1 - When backend returns a message

```javascript
import axios from "axios";
import Cookies from "js-cookie";

async function ApiCall(url, httpMethod, data) {
    const token = Cookies.get("studentsToken") || Cookies.get("professorsToken");
    try {
        const config = {
            headers: {
                Authorization: `Bearer ${token}`,
            },
        };
        if (httpMethod === "GET") {
            const response = await axios.get(url, config);
            if (response.data.message === "Invalid token" || response.data.message === "No Token Provided") {
                // remove Cookies if used
                window.location.href = "/";
            }
            return response;
        } else if (httpMethod === "POST") {
            const response = await axios.post(url, data, config);
            if (response.data.message === "Invalid token" || response.data.message === "No Token Provided") {
                // remove Cookies if used
                window.location.href = "/";
            }
            return response;
        }
    } catch (error) {
        console.error("Error in API call:", error);
        throw error;
    }
}

export default ApiCall;
```

### Method 2 - When Backend return status code

```javascript
import { toast } from "react-toastify";
import axios from "axios";

const ApiCall = async (url, method, navigate, data) => {
  console.log("******** Inside ApiCall function ********");

  if (method === "GET") {
    try {
      const response = await axios.get(url);
      console.log(response.data);
      return response.data;
    } catch (error) {
      console.error("Error in API call:", error);
      if (error.response.status === 401) {
        toast.error("You are not authorized to access this page. Please login first.");
        navigate("/login");
      } else if (error.response.status === 404) {
        toast.error("The requested resource was not found.");
        navigate("/");
      } else if (error.response.status === 500) {
        toast.error("Server Error. Please try again later.");
        navigate("/");
      } else {
        toast.error("An error occurred. Please try again later.");
        navigate("/");
      }
    }
  } else if (method === "POST") {
    try {
      const response = await axios.post(url, data);
      console.log(response.data);
      return response.data;
    } catch (error) {
      console.error("Error in API call:", error);
      if (error.response.status === 401) {
        toast.error("You are not authorized to access this page. Please login first.");
        navigate("/login");
      } else if (error.response.status === 404) {
        toast.error("The requested resource was not found.");
        navigate("/");
      } else if (error.response.status === 500) {
        toast.error("Server Error. Please try again later.");
        navigate("/");
      } else {
        toast.error("An error occurred. Please try again later.");
        navigate("/");
      }
    }
  }
};

export default ApiCall;

```

Make axios call using protected function

```javascript
import ApiCall from "../../util/ApiCall";
import { useNavigate } from "react-router-dom";

const Discover = () => {
  const navigate = useNavigate();

  useEffect(() => {
    const getUser = async () => {
      const response = await ApiCall("/user/getDetails", "GET", navigate, null);
      console.log(response.data);
      setUser(response.data.name);
    };
    getUser();
  }, []);

  return <></>;
};

export default Discover;
```

### Send post request with token

```javascript
const config = {
                headers: {
                    Authorization: `Bearer ${
                        JSON.parse(localStorage.getItem("userInfo")).jwt
                    }`,
                },
            };
            const res = await axios.post(
                "/admin/sendTicketafterVerification/",
                {},
                config
            );
            console.log("After Sending Ticket", res);
```

### Send get request with token

```javascript
const response = await axios.get(
                `/admin/verify/${props.details._id}`,
                config
            );
```

## How to show uploading of file in frontend

Yes, you can add a feature to show the upload percentage of the file using Axios's `onUploadProgress` event. This event allows you to track the progress of the upload and update the state accordingly. Here's how you can do it:

### Updated Frontend Code with Upload Progress

**FileUpload.js:**

```javascript
import React, { useState } from 'react';
import axios from 'axios';

const FileUpload = () => {
  const [file, setFile] = useState(null);
  const [name, setName] = useState('');
  const [message, setMessage] = useState('');
  const [uploadPercentage, setUploadPercentage] = useState(0);

  const handleFileChange = (e) => {
    setFile(e.target.files[0]);
  };

  const handleNameChange = (e) => {
    setName(e.target.value);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const formData = new FormData();
    formData.append('file', file);
    formData.append('name', name);

    try {
      const res = await axios.post('http://localhost:5000/upload', formData, {
        onUploadProgress: (progressEvent) => {
          const { loaded, total } = progressEvent;
          const percent = Math.round((loaded * 100) / total);
          setUploadPercentage(percent);
        },
      });

      setMessage(res.data.message);
      setUploadPercentage(0); // Reset progress bar on successful upload
    } catch (err) {
      console.error(err);
      setMessage('Upload failed');
      setUploadPercentage(0); // Reset progress bar on error
    }
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input type="text" placeholder="Enter name" value={name} onChange={handleNameChange} />
        <input type="file" onChange={handleFileChange} />
        <button type="submit">Upload</button>
      </form>
      {uploadPercentage > 0 && (
        <div>
          <progress value={uploadPercentage} max="100">{uploadPercentage}%</progress>
          <span>{uploadPercentage}%</span>
        </div>
      )}
      <p>{message}</p>
    </div>
  );
};

export default FileUpload;
```

### Explanation

1. **State Management:**
   - `uploadPercentage`: State to keep track of the upload percentage.

2. **Event Handlers:**
   - `handleFileChange`: Updates the file state when a file is selected.
   - `handleNameChange`: Updates the name state when the input value changes.
   - `handleSubmit`: Handles form submission, sets up the `FormData`, and makes the Axios request with `onUploadProgress`.

3. **Axios Request:**
   - The `onUploadProgress` event is used to track the upload progress. It receives a `progressEvent` object, from which you can extract the `loaded` and `total` bytes.
   - Calculate the upload percentage and update the `uploadPercentage` state.

4. **Render:**
   - Displays a progress bar and percentage text if `uploadPercentage` is greater than 0.
   - Shows the response message or an error message after the upload attempt.

### Backend Code (unchanged)

**server.js:**

```javascript
const express = require('express');
const multer = require('multer');
const path = require('path');

const app = express();
const PORT = 5000;

// Set up storage engine
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    cb(null, `${Date.now()}-${file.originalname}`);
  },
});

// Initialize upload
const upload = multer({ storage });

// Middleware to handle file upload and name
app.post('/upload', upload.single('file'), (req, res) => {
  try {
    const { name } = req.body;
    console.log(req.file); // File information
    console.log(name); // Name information
    res.status(200).json({ message: 'File and name uploaded successfully', file: req.file, name });
  } catch (error) {
    console.error(error);
    res.status(500).json({ message: 'Upload failed', error });
  }
});

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

### Summary

- The frontend code now includes an upload progress bar that updates in real-time using Axios's `onUploadProgress` event.
- The progress bar and percentage are displayed during the file upload process, providing feedback to the user.

With this implementation, users will see a progress bar indicating the percentage of the file that has been uploaded, enhancing the user experience.
