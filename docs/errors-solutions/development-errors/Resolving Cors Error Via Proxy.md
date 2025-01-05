# Resolving Cors Error Via Proxy

Cors Errors can be resolved via creating a proxy in frontend. If you have vite react project, vite has a inbuilt proxy feature. Just go to the `vite.config.js` and paste the following:

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      "/api": {
        target: "http://codecolosseum.westus2.cloudapp.azure.com",
        changeOrigin: true, // Set to true to avoid host-related CORS issues
        rewrite: (path) => path.replace(/^\/api/, ""), // Rewrite `/api` if necessary
        secure: false, // Set to false if using self-signed certificates
      },
    },
  },
});

```

target should be the same domain as that of backend, this proxy targets only the `/api` routes, so it will work on them.
for e.g.
```javascript
fetch("/api/health")
      .then((response) => response.json())
      .then((data) => console.log(data))
      .catch((error) => console.error("Error:", error));
```
