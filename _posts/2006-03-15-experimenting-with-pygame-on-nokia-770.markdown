---
author: teemuharjublog
comments: true
date: 2006-03-15 04:45:57+00:00
layout: post
slug: experimenting-with-pygame-on-nokia-770
title: Experimenting with Pygame on Nokia 770
wordpress_id: 126
categories:
- Nokia Tablets
- Programming
- Python
---

It's been a while since my last post to my blog and yesterday, while watching OC and lying on the couch, I thought I gotta post something. Then I started to browse through the [Pygame site](http://pygame.org) since I thought there might be something cool stuff that I could try on my Nokia 770.

First I started at the [Gamelets section](http://www.pygame.org/gamelets/). I thought that these should be simple enough and work without any editing on the device. To be honest, that was not the case, but at least I got this small game called Spacepong working. It worked really nice and I could control the ball on the screen using the stylus. The game itself is not the coolest kind, but might offer a good example on how to develop games using Pygame. Here are some screenies from the game taken from Nokia 770.

[![Spacepong menu on Nokia 770](http://static.flickr.com/47/117486307_dc89add6ea_m.jpg)](http://static.flickr.com/47/117486307_dc89add6ea_o.png)

[![Spacepong game on Nokia 770](http://static.flickr.com/55/117484318_c7cbd216cd_m.jpg)](http://static.flickr.com/55/117484318_c7cbd216cd_o.png)

Now, encouraged from this nice experience with Pygame gamelets, I decided to go for the [actual games](http://pygame.org/projects/6). Since I've always liked [Arkanoid](http://en.wikipedia.org/wiki/Arkanoid) I immediately wanted to try Pygame version of that called [Nannoid](http://www.imitationpickles.org/nannoid/). To my surprise, it also worked right out of the package. Well not fully. As you can see from the screenshots below, the game didn't quite fit to the screen. Anyhow, this seems like something that would be really cool with some modifying.

[![Nannoid menu on Nokia 770](http://static.flickr.com/38/117484317_2c953b8d4c_m.jpg)](http://static.flickr.com/38/117484317_2c953b8d4c_o.png)

[![Nannoid game on Nokia 770](http://static.flickr.com/46/117484314_20430adbc1_m.jpg)](http://static.flickr.com/46/117484314_20430adbc1_o.png)

And remember people, this was all done while watching OC. ;) All you need to do is to install [Python for Maemo](http://pymaemo.sf.net). Then download the .tar.gz packages of the games, for example, to your memory card. Then go to the directory where you downloaded them using Xterm (MMC is found in /media/mmc1) and type "tar xzvf the_name_of_the_package.tar.gz". Change to the directory that was extracted and start the game by typing "/var/lib/install/usr/bin/python2.4 the_name_of_the_file_that_starts_the_game". If you have errors, you might have tried to start the wrong file or it just doesn't work. If you find more games that would be cool for Nokia 770, I'd like to hear about it. That was all this time... happy gaming. :)
