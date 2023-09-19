#### Update Server
```
apt-get update
apt-get upgrade
reboot
```
reconnect afterward

### Install git (if it is not installed)
```
apt-get install git
```

#### Setup Tomcat and MySQL
```
apt-get install tomcat7
apt-get install mysql-server
mysql> CREATE DATABASE streama;
```
* For installing tomcat, this tutorial is fantastic: [Tomcat7 on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-7-on-ubuntu-14-04-via-apt-get)

* For installing mysql, this tutorial may be partly helpful: [LAMP on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04)

By default the application assumes a db-login of username "root", no password. If you wish to change this, edit the settings in the production-section of `grails-app/conf/DataSource.groovy`

#### Install Mail Server (optional)
By default the application assumes the default mail server configured at localhost on port 25. You install simply by using
```
apt-get install sendmail
```
In order to get emails to work with something else, look into [Grails mail plugin](http://grails.org/plugins/mail).


#### Create Upload Directory
it is recommended that you choose an upload-directory outside of the application, so that no one can access without permission. Let's assume you choose to place it in root under /data/streama
```
mkdir /data
mkdir /data/streama
chown tomcat7:tomcat7 /data/streama
```

#### Compile the Streama ROOT.war
This can be done either on your local machine or on the remote server
```
cd /wherever/you/want/to/put/streama
git clone https://github.com/dularion/streama.git
cd streama
./grailsw war ROOT.war -Dgrails.env=production
```
Note: for this step you need JDK. If you compile the war locally, just install it from [oracle.com](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html). For Ubuntu, [@TheConnMan](https://github.com/TheConnMan) suggested the following
> I ended up using the following script (along with some additions to ~/.bashrc) to add the JAVA_HOME variable.
> wget --no-check-certificate https://github.com/aglover/ubuntu-equip/raw/master/equip_java7_64.sh && bash equip_java7_64.sh

#### Deploy to Tomcat

on your remote server, stop tomcat7 & remove old tomcat7 ROOT
```
service tomcat7 stop
rm -R /var/lib/tomcat7/webapps/ROOT
```

###### ROOT.war is on remote machine
If you built the ROOT.war on your remote server, run
```
cp ROOT.war /var/lib/tomcat7/webapps
```

###### ROOT.war is on local machine
If you built the ROOT.war on your local machine, run
```
rsync -avzhP ROOT.war username@your-remote-server:/var/lib/tomcat7/webapps/
```

#### Start Application
```
service tomcat7 start
tail -f /var/log/tomcat7/catalina.out -n99
```
after successful deployment, the last line should read `INFORMATION: Server startup in X ms`


#### Navigate to Application
If everything worked out all right, you should now be able to access the application on `http://your-remote-server:8080`


# Extras
- What really comes in handy after a while is a Jenkins setup to deploy the application (and all your other applications for that matter! It is really handy). If you are interested in a tutorial, write an issue for me please :) 
- If you have iptables running, make sure to add port 8080 to the Allowed list. 
- If you want to add SSL to the application, you can either use nginx or the native functionality in tomcat. [rhulha](https://github.com/rhulha) wrote a comprehensive tutorial for this: https://github.com/dularion/streama/wiki/SSL-tutorial-for-ubuntu-and-www.startssl.com
- If you are using PostgreSQL instead of MySQL, Streama might fail to start because `user` is a reserved keyword.To make Streama run, map the `User` domain to a table called users by adding `table 'users'` inside the `static mapping` section of User.groovy.
- If you are using tomcat8,you might have to comment out the line `runtime 'mysql:mysql-connector-java:5.1.36'` from build.gradle when building the WAR file to prevent it  from being bundled into the WAR file(Can cause memory leaks).Instead, download the connector and put the jar file in TOMCAT_HOME/lib.More explanation [here](http://stackoverflow.com/a/7198049).