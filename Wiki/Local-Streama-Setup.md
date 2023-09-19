### Step 1 - JDK
- If you don't have it already, go ahead and download a copy of [JDK8 (Oracle)]
- install JDK as usual, make sure to let JDK add new variables to your system
- After install, open up your command line and make sure the following outputs look correct: 
  - `java -version`  - this should be something like 1.8.0_144
  - `echo $JAVA_HOME` - this should be something like _/Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home_ or _/usr/lib/jvm/java-8-oracle_

### Step 2 - Clone the git repo
- In your command line, or your favorite Git Application, clone this repo to your desired location. `git clone git@github.com:dularion/streama.git`.
  - Note: This location will most likely differ from the Upload Directory of your video-files, so you don't have to keep that in mind while choosing the repo-location. 

### Step 3 - MySQL Database
- if you don't have a local installation of MySQL, install one. It could also be useful to have a way to browse your databases in a convenient way, for instance with phpMyAdmin. To get all in one, i recommend [XAMPP](https://www.apachefriends.org/index.html)
- once your MySQL is up and running, create a database called "streama". via command line, this would be `mysql> CREATE DATABASE streama;`
- the application assumes the following login-credentials for the database: `username root, no password`. If you want to change this, find and edit the information in `/grails-app/conf/DataSource.groovy`.

### Step 4 - Upload Directory
- Somewhere on your machine, create a directory where the application will store the uploaded video files. You can choose any directory.

### Step 5 - Run the application
- open your command line tool 
- navigate to the directory of the repo. for instance `cd /projects/streama`
- on Windows, run `grailsw.bat run-war`
- on a unix-system (Ubuntu, Mac, etc), run `./grailsw run-war`
- This command will download the local version of grails and run the app under http://localhost:8080


### Step 6 - Base Settings
- the first thing you will see when opening the URL is a login screen. Use username `admin` and password `admin` to log in. 
- You will then be redirected to the Settings page
  - enter the Upload Directory that you created in step 4 & verify if the application has access by pressing the "verify" button
  - enter your API-Key for theMovieDb.org & verify

If Everything worked out all right, you are now ready to use the application! 

# Troubleshooting
#### Upload Directory
- if the application cannot verify your Upload Directory, make sure you have the right permissions for the folder. If the app is running in a different user, make sure to add read/write permissions for the group or even all on the folder, through something like this `chmod g+rw your/directory` or `chmod ga+rw your/directory`

#### Startup problems
- if your application throws errors on startup and won't deploy, make sure that your `$JAVA_HOME` is set correctly. It needs to point to your installation of JDK not JRE, and it needs to be version 1.7.x, not 1.6.x or 1.8.x.
- Make sure your MySQL is running and that the application can access it with the credentials above

#### Video Playback
- if your videos won't play, make sure that they are HTML5 compatible. I will add video-conversion soon, but right now there is no conversion, so you have to rely on what your browser can handle. A quick and easy test is to open a new empty browser-tab and drag&drop your video file in. If it shows up in a player, then it's a compatible format. If it downloads, it's incompatible. Also, of all browsers, Chrome supports most of the HTML5 formats as far as I know. 
