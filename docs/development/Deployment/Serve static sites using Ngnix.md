# Nginx Server Setup and Configuration Guide

This guide provides step-by-step instructions for setting up and configuring an Nginx web server on a Virtual Machine.

## Prerequisites
- Access to a Virtual Machine
- SSH access to the VM
- Administrative (sudo) privileges

## Installation and Basic Setup

### 1. Install Nginx
First, update your package list and install Nginx:
```bash
sudo apt update && sudo apt install -y nginx
```

### 2. Configure Nginx

#### 2.1 Open the default configuration file:
```bash
nano /etc/nginx/sites-enabled/default
```

#### 2.2 Replace the existing configuration with:
```nginx
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    # Use a dedicated web directory
    root /var/www/html;
    
    index index.html;
    
    server_name _;
    
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

This configuration:
- Listens on port 80 (HTTP)
- Sets the root directory to `/var/www/html`
- Uses `index.html` as the default page
- Implements a basic URL fallback mechanism

## Website Content Setup

### 1. Create Web Directory and HTML File
```bash
sudo mkdir -p /var/www/html
sudo nano /var/www/html/index.html
```

### 2. Add Basic HTML Content
Create a simple HTML file with the following content:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome</title>
</head>
<body>
    <h1>Welcome to my site!</h1>
</body>
</html>
```

## Security and Permissions

### 1. Set Correct Ownership
Assign the appropriate user and group ownership:
```bash
sudo chown -R www-data:www-data /var/www/html
```

### 2. Set File Permissions
Set the correct file permissions for the web directory:
```bash
sudo chmod -R 755 /var/www/html
```

### 3. Verify Permissions
Confirm the file permissions are set correctly:
```bash
ls -l /var/www/html/index.html
```

## Server Management

### 1. Restart Nginx
After making configuration changes, restart the Nginx service:
```bash
sudo systemctl restart nginx
```

### 2. Troubleshooting
If you encounter issues, check the Nginx error logs:
```bash
sudo tail -f /var/log/nginx/error.log
```

## Verification
After completing all steps, verify your setup by:
1. Accessing your server's IP address or domain name in a web browser
2. Checking that the welcome page loads correctly
3. Reviewing error logs if any issues occur

## Common Issues and Solutions
- If the page doesn't load, verify that:
  - Nginx service is running
  - Firewall allows traffic on port 80
  - File permissions are set correctly
  - Configuration syntax is correct
  - Error logs show no critical issues

## Additional Resources
- Nginx official documentation
- Ubuntu server guide
- Linux permissions documentation

## Security Notes
Remember to:
- Regularly update your system
- Configure SSL/TLS for production use
- Implement proper security measures
- Back up your configuration files