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
