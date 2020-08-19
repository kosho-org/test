# hls-46-plugin
This is a plug-in for hls.js to compare speeds of IPv4 and IPv6 session and choose faster one. 

#Usage
Under the currnet implementation, you must prepare 2 URls for IPv4 and IPv6 (for example, https://ipv4.example.com/media00.ts, https://ipv6.example.com/media01.ts). Also the subdomians must be ipv4 and ipv6.


```
<html>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/hls.js@0.14.1"></script>
  <video id="video" controls preload="none"></video>
  <script src="hls-46.js"></script>
  <script src="hls-customload.js"></script>

  <script>
    var video = document.getElementById('video');
    var videoSrc = 'tears-2m-2-mixed.m3u8';
  
    var customLoader = function() {};
    customLoader.prototype = new Hls.DefaultConfig.loader();
    customLoader.prototype.loadInternal = customLoadInternal;

    init46(0,"http://ipv6.jpcdn.jp/beacon.php",
      "http://ipv4.jpcdn.jp/","http://ipv6.jpcdn.jp/",
      "https://ipv4.jstream.jp/media/hls/","http://ipv6.jstream.jp/media/hls/");

    var hls = new Hls({loader: customLoader});
    hls.loadSource(videoSrc);
    hls.attachMedia(video);
    
    var track = video.addTextTrack("captions","test","en");
    track.mode = "showing";

    if (window.VTTCue) {
      cue = new VTTCue(0,999,"Please play video, you will see download speeds");
      track.addCue(cue);
    }
  </script>
</html>

```

##Mode

#Files
- Client
 - hls-46.js
  - main plug-in
 - hls-customload.js
  - own customLoadInternal(media download handler) for hls.js. 
- Server (some codes for reporting)
 - beacon.php
  -
 - ip.php
  -
 - sid.php
  -
 - uid.php
  -



#Sample

