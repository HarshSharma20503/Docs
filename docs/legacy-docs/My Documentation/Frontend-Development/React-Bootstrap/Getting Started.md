>[!SUMMARY]- Table of Contents
>- [[Getting Started#Installation|Installation]]
>- [[Getting Started#React-Bootstrap Components|React-Bootstrap Components]]
# Installation

Install package

```console
npm i react-bootstrap bootstrap
```

Import CSS file in the main.jsx

```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";
import "./index.css";
import "bootstrap/dist/css/bootstrap.min.css";

ReactDOM.createRoot(document.getElementById("root")).render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
);
```

Now you can start using react-bootstrap components.

---
# React-Bootstrap Components
[[Navbar]]
[Footer](./Footer)
