---
author: teemuharjublog
comments: true
date: 2005-12-11 16:37:35+00:00
layout: post
slug: stream-your-mp3-files-with-nokia-770
title: Stream your mp3 files with Nokia 770
wordpress_id: 84
categories:
- Nokia Tablets
---

Since the storage capacity on Nokia 770 is rather limited, I thought that it would be nice to stream the mp3s I have on my Linux server over WLAN. After little Googling I found ajax/PHP based application called [mp3act](http://www.mp3act.net). I installed it on my server and it worked fine with Mozilla Firefox on my laptop. I created playlists from the songs on my server and succesfully streamed them using Windows Mediaplayer. Then I tried it with my 770 and disappointment was rather big when I noticed that it doesn't work. It appears that the ajax support on the N770's Opera browser isn't fully working. Well I didn't want that to prevent me from streaming my mp3s. I found a workaround for the problem and I'm streaming music while typing this. Read the rest of this post if you are interested in how this can be done.
<!-- more -->




  1. Download and install [mp3act](http://www.mp3act.net). See the [requirements](http://www.mp3act.net/requirements/) first.


  2. Using your computer's browser log into the mp3act. (mp3act doesn't work on 770's browser).

  3. Add your favourite albums using mp3act's admin panel. Remember that for this to work the web server needs to have read access to the mp3 files.


  4. Create a playlist from the files you want to listen on your N770.


  5. Press "play" to play the list, but instead of opening the playlist, save it to your computer and copy/move the file to your 770.


  6. Open the file on 770 with the audio player and enjoy the music.


Lets hope that the ajax support will be improved in the future updates so that mp3act could be fully used. Then I could really have all my mp3s everywhere I go.
