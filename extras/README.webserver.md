## SSL Configuration

### Setting Up HTTPS with Let's Encrypt

#### 1. Configure Nginx Server Block
Create a new server block configuration:

```bash
sudo nano /etc/nginx/sites-available/landing-page
```

Add the following configuration:
```nginx
server {
    listen 80;
    listen [::]:80;
    server_name your-domain.com www.your-domain.com;
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Create symbolic link and test configuration:
```bash
sudo ln -s /etc/nginx/sites-available/landing-page /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

#### 3. Verify Server Status
Ensure that you verify the server status after creating the symbolic link and reloading nginx.

#### 4. Include Certificate Installation
Update your server configuration to include SSL settings. The new configuration should look similar to:

```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name your-domain.com www.your-domain.com;
    
    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;
    
    # Enable HSTS
    add_header Strict-Transport-Security "max-age=31536000" always;
    
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name your-domain.com www.your-domain.com;
    return 301 https://$server_name$request_uri;
}
```

#### 5. Set Up Auto-Renewal
Certbot automatically installs a renewal timer. Verify it's active:

```bash
# Check timer status
sudo systemctl status certbot.timer

# Test automatic renewal
sudo certbot renew --dry-run
```

Certificates automatically renew if the Certbot timer is running. If you'd like to manually renewal, you can run the following command:
```bash
sudo certbot renew
```

After doing all of the above, I'd recommend that you reload nginx to confirm that your server is working as expected. 

