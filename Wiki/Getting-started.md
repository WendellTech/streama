On this page you will find deployment info (mainly for Linux).   
For the FAQs, visit [the dedicated FAQs Page](https://github.com/dularion/streama/wiki/FAQs).

# Guide for Linux
Here is a video Tutorial of **@dularion** setting it up: http://www.youtube.com/watch?v=GUcbVTrdNv8   
Below you can find a step-by-step.

## 1. **Install Java** 
Get and install Java 8 (JDK or JRE). OpenJDK is a preferred JDK.

**Java JRE**
```bash
sudo apt update
sudo apt install openjdk-8-jre
```

OR

**Java JDK**
```bash
sudo apt update
sudo apt install openjdk-8-jdk
```

## 2. **Create an install directory**
Create an install directory for Streama our example uses _/data/streama_ for the yml and the jar and _/data/streama/files_ for any uploaded files.
```bash
mkdir -p /data/streama /data/streama/files
cd /data/streama
```

## 3. **Download the .jar and create an application.yml** 
Download the latest jar from the [releases page](https://github.com/dularion/streama/releases/latest), and then copy the jar file to your device.
```bash
wget https://github.com/streamaserver/streama/releases/download/v1.9.1/streama-1.9.1.jar
```

(Optional)
A sample application.yml is avalible at _https://github.com/streamaserver/streama/blob/master/docs/sample_application.yml_
```bash
wget https://raw.githubusercontent.com/streamaserver/streama/master/docs/sample_application.yml
mv sample_application.yml application.yml
```

## 3. **Run!**
The main way to start the app is via the command below.

```bash
sudo ./streama-[version].jar
```
If the above command give you an error you can try either of the below commands to correct this
```bash
chmod +x ./streama-[version].jar
```
OR
```bash
java -jar streama-[version].jar
```

Once you see the console output  
`Grails application running at http://localhost:8080 in environment: production`  
you are set to go! just navigate to localhost:8080 (If you are on the computer that you are running this from else it will be that devices local IP) and you will see a login screen (default = username:admin, password:admin), and the program is up and running. Enjoy! :) 

## Next steps 
For production-purposes, you should consider running streama as a background-service and use mysql instead of the built-in h2 database this give you the ability to backup your database and scale your enviroment. 
- [Running streama as a service](https://github.com/streamaserver/streama/wiki/Running-as-a-service-(autostart)-on-Ubuntu-15-or-higher)
- [Configuring MySQL](https://github.com/streamaserver/streama/wiki/FAQs#2-where-is-the-data-stored)

***

:information_source: If something didn't work or you want to improve the workflow, check the [the dedicated FAQs Page](https://github.com/dularion/streama/wiki/FAQs). 

***
