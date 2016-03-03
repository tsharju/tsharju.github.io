---
author: teemuharjublog
comments: true
date: 2006-05-20 07:09:47+00:00
layout: post
slug: the-original-bittorrent-on-nokia-770
title: The original BitTorrent on Nokia 770
wordpress_id: 146
categories:
- Nokia Tablets
- Programming
- Python
---

As the readers of this blog have probably noticed, I like doing stuff with [Python](http://www.python.org). I also like downloading all kinds of stuff from the Internet and what better way to do that nowadays than the [BitTorrent](http://www.bittorrent.com). How convenient, BitTorrent has originally been written in Python. ;) Here we go... I need to port the BitTorrent client on Maemo platform using [pymaemo](http://pymaemo.sf.net). I know there are command line versions of some other BitTorrent clients already available for maemo, but I kind of like having UIs. They make life alot easier. ;)

The surrent stable version of the BitTorrent client uses [PyGtk](http://www.pygtk.org) for the user interface so porting should go quite smoothly. I decided to give it a go and as I suspected it worked straight from the package. Of course I had to modify the .deb installer a bit to make it install using the application installer on the device, but otherwise nothing else was needed.

Here are couple of screenshots. As you can see, no Hildonization for the UI yet, but I'm already rolling up my sleeves so lets see when I can release the first installer package.

[![Maemo BitTorrent main screen](http://static.flickr.com/54/149660070_d98e220d89_m.jpg)](http://www.flickr.com/photos/teemu/149660070/)

[![Maemo BitTorrent about screen](http://static.flickr.com/46/149660071_642c79c094_m.jpg)](http://www.flickr.com/photos/teemu/149660071/)
