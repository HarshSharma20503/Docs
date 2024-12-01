## Steps:
### Step 1: Create a virtual machine 

- Create a virtual machine instance on any cloud service like aws, azure or google cloud.
- Opt for ubuntu OS if possible.
- Get the ip for it.
### Step 2: Configure server

- ssh into the server or open browser shell
- write following commands to update apt and install nginx
 ```bash
 sudo apt update && sudo apt install -y nginx
 ```
 - open the default nginix configuration
```bash
 nano /etc/nginx/sites-enabled/default
```
- write the following in the default configuration, create the upstream and set it in location
```bash
upstream myapp{
    server 127.0.0.1:8000; // your express or flask server route
}

server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		proxy_pass http://myapp; //proxy forwarding here 
	}

}
```
- restart nginix
```bash
sudo systemctl restart nginx
```

### Step 3: Get SSL certificate through certbot

- install certbot
```bash
sudo apt install -y certbot python3-certbot-nginx
```
- Put an A DNS record in your dns management and give a domain to the ip of the server to get the ssl certificate.
- get the ssl certificate through following command, it will ask you to accept some conditions and enter your email.
```bash
sudo certbot --nginx -d your_domain_or_ip
```
- verify ssl certificate
```bash
sudo nginx -t
```
- reload nginx
```bash
sudo systemctl reload nginx
```
- Set Up Automatic Certificate Renewal
	Certbot’s certificates are valid for 90 days, but it installs a cron job for automatic renewal. To manually test renewal, you can run:
```bash
sudo certbot renew --dry-run
```

