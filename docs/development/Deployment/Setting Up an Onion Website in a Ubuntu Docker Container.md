# Setting Up an Onion Website in a Ubuntu Docker Container

## **Step 1: Install Docker**
If Docker is not installed, install it using:

- **Windows/Mac:** Download Docker Desktop from [Docker’s official website](https://www.docker.com/products/docker-desktop).
- **Linux (Ubuntu/Debian):**
  ```bash
  sudo apt update
  sudo apt install docker.io -y
  sudo systemctl enable --now docker
  ```

## **Step 2: Run an Ubuntu Docker Container**

```bash
docker pull ubuntu:latest
docker run -it --name tor-hidden-service -p 8080:8080 ubuntu
```
- `--name tor-hidden-service`: Names the container.
- `-p 8080:8080`: Maps container's web server to the local machine.

## **Step 3: Install Required Packages Inside the Container**

```bash
apt update && apt install tor nginx nano -y
```

## **Step 4: Configure Tor for a Hidden Service**

```bash
nano /etc/tor/torrc
```

Add the following lines at the bottom:

```ini
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:8080
```

Save and exit (Press `CTRL+X`, then `Y`, then `Enter`).

Restart Tor:
```bash
service tor restart
```

## **Step 5: Get Your .onion Address**

```bash
cat /var/lib/tor/hidden_service/hostname
```
Example output:
```
abcxyz123456789.onion
```

## **Step 6: Set Up a Web Server**

Modify the Nginx configuration:
```bash
nano /etc/nginx/sites-available/default
```
Change the content to:
```ini
server {
    listen 8080;
    server_name localhost;
    root /var/www/html;
    index index.html;
}
```
Save and exit.

Restart Nginx:
```bash
service nginx restart
```

## **Step 7: Create a Simple Web Page**

```bash
echo "<h1>Welcome to my Onion Website</h1>" > /var/www/html/index.html
service nginx restart
```

## **Step 8: Test Your Onion Website**

1. Open **Tor Browser**.
2. Visit `abcxyz123456789.onion`.

## **Step 9: Manage the Docker Container**

To exit but keep the container running:
```bash
CTRL + P + Q
```

To stop the container:
```bash
docker stop tor-hidden-service
```

To restart it:
```bash
docker start -ai tor-hidden-service
```

### **Congratulations! Your Onion Website is Live! 🎉**