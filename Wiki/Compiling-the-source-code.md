When you have made adjustments to the source code, it is likely that you will want to create a new .jar file and deploy it on your server. For this, you can use a simple command: 

```bash
# for unix based systems
./gradlew assemble 

# for windows
./gradlew.bat assemble
```

This will create 2 new .jar files under `build/libs`, 
- **streama-{version}.jar**
- **streama-{version}.jar.original**

all you will need is the **streama-{version}.jar**. 

This file is an executable, so you can just copy it into your deployment directory / your server and start it as usual. 


### How to change the version number
The version number can be configured in in the  **build.gradle** around line 20

```
// ...

version "1.6.0-RC6"
group "streama"


apply plugin:"eclipse"
apply plugin:"idea"
// ...
```
The version number is intended to be semantic, so {major}.{minor}.{patch} with RC info if you need it. But you can change it to any string that you like.