1. Clone the repo:  
`git clone https://github.com/dularion/streama.git`

2. Make a user for streama to run as:  
`adduser --system --no-create-home --group streama`

3. Install openjdk 8 jre:  
`sudo apt-get install openjdk-8-jre`

4. Build Streama:  
`cd streama`  
`./gradlew assemble`

5. Make a directory for streama, we use /data/streama here, but you can use anything. Just make sure to replace any references to the correct directory:  
`sudo mkdir -p /data/streama`

6. Move the built .war into /data/streama (replace [version] with the built version):  
`sudo mv ./build/libs/streama-[version].jar /data/streama/streama.jar`
then
`sudo chmod ug+x /data/streama/streama.jar`

7. Fix permissions for the folder:  
`sudo chown -R streama:streama /data/streama/`  
`sudo chmod -R 744 /data/streama/`

8. Create the service:  
`sudo touch /etc/systemd/system/streama.service`  
`sudo chmod 644 /etc/systemd/system/streama.service`  
`sudo nano /etc/systemd/system/streama.service`  

```
[Unit]
Description=Streama Server
After=syslog.target
After=network.target

[Service]
User=streama
Type=simple
WorkingDirectory=/data/streama/
ExecStart=/data/streama/streama.war
TimeoutStartSec=120
Restart=on-failure
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=Streama

[Install]
WantedBy=multi-user.target
```

9. Start the service:  
`sudo systemctl start streama.service`

10. Wait about 40-60 seconds for the server to start. Check it with:  
`sudo systemctl status streama.service`  
At the end it should say something along the lines of:  
`Grails application running at http://localhost:8080 in environment: production`