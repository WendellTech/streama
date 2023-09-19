
#### Install Java

Java is required for the application and is recommended that you use openjdk from the official repository
```
yum install java-1.8.0-openjdk-devel
```

After installing java, tomcat and the grails wrapper are going to require your JAVA_HOME to be set. You can do this by editting your ~/.bash_profile

~/.bash_profile
```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
export JAVA_HOME
```

You can then reload the .bash_profile file
```
. ~/.bash_profile
```

#### Setup Tomcat

Install wget if you don't have it already
```
yum install wget
```

Download latest Tomcat 7 tar.gz file from http://tomcat.apache.org/download-70.cgi
```
cd /tmp
wget http://www.webhostingjams.com/mirror/apache/tomcat/tomcat-7/v7.0.63/bin/apache-tomcat-7.0.63.tar.gz -O tomcat7.tar.gz
mkdir /opt/tomcat
tar -xzvf tomcat7.tar.gz -C /opt/tomcat --strip-components=1 && rm -rf tomcat7.tar.gz
```

Create a tomcat user
```
useradd -r -s /sbin/nologin tomcat
chown -R tomcat:root /opt/tomcat
```

After installing tomcat since Centos defaults to having firewalld active we need to open port 8080 for streama if you are accessing the machine outside of localhost
```
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload
```

Create a systemd service for deploy on boot. Service file sourced from https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-centos-7

/etc/systemd/system/tomcat.service

```
# Systemd unit file for tomcat
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=tomcat
Group=tomcat

[Install]
WantedBy=multi-user.target
```

Please note the user and group fields. If your tomcat user is named something different you will have to change them to match the user added in the useradd command from the previous steps.

After creating the service file you need to reload the systemd configurations
```
systemctl daemon-reload
```

After creating and refreshing the systemd configuration you should be able to start tomcat and enable it so that it starts up on machine boot
```
systemctl start tomcat
systemctl enable tomcat
````

### Installing MySql

You can get mysql from the main repositories using
```
yum install mariadb mariadb-server
```

After getting mysql you can run the secure mysql install using
```
sudo mysql_secure_installation
```

NOTE: By default the streama application assumes a db-login of username "root" with no password. If you feel this is okay then specify that you do not want to create a root password in the mysql_secure_installation.

For most of the answers on the secure installation you will want to use the default values as this clears sample database information, disables remote root and various other mysql cleanup which will make your instance safer.

After such your database should be setup and you just have to create the streama database. From here access your mysql server. Note that if you specified a different root password your command may require a "-p" argument.
```
mysql -u root
```

after connecting to mysql you can create the streama database by running
```
MariaDB [(none)]> create database streama;
```

If you are okay with the application defaults of localhost and of a db-login of "root" with no password you can skip to the Install Mail Server section. Otherwise we will want to create a new "streama" user on mysql for use by the application and grant it permission to the streama database
```
MariaDB [(none)]> create user 'streama'@'localhost' identified by 'streama';
--if the streama application will be on a different machine from the database machine you will also want to create a remote user
MariaDB [(none)]> create user 'streama'@'%' identified by 'streama';
MariaDB [(none)]> grant all on streama.* to 'streama'@'localhost';
MariaDB [(none)]> grant all on streama.* to 'streama'@'%';
```

After creating the mysql user and granting the permissions you can exit out of mysql. From here we need to configure the application using a file in the source found at 'streama/grails-app/conf/DataSource.groovy'. There are two places we need to update

```
//At the top
dataSource {
    pooled = true
    jmxExport = true
    driverClassName = "org.h2.Driver"
    username = "streama"  //changes to mysql user
    password = "streama"  //changes to mysql streama pass
}
...
//under environments
production {
    dataSource {
      dbCreate = "update"
      driverClassName = "com.mysql.jdbc.Driver"
      dialect = org.hibernate.dialect.MySQL5InnoDBDialect

      //DEV
      url = "jdbc:mysql://localhost:3306/streama"
      username = "streama"  //changes to mysql user
      password = "streama"  //changes to mysql streama pass
...
```

After these changes the application will now use the streama user and password to connect to the database. You can also update the url values if your database machine is on a seperate machine from your streama machine. 

#### Install Mail Server (optional)

By default the application assumes the default mail server configured at localhost on port 25. You need to install the mail transport agent using
```
yum install sendmail
```

By default Centos has firewalld setup to disallow port traffic to 25. To check if port 25 is currently open, assuming you don't have a dmz setup, use
```
firewall-cmd --zone=public --list-ports
```

This should list what ports are currently open. If you do not see port 25 from the previous command you will have to add an entry for it
```
firewall-cmd --zone=public --add-port=25/tcp --permanent
firewall-cmd --reload
```

In order to get emails to work with something else, look into [Grails mail plugin](http://grails.org/plugins/mail).


#### Create Upload Directory
it is recommended that you choose an upload-directory outside of the application, so that no one can access without permission. Let's assume you choose to place it in root under /data/streama
```
mkdir /data
mkdir /data/streama
chown -R tomcat:root /data/streama
```

#### Compile the Streama streama.war
This can be done either on your local machine or on the remote server
```
cd /wherever/you/want/to/put/streama
git clone https://github.com/dularion/streama.git streama
cd streama
./grailsw war streama.war -Dgrails.env=production
```
#### Deploy to Tomcat

on your remote server, stop tomcat7 & remove old tomcat7 streama
```
systemctl stop tomcat && rm -rf /opt/tomcat/webapps/stream.war
```

Then you will want to place the war in the tomcat webapp directory
```
cp streama.war -t /opt/tomcat/webapps
```

#### Start Application
```
systemctl start tomcat
tail /opt/tomcat/logs/catalina.out -n200
```
after successful deployment, the last line should read `INFORMATION: Server startup in X ms`


#### Navigate to Application
If everything worked out all right, you should now be able to access the application on `http://your-remote-server:8080/streama`


# Extras
- If you want to add SSL to the application, you can either use nginx or the native functionality in tomcat. [rhulha](https://github.com/rhulha) wrote a comprehensive tutorial for this: https://github.com/dularion/streama/wiki/SSL-tutorial-for-ubuntu-and-www.startssl.com