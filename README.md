# StreamOrNot

##A set of testing tools to determine how well your video will stream.

##Sample Usage:  

1. Open the [Stream or not](https://dougsillars.github.io/StreamOrNot/) page.
2. A video from [NASA](https://nasa.gov/multimedia/hd/index.html) will begin to play.
3. You'll notice a number of Stats accumulating below the video on playback.
4. When the video completes, a summary is presented
5. Test the video at different network speeds using [devTools](https://css-tricks.com/throttling-the-network/) or a proxy like Charles.

##Advanced Features
1. Add 'video' query parameter to the url pointing to your video to get stats on how well your video plays back
> https://dougsillars.github.io/StreamOrNot/?video=https://res.cloudinary.com/dougsillars/video/upload/v1533591785/depend_p2ryou.mp4
2. Videos rarely load alone on the web, and there are other assets competing for bandwidth.
..1. Add 'imagecount' query parameter.  Values can be from 0 to 99.  This will download the specified number of images while the video is playing back.
3. Oh, and JavaScript parsing will be going on - slowing down the browser, CPU, etc.
..1. Add js=true query parameter.  This will compute a recursive fibonacci every 250ms during playback to simulate JS execution.


###What Does It All Mean?

![screenshot of top entries](/images/View1.png)

1. The bar at the top shows how far the playback has gone (green), and the amount of video in the buffer (gray).  Code borrowed from [MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/Audio_and_video_delivery/buffering_seeking_time_ranges)
2. On Chrome, the connection speed is shown (navigation.connection is Chrome only)
3. Video playback state. Possible options are 
..1. Not Started
..2. Playing
..3. waiting
..4. stalled
..5. empty buffer
..6. ended
4. Video Start time.  Measured from start of JS execution to when the video begins playing.  The longer this takes, the text color changes to yellow and then red.
5. Playback time (in seconds)
6. Remaining time in buffer (in seconds)


![buffering chart](/images/buffer1.png)

This chart shows the amount of video in the downloaded buffer (video that is local and ready to play.)  The more video in the buffer, the less likely there will be a stall.  IN the chart, if there is more than 10s of video in the buffer, the line is green. If there is between 5-10s in the buffer, the line is yellow (low).  If there is less than 5s in the buffer, the line is red (danger).

Where did I come up with these metrics for danger and low? I made a good educated guess.

![buffering chart with very low levels](/images/stall1.png)
When the buffer level is really low, there is a possibility that it will empty, and the video will stall.

