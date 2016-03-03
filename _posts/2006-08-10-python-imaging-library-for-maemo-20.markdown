---
author: teemuharjublog
comments: true
date: 2006-08-10 15:21:25+00:00
layout: post
slug: python-imaging-library-for-maemo-20
title: Python Imaging Library for Maemo 2.0
wordpress_id: 160
categories:
- Nokia Tablets
- Programming
- Python
---

As I got things going now, I decided to port (or more like package) the [Python Imaging Library](http://www.pythonware.com/products/pil/) for the [Maemo 2.0](http://www.maemo.org) platform. I decided to take the 1.1.6 beta release, since the 1.1.5 had some annoying bugs that made e.g., the pildriver.py script unusable. I hope that the 1.1.6 is more stable. You can get the deb packages for both armel and i386 from [here](http://www.saunalahti.fi/~tsharju/pymaemo).

Besides being a usefull Python library, you can use the included scripts to modify images on your 770. For example you can use the pildriver.py script to resize an image (larger.jpg) to size 320x240 and save it to a separate file (smaller.jpg) by typing the following line in terminal window...


    
    pildriver.py save smaller.jpg resize 320 240 open larger.jpg
