When you improve on a feature, or add some custom code, you will eventually need to create your own .war file to deploy to your server or to dockerize. Here are the easy steps to build your own .war: 

## Dependencies
- JDK 8 (you need jdk, not just plain java) 
- The source code of streama 


## Steps 
- Enter the streama directory with your preferred command-line tool 
- make sure there is a `gradlew` file (for mac) or a `gradlew.bat` (for windows) in that directory
- run `./gradlew assemble` for mac, or `./gradlew.bat assemble` for windows 
- after completion, you will find your new .war file under build/libs/streama-[version].war
  - Note: you can rename this file freely after it's been built. 

