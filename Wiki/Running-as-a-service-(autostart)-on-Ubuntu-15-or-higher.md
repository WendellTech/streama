# Go to the app directory, for example
`cd /data/streama`

# Create link a link to your installed version
This is useful when you update streama you can just update the link:

`ln -s streama-[version].jar streama.jar`

# Create the systemctl service
`nano /etc/systemd/system/streama.service`

In this file add the below, if your streama isn't in `/data/streama` change them.

```
[Unit]
Description=streama
After=syslog.target

[Service]
User=[USER TO RUN APP]
ExecStart=/data/streama/streama.jar
SuccessExitStatus=143
ConditionPathExists=/data/streama/streama.jar
# end streama.service content

[Install]
WantedBy=multi-user.target
```

# Enable and start the service
```
sudo systemctl enable streama.service
sudo systemctl start streama.service
```