# StreamOrNot

## A set of testing tools to determine how well your video will stream.

## Sample Usage:  

1. Open the [Stream or not](https://dougsillars.github.io/StreamOrNot/) page.

You can add your own video to test, or use the default video (of a [NASA](https://nasa.gov/multimedia/hd/index.html) rocket launch).



3. You'll notice a number of Stats accumulating below the video on playback.
4. When the video completes, a summary is presented
5. Test the video at different network speeds using [devTools](https://css-tricks.com/throttling-the-network/) or a proxy like Charles.

## Advanced Features
### API: every feature on the form is available as a query string to test.html.
1. Add 'video' query parameter to the url pointing to your video to get stats on how well your video plays back
>https://dougsillars.github.io/StreamOrNot/?video=https://res.cloudinary.com/dougsillars/video/upload/v1533591785/depend_p2ryou.mp4
2. Videos rarely load alone on the web, and there are other assets competing for bandwidth.
    1. Add 'imagecount' query parameter.  Values can be from 0 to 99.  This will download the specified number of images while the video is playing back.
    2. You can see the images load right below the video, and again below all the text as 10x10 images.
    >https://dougsillars.github.io/StreamOrNot/?imagecount=25&video=https://res.cloudinary.com/dougsillars/video/upload/v1533591785/depend_p2ryou.mp4
3.  You can choose to deliver the images via HTTP or HTTP2 (http2 parameter).  Why do we care?  
	1. Images delivered by HTTP2 will use just one HTTP connection.
	2. Using HTTP1 (http2=false) better simulates many connections for files from many domains. It will max out the number of browser connections (if you use enough images.)
4. Oh, and JavaScript parsing will be going on - slowing down the browser, CPU, etc.
    1. Add js=true query parameter.  This will compute a recursive fibonacci every 250ms during playback to simulate JS execution.
    2. You can see the parse times in the console.
>https://dougsillars.github.io/StreamOrNot/?js=true&imagecount=25&video=https://res.cloudinary.com/dougsillars/video/upload/v1533591785/depend_p2ryou.mp4
    

  
  
### What Does It All Mean?

![screenshot of top entries](/images/View1.png)

1. The bar at the top shows how far the playback has gone (green), and the amount of video in the buffer (gray).  Code borrowed from [MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/Audio_and_video_delivery/buffering_seeking_time_ranges)
2. On Chrome, the connection speed is shown (navigation.connection is Chrome only)
3. Video playback state. Possible options are 
    1. Not Started
    2. Playing
    3. waiting
    4. stalled
    5. empty buffer
    6. ended
4. Video Start time.  Measured from start of JS execution to when the video begins playing.  The longer this takes, the text color changes to yellow and then red.
5. Playback time (in seconds)
6. Remaining time in buffer (in seconds)


![buffering chart](/images/buffer1.png)

This chart shows the amount of video in the downloaded buffer (video that is local and ready to play.)  In the above chart, there is an initial big jump in data in the buffer, and small subsequent jumps that 'top off' the buffer as it slowly empties.

The more video that is in the buffer, the less likely there will be a stall.  In the chart, if there is more than 10s of video in the buffer, the line is green. If there is between 5-10s in the buffer, the line is yellow (low).  If there is less than 5s in the buffer, the line is red (danger).

Where did I come up with these metrics for danger and low? I made a good educated guess.

![buffering chart with very low levels](/images/stall1.png)

When the buffer level is really low, there is a possibility that it will empty, and the video will stall.
The most recent stall length is displayed, as is the total count of stalls (and the total elapsed time)

### Using WebPageTest

If you (like me) love using WebPageTest to get metrics on page load, StreamOrNot can report the streaming data via custom metrics. Once the video is completed, the video results are placed in hidden DIVs that WPT can read.

To import the video results data into WPT, copy the following into the Advanced Settings:Custom:Custom Metrics: 
```
[videostats]
return document.getElementById("WPTData").innerHTML;
[videoStartTime] 
return document.getElementById("WPTStartTime").innerHTML;
[videoStallTotal] 
return document.getElementById("WPTStallTotal").innerHTML;
[videoStallTotalTime] 
return document.getElementById("WPTStallTotalTime").innerHTML;
[videominStall] 
return document.getElementById("WPTminStall").innerHTML;
[videomaxStall] 
return document.getElementById("WPTmaxStall").innerHTML;
[videoavgStall] 
return document.getElementById("WPTavgStall").innerHTML;
[videoBufferPercentDanger] 
return document.getElementById("WPTBufferPercentDanger").innerHTML;
[videoBufferPercentLow] 
return document.getElementById("WPTBufferPercentLow").innerHTML;
[videoBufferPercentSafe] 
return document.getElementById("WPTBufferPercentSafe").innerHTML;
[videoBufferMax] 
return document.getElementById("WPTBufferMax").innerHTML;
[videoBufferStart] 
return document.getElementById("WPTBufferAtStart").innerHTML;
[videoBitrateChangeCount] 
return document.getElementById("WPTBitrateChangeCount").innerHTML;
```
The first entry is a JSON object with all of the stats.  However, when testing with WPT - one typically runs multiple tests and collect the median value.  What's the median value of a JSON object?  So, for (mostly my) convenience, each piece of data is also supplied as an individual stat.  

#### Parsing the WPT data.  

I have modified Andy Davies' [WPT Bulk Tester](https://github.com/andydavies/WPT-Bulk-Tester) to include the video stats.

You can copy my [video enhanced version](https://docs.google.com/spreadsheets/d/1v73GZCFz3vA8NmzFk8R1nDHR6k-YUtA7c-jFfLqqfrE/edit?usp=sharing), and use it for your tests. 

**NB: I have set the median metric to be the video startup time.  Typically WPT uses the SpeedIndex as the median. **

### YouTube Tutorial

[![](http://img.youtube.com/vi/wH9HhmbfnhA/0.jpg)](http://www.youtube.com/watch?v=wH9HhmbfnhA "")
