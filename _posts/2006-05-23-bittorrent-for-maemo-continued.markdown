---
author: teemuharjublog
comments: true
date: 2006-05-23 06:32:41+00:00
layout: post
slug: bittorrent-for-maemo-continued
title: BitTorrent for Maemo continued
wordpress_id: 148
categories:
- Nokia Tablets
- Programming
- Python
---

Ok, now I've got the UI of the BitTorrent client somewhat Hildonized. I decided to move the start/stop button, the search field and the connection indicator to a toolbar at the bottom of the view. I think this makes it a bit more usable. I'm considering moving the upload rate adjust bar to the toolbar also, so that the main view would only contain the information about the torrents that are being downloaded or uploaded. I haven't yet touched the other views, but I think those need less editing anyways.

The search bar on the bottom does not of course work yet, since it is not possible to launch the web browser form Python applications yet. However, this should become possible with the new software release and new pymaemo version. Then also MIME support for bittorrent files would be nice. I'll look into that and then we have full blown BitTorrent for the Maemo platform. All I would need now is actual content distributed via BitTorrent that actually could be used in the Nokia 770. Usually videos are too large and those iPod mp4 videos does not seem to play in the video player. Anyone know any good podcasts that are distributed using BitTorrent?

[![Maemo BitTorrent](http://static.flickr.com/46/151726823_9c87289125_m.jpg)](http://www.flickr.com/photos/teemu/151726823/)
