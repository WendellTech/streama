```bash
sudo su
apt-get upgrade
apt-get update
reboot

apt install openjdk-8-jre
mkdir /data
mkdir /data/streama
touch /data/streama/README.md

# new linux user
sudo adduser streama  # add password to README.md
sudo usermod -aG sudo streama
sudo chown streama:streama /data/streama/ -R

# download streama
cd /data/streama
sudo su streama
wget https://github.com/streamaserver/streama/releases/download/v1.7.0/streama-1.7.0.jar # or newer release
chmod ug+x streama-1.7.0.jar # make executable
ln -s streama-1.7.0.jar streama.jar # create link

# mysql:
sudo apt install mysql-server
sudo mysql_secure_installation # add password to README.md
sudo mysql
> create database streama; 

# application.yml 
wget https://raw.githubusercontent.com/streamaserver/streama/master/docs/sample_application.yml
mv sample_application.yml application.yml
vi application.yml # add your mysql username & password

# add system.d service
sudo su
touch /etc/systemd/system/streama.service
vi /etc/systemd/system/streama.service

# START streama.service
[Unit]
Description=streama
After=syslog.target
 
[Service]
User=streama
ExecStart=/data/streama/streama.jar
SuccessExitStatus=143
ConditionPathExists=/data/kcenter/streama.jar
 
[Install]
WantedBy=multi-user.target
# 
# END streama.service


chmod 664 /etc/systemd/system/streama.service
sudo systemctl enable streama


sudo systemctl start streama #start streama
sudo journalctl -u streama -f #logs


# nginx
sudo apt-get -y install certbot nginx python3-certbot-nginx
sudo touch /etc/nginx/sites-available/streama
vi /etc/nginx/sites-available/streama

# START nginx-conf
server {
  listen 80;
  listen [::]:80;

  server_name YOUR_DOMAIN.com;

  client_max_body_size 128g; # allows larger files (like videos) to be uploaded.

  location / {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;

      # websocket start
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $http_connection;
      proxy_read_timeout 86400;
      # websocket end

      proxy_pass http://localhost:8080;
  }
}
# END


sudo ln -s /etc/nginx/sites-available/streama /etc/nginx/sites-enabled/streama 
sudo service nginx reload
sudo certbot --nginx -d YOUR_DOMAIN.com
```

