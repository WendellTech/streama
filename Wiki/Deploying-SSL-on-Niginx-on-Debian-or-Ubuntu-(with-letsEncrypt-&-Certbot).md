# This is out of date, please see https://docs.streama-project.com/

&nbsp;


&nbsp;


&nbsp;
&nbsp;


&nbsp;


&nbsp;&nbsp;


&nbsp;


&nbsp;



This guide will show you how to use [Certbot](https://certbot.eff.org/) to automatically retrieve a free signed SSL certificate from https://letsencrypt.org/ and configure a Nginx reverse proxy to use it for Streama.

Install `certbot` with the `nginx` plugin

> sudo apt-get -y install certbot nginx python-certbot-nginx

Create a basic reverse proxy configuration to start with. Replace `streama.example.net` with your own domain.

> sudo nano /etc/nginx/sites-available/streama 

```nginx
server {
  listen 80;
  listen [::]:80;

  server_name streama.example.net;

  client_max_body_size 128g; # allows larger files (like videos) to be uploaded.

  location / {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Forwarded-Port $server_port;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

      #WebSocket Support
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $http_connection;
      
      proxy_pass http://localhost:8080;
  }
}
```
Enable the new Nginx configuration

> sudo ln -s /etc/nginx/sites-available/streama /etc/nginx/sites-enabled/streama
> sudo service nginx reload

Obtain the certificate:

> sudo certbot --nginx -d streama.example.net

Reconfigure Nginx to use the certificate:

> sudo nano /etc/nginx/sites-available/streama

```nginx
server {
  listen 80;
  listen [::]:80;

  server_name strema.example.net;
  
  # Redirect all HTTP requests to HTTPS with a 301 Moved Permanently response.
    return 301 https://$host$request_uri;
}

server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;

  # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
  ssl_certificate /etc/letsencrypt/live/streama.example.net/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/streama.example.net/privkey.pem;
  ssl_session_timeout 1d;
  ssl_session_cache shared:SSL:50m;
  ssl_session_tickets off;


  # modern configuration. tweak to your needs.
  ssl_protocols TLSv1.2;
  ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256';
  ssl_prefer_server_ciphers on;

  # HSTS (ngx_http_headers_module is required) (15768000 seconds = 6 months)
  add_header Strict-Transport-Security max-age=15768000;

  # OCSP Stapling ---
  # fetch OCSP records from URL in ssl_certificate and cache them
  ssl_stapling on;
  ssl_stapling_verify on;

  ## verify chain of trust of OCSP response using Root CA and Intermediate certs
  ssl_trusted_certificate /etc/letsencrypt/live/streama.example.net/chain.pem;

  resolver 1.1.1.1;

  server_name strema.example.net;

  client_max_body_size 128g;

  location / {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Forwarded-Port $server_port;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

      #WebSocket Support
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $http_connection;
      
      proxy_pass http://localhost:8080;
  }
}
```

> sudo service nginx reload