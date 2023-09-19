# Requirements
- make sure you have the upload directory set up
- optional: set up a "Local Video Files" directory for easier movie-adding
- optional: add a theMovieDbKey to your app for easier creation

# Creating the Movie
- Navigate to `Manage Content` -> `Movies`
- Use the button "Create new Movie"

![](https://i.imgur.com/huMGe3Y.png)

# Add Movie MetaData
Based on in if you have a theMovieDbKey or not, you can either add the movie's metaData automatically (by searching for the name) or manually. 
## Manual Input 
![Imgur](https://i.imgur.com/Os2LXds.png)

## Automatic Input 
![Imgur](https://i.imgur.com/vhQcNRR.jpg)

# Attach the Video File
In the Movie page, go to the settings dropdown and choose `Manage Files`. 
![Imgur](https://i.imgur.com/324WAnc.png)

There, you can choose between 2 or 3 Options, depending on if you set up the "Local Video Files". 
### Option 1: Uploading a file directly
This will allow you to upload a Video file directly, using regular http uploads. It will internally be uploaded to your pre-configured Upload-Directory. 
![Imgur](https://i.imgur.com/4WpipX1.png)

### Option 2: External URL 
Here you can input a url to an mp4 or mkv file if you have such a URL. Important note, this URL may not be barred behind a login or similar. 
![Imgur](https://i.imgur.com/xa1rcY4.png)

### Option 3: Local File (recommended)
Here you get to browse the directories / files that are contained within your  "Local Video Files" directory. 
This is a very convenient way to add videos, as it does not require extra uploading and allows for speedy media-adding. 
![Imgur](https://i.imgur.com/P2rPDxz.png)

### Optional: add Subtitles 
You can now add subtitles to your movie the same way that you added the video File. Make sure its an .srt or .vtt file. You can even add Multiple subtitles and assign them with Languages & an internal identification-key.

# FAQ
- Can I bulk add Videos? Yes you can! We built a file-crawler which uses the tmdb key and fetches the metaData for any file that it finds a hit for. Check it out: https://github.com/streamaserver/streama/wiki/Streama-regex-Matcher-for-Batch-File-Adding
- What Video Formats are supported? See https://github.com/streamaserver/streama/wiki/FAQs#11-video-playback


 