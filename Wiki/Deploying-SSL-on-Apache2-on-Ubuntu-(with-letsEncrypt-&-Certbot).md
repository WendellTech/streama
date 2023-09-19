# This is out of date, please see https://docs.streama-project.com/

&nbsp;


&nbsp;


&nbsp;
&nbsp;


&nbsp;


&nbsp;&nbsp;


&nbsp;


&nbsp;



This guide will show you how to use [Certbot](https://certbot.eff.org/) to automatically retrieve a free signed SSL certificate from https://letsencrypt.org and configure Apache2 reverse proxy to use it for Streama.

Install `certbot`

>sudo apt-get install certbot

Create a basic reverse proxy configuration to start with. Replace `streama.example.net` with your own domain.

>sudo nano /etc/apache2/sites-available/streama.example.net.conf

```apache
<VirtualHost *:80>
    #Basic apache vhost configuration
    ServerName streama.example.net
    ServerAdmin webmaster@localhost
    ErrorLog /var/log/apache2/syslog
    LogLevel warn
    CustomLog /var/log/apache2/syslog combined
    ServerSignature on

    #Reverse proxy configuration
    #Change the port (here 8080) to whatever you define in your application.yml
    ProxyPreserveHost On
    ProxyPass / http:127.0.0.1:8080
    ProxyPassReverse / http://127.0.0.1:8080
</VirtualHost>
```
Reload apache to use the new conf
>sudo systemctl reload apache2

Enable the new Apache configuration
>sudo a2enconf streama.example.net.conf

Obtain the certificate:
>sudo certbot --apache

Follow the guide, but be sure to not redirect traffic to https://, we'll do this manually

Reconfigure Apache to use the certificate:
>sudo nano /etc/apache2/sites-available/streama.example.net
```apache
<VirtualHost *:80>
    #Basic apache vhost configuration
    ServerName streama.example.net
    ServerAdmin webmaster@localhost
    ErrorLog /var/log/apache2/syslog
    LogLevel warn
    CustomLog /var/log/apache2/syslog combined
    ServerSignature on

    #Reverse proxy configuration
    #Change the port (here 8080) to whatever you define in your application.yml
    ProxyPreserveHost On
    ProxyPass / http:127.0.0.1:8080
    ProxyPassReverse / http://127.0.0.1:8080

    #Redirect traffic to https://
    RewriteEngine on
    RewriteCond %{SERVER_NAME} =streama.example.net
    RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

<VirtualHost *:443>
    #Basic vhost configuration
    ServerName streama.example.net
    ServerAdmin webmaster@localhost
    ErrorLog /var/log/apache2/syslog
    LogLevel warn
    CustomLog /var/log/apache2/syslog combined
    ServerSignature On

    #SSL configuration
    Include /etc/letsencrypt/options-ssl-apache.conf
    SSLCertificateFile /etc/letsencrypt/live/example.net/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/example.net/privkey.pem
    Header always set Strict-Transport-Security "max-age=63072000; includeSubdomains;"

    #Proxy configuration
    ProxyPreserveHost On
    ProxyPass / https://127.0.0.1:8080/
    ProxyPassReverse / https://127.0.0.1:8080/
    
    #Ensure SSL and the proxy work nicely together
    SSLProxyEngine On
    SSLProxyVerify none
    SSLProxyCheckPeerCN off
    SSLProxyCheckPeerName off
    SSLProxyCheckPeerExpire off
</VirtualHost>
```
Reload apache
>sudo systemctl reload apache2.service

You'll now be able to access your streama server via streama.example.net
