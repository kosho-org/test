# hls-46-plugin
This is a plug-in for hls.js to compare speeds of IPv4 and IPv6 session and choose faster one. 

# Usage

## Sample code
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

    init46(2,"http://ipv4.example.com/","http://ipv6.example.com/",
      "https://ipv4.media.example.com/hls/","http://ipv6.media.example.com/hls/");

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

## Mode


# Install

## Files
### Client
- hls-46.js
 - main plug-in
- hls-customload.js
 - own customLoadInternal(media download handler) for hls.js. 
### Server (some codes for reporting)
- beacon.php
  - get JSON and write it to log
- ip.php
  - return client's ip address
- sid.php
  - return UUID
- uid.php
  - return UUID

Under the currnet implementation, you must prepare folllowing URls. 

## for HTML and api
- https://ipv4.example.com/
  - https://ipv4.example.com/ip.php (to get IPv4 address by plugin)
- https://ipv6.example.com/
  - https://ipv6.example.com/beacon.php (to send statistic by plugin)
  - https://ipv6.example.com/uid.php (to get user-id by plugin)
  - https://ipv6.example.com/sid.php (to get session-id by plugin)
  - https://ipv6.example.com/ip.php (to get IPv4 address by plugin)

## for media delivery (subdomians must be ipv4 and ipv6 to undestand by plugin)
- https://ipv4.media.example.com/
- https://ipv6.media.example.com/



