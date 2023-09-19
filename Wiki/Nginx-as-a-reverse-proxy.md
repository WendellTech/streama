# This is out of date, please see https://docs.streama-project.com/

&nbsp;


&nbsp;


&nbsp;
&nbsp;


&nbsp;


&nbsp;&nbsp;


&nbsp;


&nbsp;



# On the root of a (sub)domain
1. On the settings page, set the `Base URL` that you will use after configuring nginx.
EG: `http://example.com`

2. Configure nginx:

```nginx
server {
  listen 80;
  listen [::]:80;

  server_name example.com;

  client_max_body_size 128g; # allows larger files (like videos) to be uploaded.

  location / {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_pass http://localhost:8080;
  }
}
```

# On a subdirectory
1. On the settings page, set the `Base URL` that you will use after configuring nginx.
EG: `http://example.com/streama`

2. Set `application.yml`

In the `application.yml` set:

```yaml
environments:
    production:
        grails:
            assets:
                url: http://example.com/streama/assets/
```

3. Configure nginx:

```nginx
server {
  listen 80;
  listen [::]:80;

  server_name example.com;

  client_max_body_size 128g; # allows larger files (like videos) to be uploaded.

  location /streama/ {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      rewrite ^/streama/(.*)$ /$1 break;
      proxy_pass http://localhost:8080;
  }
}
```