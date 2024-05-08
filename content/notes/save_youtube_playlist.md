+++
title = "Save Youtube Playlist"
date = ""
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["cli", "linux", "tricks"]
description = "How to download youtube playlists with yt-dlp"
showFullContent = false
readingTime = false
hideComments = false
color = "orange" #color from the theme settings
Toc = false
+++

This seems pretty straightforward but it took about half an hour to figure out reading through documentation and mistral chat. I tried a bunch of formats but audio wasn't working. After some time I landed at this, it also let's you save private membership videos with browser cookies.

```
yt-dlp -f "bestvideo[height<=1080]+bestaudio" --merge-output-format mp4 https://www.youtube.com/playlist\?list\=PL6W8uoQQ2c61X_9e6Net0WdYZidm7zooW --cookies-from-browser brave:Default
```

