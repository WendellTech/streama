# FAQ
- [1. The .jar file is not executable](#1-the-jar-file-is-not-executable)
- [2. Where is the data stored?](#2-where-is-the-data-stored)
- [3. How do i install java?](#3-how-do-i-install-java)
- [4. How do I hook in my existing file system](#4-how-do-i-hook-in-my-existing-file-system-so-that-i-dont-need-to-re-upload-each-video-file)
- [5. Automatic Video Conversion](#5-so-what-about-video-encoding-and-html5-any-news-on-auto-conversion)
- [6. When restarting the app, the data is gone](#6-when-restarting-the-app-the-data-is-gone)
- [7. How to run in background](#7-how-to-run-in-background)
- [8. How do I change the port?](#8-how-do-i-change-the-port)
- [10 How do I install it as a service?](#10-how-do-i-install-it-as-a-service)
- [11 Video playback](#11-video-playback)
- [12 How to use SQL instead of H2 as a datastore](#12-how-to-use-sql-instead-of-h2-as-a-datastore)

### 1. The .jar file is not executable
If your .jar file isnt executable after download, just change the permissions with `sudo chmod u+x streama-1.x.x.jar`.  
   
:information_source: How is this concept of an executable .jar even possible? Internally, the application uses gradle and a plugin called gradle-boot. This plugin together with the overall setup of the application allows for the .jar file to executable all by itself!  
*Protip:* For a professional linux setup, use this feature to create a system service and run it by calling `sudo service streama start/stop/restart` :)

### 2. Where is the data stored?
By default, the data is stored in an embedded, persistent database called H2. This database persists the data into a file adjacent to the .war file that was executed.  
If you prefer the security of a mysql setup, use the [sample application.yml](https://github.com/dularion/streama/blob/master/docs/sample_application.yml) to configure the mysql connection. 

If your MySQL differs from the default 'root'@'localhost' with database 'streama' then you can just change those values in the application.yml. When running the app using `./streama-[version].jar` make sure that the application.yml is named correctly resides in the same directory as the .jar file.


### 3. How do i install java?
Make sure you got java8 up and running in your command line. Using OpenJDK works just fine! the below works for ubuntu. `sudo apt-get install openjdk-8-jdk`

### 4. How do I hook in my existing file system, so that I don't need to re-upload each Video file? 
Thanks to @jendib you can now use the "Local Video Files" Directory in the settings page! Just point it to the root directory of your media collection and you will have a nifty file-browser showing a manage-files popup for each episode/movie

### 5. So what about Video encoding and HTML5? Any news on auto-conversion? 
Auto-Conversion is still something that I want to see for streama, but there is so much to consider when it comes to self-hosted instances, such as CPU-power for conversion, local dependencies such as ffmpeg, potentially using nodejs as the crawler/worker ... I am still planning this and trying to figure out a way to make it as comfortable to host as possible, but for now it is still up to you to convert videos into html5.  

### 6. When restarting the app, the data is gone
Don't worry, the data isn't lost, in fact it is persisted to a file called Streama.db. You just need to make sure to always start the app from the same directory each time. The easiest is to always start it from inside the folder or using systemctl. 

### 7. How to run in background
easiest is by installing something like byobu or screen and running it in one of the tabs :) 
Alternatively, you can configure the programme as a service via init.d. There are several tutorials for that online for your perusal :) But we also got one for you here [How do I install Streama as a Service](#10-how-do-i-install-it-as-a-service).


### 8. How do I change the port? 
By Default, streama uses the port :8080, so if you want to access the application you will have to do it via yourIp:8080 or someDomain.com:8080. If you dont want to use a port, you need to use the default http-port, which is 80. The easiest way is to just edit it in the settings of the application.yml. You need to download the [sample application.yml](https://github.com/dularion/streama/blob/master/docs/sample_application.yml), rename it to "application.yml" and place it next to the war file before executing it. Check in the sample_application.yml, there is the port 8080, you can simply change that to 80 and then you can access streama directly via IP or domain-name. 


### 10 How do I install it as a service?
See the link below for service setup on Ubuntu 15+  
https://github.com/dularion/streama/wiki/Running-as-a-service-(autostart)-on-Ubuntu-15-or-higher

### 11 Video playback
if your videos won't play or you don't have any sound, make sure that they are HTML5 compatible. I will add video-conversion soon, but right now there is no conversion, so you have to rely on what your browser can handle.   
A quick and easy test is to open a new empty browser-tab and drag&amp;drop your video file in. If it **shows up** in a player and **there is sound**, then it's a compatible format. If it downloads, it's incompatible for both. if it only plays the video, but without sound, the audio codec is incompatible. The format that is most compatible is h264 for video and aac for audio. 
            
Try this: `ffmpeg -i input.mkv -vcodec h264 -acodec aac -strict -2 output.mp4`

### 12 How to use SQL instead of H2 as a datastore
You need to use application.yml alongside the war file. here is a [sample application.yml](https://github.com/dularion/streama/blob/master/docs/sample_application.yml) for you to work off of. Here, just uncomment the sql-lines and comment out the h2 lines! 
