# Nginx

## [Youtube Tutorial](https://www.youtube.com/watch?v=NTOcdjMs8E4)

## Steps to install and configure Nginx

---

### Install Nginx on your AWS EC2

- This command will install Nginx on your VM (Virtual Machine):

  ```bash
  sudo yum install nginx
  ```

- Start Nginx:

  ```bash
  sudo systemctl start nginx
  ```

- See Nginx status:

  ```bash
  sudo systemctl status nginx
  ```

- After starting, access your EC2 public IP in a browser. You should see the default static HTML page of Nginx.

---

### See the Configs of Nginx

- View the Nginx config file at:

  ```bash
  /etc/nginx/nginx.conf
  ```

- Basic structure of `nginx.conf`:

  ```nginx
  events {
  }

  http {
      server {
          location {
          }
      }
  }
  ```

- Default HTML files are located at:

  ```bash
  /usr/share/nginx/html
  ```

- Delete all existing HTML files (⚠️ careful!):

  ```bash
  sudo rm -rf /usr/share/nginx/html/*
  ```

- Create a new `index.html` file:

  ```bash
  sudo nano /usr/share/nginx/html/index.html
  ```

- After editing, save the file and restart Nginx:

  ```bash
  sudo systemctl restart nginx
  ```

- Now refresh your website in the browser to see your changes (e.g., a "Hello World" message).

---

### Modifying the `nginx.conf` File

- Backup the existing config file:

  ```bash
  sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf_backup
  ```

- Delete the original file (only if needed):

  ```bash
  sudo rm -rf /etc/nginx/nginx.conf
  ```

- Recreate the config file:

  ```bash
  sudo vi /etc/nginx/nginx.conf
  ```

- Press `i` (insert mode), then paste the following structure:

  ```nginx
  events {
  }

  http {
      include mime.types;

      server {
          listen 80; # or any port
          root /usr/share/nginx/html; # or any path (ensure the path is correct)
      }
  }
  ```

- Save and exit (`Esc` then `:wq`)

- Test the Nginx config:

  ```bash
  sudo nginx -t
  ```

- If everything is OK, restart Nginx:

  ```bash
  sudo systemctl restart nginx
  ```

---

### Nginx Logs

- View Nginx logs here:

  ```bash
  /var/log/nginx
  ```

---

### Host Multiple Websites (Virtual Hosts)

- To host multiple websites, add another `server` block in the config file:

  ```nginx
  events {
  }

  http {
      include mime.types;

      server {
          listen 80;
          root /usr/share/nginx/html;
      }

      server {
          listen 8080; # your second site's port
          root /usr/share/nginx/second-site; # correct path to second site
      }
  }
  ```

- If CSS doesn't load, make sure this line is included in the `http` block:

  ```nginx
  include mime.types;
  ```

- Save the config file and restart Nginx again:

  ```bash
  sudo systemctl restart nginx
  ```

---
