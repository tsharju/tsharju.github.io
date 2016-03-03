---
author: teemuharjublog
comments: true
date: 2005-08-03 07:38:10+00:00
layout: post
slug: something-flickr-on-my-mobile
title: Something Flickr on my mobile
wordpress_id: 48
categories:
- Programming
- Python
- Series 60
---

Yesterday evening I started playing around with the [Python for Series 60](http://www.forum.nokia.com/main/1,6566,034-821,00.html) installation on my Nokia 6630. I've always liked to code in [Python](http://www.python.org). There is just that something in that language that I like. And I like the fact that the name comes from Monty Python. Anyway...  I started experimenting Python for Series 60 using [the Flickr API](http://www.flickr.com/services/api/). I chose to use [XML-RPC](http://www.xml-rpc.com/) to connect to the API since that seems like the most simple solution to use.

The first problem naturally came from the fact that there is no [xmlrpclib module](http://www.pythonware.com/products/xmlrpc/) installed in current Python for Series 60 release. Anyway I managed to get it working by taking the xmlrpclib and [xmllib](http://docs.python.org/lib/module-xmllib.html) modules from [Python 2.2](http://www.python.org/2.2.3/) release and putting them in my phones python folder along with my test script. So now XML-RPC works and I can connect to Flickr API.

Then there was more problems. Flickr API is kind of limited in that sence that when one is using XML-RPC to connect you get the replies back in XML format and not in XML-RPC format. The latter format would be a lot easier because now I need also an XML parser to parse the reply from Flickr API. Ok... I've got the xmllib module working, but that is not the best solution for parsing XML in Pyhon. I did some experimenting with [pyexpat](http://pdis.hiit.fi/pdis/download/pyexpat/), but couldn't get it working yet with the Series 60 emulator so couldn't decide yet whether to use it or not. If only Flirck would send those XML-RPC replies in correct format.

So the project continues... I'll keep you posted.
