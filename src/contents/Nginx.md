---
author: kuldeep
datetime: 2024-08-03
title: Nginx
slug: nginx
featured: true
draft: false
tags:
  - nginx
ogImage: ""
description: Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.
---

# Nginx
Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache. The software was created by Russian developer Igor Sysoev and publicly released in 2004. Nginx is free and open-source software, released under the terms of the 2-clause BSD license.

## Installation
```bash
sudo apt install nginx
```

## Configuration and setup
1. **Nginx configuration:**

   The main configuration file is usually located at `/etc/nginx/nginx.conf`, but it's common practice to define server blocks in separate files within `/etc/nginx/sites-available/` and create symbolic links to them in `/etc/nginx/sites-enabled/`.

   If you don't have a file for your server yet, you can create one. For example:
   ```bash
   sudo nano /etc/nginx/sites-available/my_site
   ```

2. **Add or modify the server block to forward requests from port 80 to port 9000:**

   Add the following configuration in your Nginx file:
   ```nginx
   server {
       listen 80;
       server_name mysite.com; # Change this to your domain name or use _ for default

       location / {
           proxy_pass http://localhost:9000; # Assuming the service is running on port 9000
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

   Replace `mysite.com` with your actual domain name. If you want this configuration to be the default for any request to your server on port 80, you can use `_` as the server name.

3. **Enable the configuration:**

   If you created the configuration file in `/etc/nginx/sites-available/`, create a symbolic link to it in `/etc/nginx/sites-enabled/`:
   ```bash
   sudo ln -s /etc/nginx/sites-available/my_site /etc/nginx/sites-enabled/
   ```

4. **Test the Nginx configuration:**

   Before restarting Nginx, test the configuration to ensure there are no syntax errors:
   ```bash
   sudo nginx -t
   ```

5. **Restart Nginx:**

   If the configuration test is successful, restart Nginx to apply the changes:
   ```bash
   sudo systemctl restart nginx
   ```


## SSL Config
### Certbox installation

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
```

### Use
```bash
sudo certbot --nginx -d sub.example.com
```
