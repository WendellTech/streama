# This is out of date, please see https://docs.streama-project.com/

&nbsp;


&nbsp;


&nbsp;
&nbsp;


&nbsp;


&nbsp;&nbsp;


&nbsp;


&nbsp;



Unfortunately, the application does not convert your videos for you (yet!). In order to get the videos working, you might have to convert them yourself. For compatible codecs, see [HTML5 Video Browser Support](https://en.wikipedia.org/wiki/HTML5_video#Browser_support)

if your videos won't play, make sure that they are HTML5 compatible. I will add video-conversion soon, but right now there is no conversion, so you have to rely on what your browser can handle. A quick and easy test is to open a new empty browser-tab and drag&drop your video file in. If it shows up in a player, then it's a compatible format. If it downloads, it's incompatible. Also, of all browsers, Chrome supports most of the HTML5 formats as far as I know.

## Batch-Conversion H.264
I've written a bash-script [html5VideoHandBrakeFolder.sh](https://gist.github.com/dularion/6237d651c385d2552916) that loops over the contents of a given folder and converts them to H.264. It requires HandBrakeCLI. 

Usage: 
```
 ./html5VideoHandBrakeFolder.sh  directory
```