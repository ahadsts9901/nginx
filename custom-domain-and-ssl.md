# Configure SSL and Custom Domain for Your Website

---

### 🌐 Add a Custom Domain

- Buy a domain from any provider (e.g., GoDaddy, Hostinger, Namecheap).
- Create an **A Record** pointing to your AWS EC2 public IP in your DNS provider's dashboard.

---

### ⚙️ Update `nginx.conf` for Your Custom Domain

- Navigate to the Nginx config file:

  ```bash
  cd /etc/nginx/nginx.conf
  ```

- Modify the file with the following configuration:

  ```nginx
  events {
  }

  http {
      server {
          listen 80;
          server_name mywebsite.com www.mywebsite.com;

          root /usr/share/nginx/path-to-your-website;
          index index.html;

          error_page 404 /404.html;

          location / {
              try_files $uri $uri/ =404;
          }

          access_log /var/log/nginx/mywebsite.com.access.log;
          error_log /var/log/nginx/mywebsite.com.error.log;
      }
  }
  ```

- Replace `mywebsite.com` and `path-to-your-website` with your actual domain and path.

- Save the changes, test your configuration:

  ```bash
  sudo nginx -t
  ```

- Then restart Nginx:

  ```bash
  sudo systemctl restart nginx
  ```

- For nodejs or nextjs app

    ```bash
    location / {
        proxy_pass http://localhost:3000;  # Your Node.js server
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    ```

---

✅ Once your DNS is propagated, you should be able to access your website via your custom domain over HTTP.


# 🌐 Add SSL to your website (http to https setup)

### Install Dependencies

- Install certbot

<!-- `sudo yum install epel-release -y` -->
```bash
sudo yum install certbot python3-certbot-nginx -y
```

- Stop Nginx

```bash
sudo systemctl stop nginx.service
```

- Generate Certificates

```bash
sudo certbot certonly --standalone -d yourdomain.com
```

- If it ask for email so give your email

- Certificate files will be generated in

```bash
/etc/letsencrypt/live/yourdomain.com
```

- Above command will give you 2 paths, copy them and add them in config file

- Update your nginx.conf file with the copied paths, located at:

```bash
/etc/nginx/nginx.conf
```

- Modify the file with the following configuration

```nginx
events {
}

http {
    server {
        listen 443 ssl http2;
        server_name mywebsite.com www.mywebsite.com;

        root /usr/share/nginx/path-to-your-website;
        index index.html;

        ssl_certificate /generated-path/fullchain.pem;
        ssl_certificate_key /generated-path/privkey.pem;

        ssl_protocols TLSv1.2 TLSv1.3;
        error_page 404 /404.html;

        location / {
            try_files $uri $uri/ =404;
        }

        access_log /var/log/nginx/mywebsite.com.access.log;
        error_log /var/log/nginx/mywebsite.com.error.log;
    }
}
```
- Test the Nginx config:

```bash
sudo nginx -t
```

- If everything is correct so start your nginx server

```bash
sudo systemctl start nginx
```

- Also check is your nginx is in running state or not

```bash
sudo systemctl status nginx
```

- If it is in running state so 
### 💥 Boom, Your website is now live on custom domain and https secured SSL.
