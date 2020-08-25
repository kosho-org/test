# hls-46-plugin
This is a plug-in for hls.js to compare speeds of IPv4 and IPv6 session and choose faster one. 

# Mode

- mode 0 (simple speed comparision)
- mode 1 (speed comparision)
- mode 2 (initital check)
- mode 3 (continous check)

# Files
## Client
- hls-46.js
  - main plug-in
- hls-customload.js
  - own customLoadInternal(media download handler) for hls.js. 
## Server (codes for reporting)
- beacon.php
  - receive JSON and write it to log
- ip.php
  - return client's ip address
- sid.php
  - return UUID
- uid.php
  - return UUID
## tool 
- set-mixed-m3u8.pl
  - make a full path m3u8
    - perl set-mixed-m3u8.pl < sunrise-2.m3u8 https://ipv4.jstream.jp/media\
/hls/sunrise-2/ https://ipv6.jstream.jp/media/hls/sunrise-2/ > sunrise-2-mixed.\
m3u8
- ts-size-out.pl
  - make a filesize js file
    - cd <ts file directory>
    - perl ts-size-out.pl  > sunrise-2-filesize.js
  
# Usage

## Mode 0 (Simple speed comparision)
```
<html>
  <script src="https://cdn.jsdelivr.net/npm/hls.js@0.14.1"></script>
  <video id="video" controls preload="none"></video>
  <script src="hls-46.js"></script>
  <script src="hls-customload.js"></script>

  <script>
    var video = document.getElementById('video');
    var videoSrc = 'tears-2m-2.m3u8';
  
    var customLoader = function() {};
    customLoader.prototype = new Hls.DefaultConfig.loader();
    customLoader.prototype.loadInternal = customLoadInternal;

    hls46init(0,"http://ipv4.example.com/","http://ipv6.example.com/","","");

    var hls = new Hls({loader: customLoader});
    hls.loadSource(videoSrc);
    hls.attachMedia(video);

    hls46TextShow();
  </script>
</html>

```



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



