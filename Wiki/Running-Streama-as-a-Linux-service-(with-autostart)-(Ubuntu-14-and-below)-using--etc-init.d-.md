Create a new config named `streama` under `/etc/init.d/`.

```bash
sudo su
touch /etc/init.d/streama
```

now add in this content: 
```bash
#! /bin/sh
### BEGIN INIT INFO
# Provides: streama
# Required-Start: $remote_fs $syslog
# Required-Stop: $remote_fs $syslog
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Streama
# Description: This file starts and stops Streama server
#
### END INIT INFO

STREAMA_DIR=/data/streama/

case "$1" in
 start)
   $STREAMA_DIR/streama.war
   ;;
 stop)
   $STREAMA_DIR/streama.war
   sleep 10
   ;;
 restart)
   $STREAMA_DIR/streama.war
   sleep 20
   $STREAMA_DIR/streama.war
   ;;
 *)
   echo "Usage: streama {start|stop|restart}" >&2
   exit 3
   ;;
esac
```

The above script assumes, that you placed your version of `streama.war` under `/data/streama/`. If you want to adjust any of this, make the appropriate adjustments in the above content. 

**Protip**: If you update streama frequently, keep the file name as it is when downloading (i.e. streama-x.x.x.war) and link it to "streama.war". 
```
ln -s streama-1.6.0-RC5.war streama.war
```


Finally, add the service to autostart:

```
sudo update-rc.d streama defaults
```