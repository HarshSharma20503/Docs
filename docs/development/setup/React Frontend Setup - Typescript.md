# Typescript React Frontend Setup

## Installing React using vite bundler

Create a folder in which you want to create your project. Write following command in it.

```console
npm create vite@latest
```

Answer all the question related to it.

```console
 Project name: … <project-name>
✔ Package name: … <package-name>
✔ Select a framework: › React
✔ Select a variant: › Typescript

Scaffolding project in /home/../..

Done. Now run:
  cd <project-name>
  npm install
  npm run dev
```

Run $npm \space install$ to install node modules.

```console
npm install
```

Now to run the file write following command in terminal

```console
npm run dev
```

---

## Removing Unnecessary files

Remove the following files

- public/vite.svg
- assets folder

Remove the icon from index.html and the extra meta tags and put your own meta tags like icon, title and description.

Clear the app.css, index.css and app.jsx file. Write rafce (used to create react arrow functional component) and start making changes.

The index.html will look something like this

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <!-- Favicon -->
    <link rel="icon" type="image/svg+xml" href="/path/to/favicon.svg" />
    <!-- Responsive design for mobile devices -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- Page title -->
    <title>Generic Website Title</title>
    <!-- Page description for SEO -->
    <meta
      name="description"
      content="This is a generic description for your website, helping improve SEO by describing the content of your page."
    />
    <!-- Keywords for SEO -->
    <meta
      name="keywords"
      content="generic, keywords, website, SEO, description"
    />

    <!-- Open Graph Meta Tags for social sharing -->
    <meta
      property="og:title"
      content="Generic Open Graph Title"
    />
    <meta
      property="og:description"
      content="This is a generic description for Open Graph, used when sharing the website on social platforms."
    />
    <meta
      property="og:image"
      content="https://example.com/path/to/og-image.jpg"
    />
    <meta property="og:url" content="https://example.com" />

    <!-- Twitter Card Meta Tags for sharing on Twitter -->
    <meta name="twitter:card" content="summary_large_image" />
    <meta
      name="twitter:title"
      content="Generic Twitter Title"
    />
    <meta
      name="twitter:description"
      content="This is a generic description for Twitter Card, used when sharing the website on Twitter."
    />
    <meta
      name="twitter:image"
      content="https://example.com/path/to/twitter-image.jpg"
    />

    <!-- Compatibility for older IE versions -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  </head>
  <body>
    <!-- Root element for app -->
    <div id="root"></div>
    <!-- Main script (app entry point) -->
    <script type="module" src="/path/to/main.js"></script>
  </body>
</html>
```

---

## Setting up folders and files

- Create a $components$ folder inside the src to store the components.
- Create a $pages$ folder inside the src to store the pages files.
- Create a $utils$ folder inside the src to store the utility functions.

Create .prettierrc and add the configurations

```json
{
  "singleQuote": false,
  "bracketSpacing": true,
  "tabWidth": 4,
  "trailingComma": "es5",
  "semi": true
}
```

Create .prettierignore and add the configurations.
You can also use prettierignore generator to generate it.

```bash
*.env
.env
.env.*
/.vscode
/node_modules
./dist
```

## Create your APICall Util function

1. Install `Axios` package

    ```bash
    npm install axios
    ```

2. Create a ApiCall.ts file in the utils folder and paste the following code

    ```typescript
    import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from "axios";

    // Create an Axios instance
    const axiosInstance: AxiosInstance = axios.create({
      baseURL: import.meta.env.VITE_SERVER_URL, // Base URL for requests
      timeout: 10000, // Timeout for requests
      headers: {
        "Content-Type": "application/json", // Default content type
      },
    });

    // Request interceptor to include token
    axiosInstance.interceptors.request.use(
      (config) => {
        const token = localStorage.getItem("jwtToken");
        if (token) {
          config.headers["Authorization"] = `Bearer ${token}`;
        }
        return config;
      },
      (error) => Promise.reject(error) // Handle request error
    );

    // Response interceptor for global error handling
    axiosInstance.interceptors.response.use(
      (response) => response, // Return response as is
      (error) => {
        console.error(
          "Global error handler:",
          error.response?.data || error.message
        );
        return Promise.reject(error); // Pass error to the catch block
      }
    );

    interface ApiCallOptions {
      url: string;
      method: "GET" | "POST" | "PUT" | "DELETE" | "PATCH";
      data?: any;
      headers?: Record<string, string>;
    }

    // Function to handle API errors
    const handleApiError = (error: any) => {
      if (error.response) {
        console.error("API Error:", error.response.data);
        switch (error.response.status) {
          case 401:
            console.error("Unauthorized access. Please log in again.");
            break;
          case 404:
            console.error("Requested resource not found.");
            break;
          case 500:
            console.error("Server error. Please try again later.");
            break;
          default:
            console.error("An unexpected error occurred.");
        }
      } else if (error.request) {
        console.error("No response received from the server.");
      } else {
        console.error("Error setting up the request:", error.message);
      }
    };

    // API call function
    export const apiCall = async <T>({
      url,
      method,
      data,
      headers = {},
    }: ApiCallOptions): Promise<T | null> => {
      try {
        const config: AxiosRequestConfig = {
          url,
          method,
          headers: {
            ...headers,
            // Default headers if necessary
          },
          data,
        };

        const response: AxiosResponse<T> = await axiosInstance(config);
        return response.data ?? null; // Handle null or undefined data
      } catch (error) {
        handleApiError(error); // Handle errors using the internal error handler
        return null;
      }
    };
    ```

3. How to use that apicall function

    ```typescript
    interface User {
      id: number;
      name: string;
      email: string;
    }

    // Define the API endpoint and request options
    const fetchUserData = async (userId: number) => {
      try {
        const result = await apiCall<User>({
          url: `/users/${userId}`,
          method: "GET",
        });

        if (result) {
          console.log("User Data:", result);
        } else {
          console.log("No data returned from the API.");
        }
      } catch (error) {
        console.error("Error fetching user data:", error);
      }
    };

    // Call the function to fetch user data
    fetchUserData(1); // Replace 1 with the actual user ID you want to fetch
    ```

## Setup Different Component libraries or styling mechanism

1. Some basic css stuff in index.css

    ```CSS
    :root {
      --primary-color: #3498db;
      --secondary-color: #2ecc71;
      --background-color: #f4f4f4;
      --text-color: #333;
      --header-footer-bg: #333;
      --card-bg: #fff;
      --border-color: #ddd;
      --button-hover-bg: #2980b9;
      --font-family: "Arial", sans-serif;
      --font-size: 16px;
      --line-height: 1.6;
    }

    /* Global Styles */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    html,
    body {
      font-family: var(--font-family);
      line-height: var(--line-height);
      color: var(--text-color);
      background-color: var(--background-color);
      height: 100%; /* Ensures body takes full height */
    }
    ```

### Different UI Libraries

- [React Bootstrap](../../learning/UI-Libraries-Frontend-Styling/React%20Bootstrap)
- [ShadCN](https://ui.shadcn.com/docs/installation/vite)

## Setup Storage

- [Context API - Hitesh Chaudhary (Youtube)](../../learning/react/Context%20API%20-%20Hitesh%20Chaudhary%20(Youtube).md)
