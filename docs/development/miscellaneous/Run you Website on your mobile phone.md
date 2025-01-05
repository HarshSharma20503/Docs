# Accessing Localhost on Mobile Devices

If you’re developing a web application and need to test it on your mobile device, accessing your local server (localhost) can be tricky. To do that follow these steps:

`Note`: This is only for MERN stack, but the core concept of accessing through ip address will remain same, sometimes  firewalls or using VPN may cause error in this procedure.

## Step 1: Find Your Computer’s Local IP Address

To access your local server, you’ll need your computer’s local IP address:

- **Windows**:

1. Open Command Prompt and run:

    ```bash
    ipconfig
    ```

2. Note the IPv4 address (e.g., 192.168.x.x).

- **Mac/Linux**:

1. Open Terminal and run:

    ```bash
    ifconfig
    ```

   (or ip a on Linux).
2. Look for the IPv4 address under your active network interface.

## Step 2: Configure Vite to Allow External Connections

By default, Vite only allows connections from localhost. You need to update its settings:

- Update the `vite.config.js` file to bind it to all network interfaces:

```javascript
export default {
  server: {
    host: '0.0.0.0',
  },
};
```

if vite.config.js already exists then  it might look something like this after adding server property.

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    host: "0.0.0.0",
  },
});
```

## Step 3: Adjust Your Network Settings

Usually if you are using your hotspot or home wifi, this problem will not occur.

- **Firewall**: Ensure your computer’s firewall allows traffic on port 5173 (or whatever port you are working on).
- **Network**: Confirm that your mobile and computer are connected to the same Wi-Fi network.

## Step 4: Access Your Vite App on Mobile

1. Open a browser on your mobile device.
2. Enter your computer’s IP address and port 5173:

```bash
http://<IP_ADDRESS>:5173
```

Example: `http://192.168.1.100:5173`

- **Restart Server**: Restart Vite after changing its configuration.
- **Disable VPN/Proxy**: Ensure no VPN or proxy is blocking the connection.
