---
author: teemuharjublog
comments: true
date: 2005-08-05 06:19:58+00:00
layout: post
slug: solution-to-xml-rpc-problem
title: Solution to XML-RPC problem
wordpress_id: 50
categories:
- Programming
- Python
- Series 60
---

I think I've now figured out the problem of parsing Flickr's XML-RPC responses using Python for Series 60. Mike from [Flickr mailing list](http://groups.yahoo.com/group/yws-flickr/message/159) was kind enough to point me to use [ElementTree](http://effbot.org/zone/element-index.htm) to parse the xml responses in handy tree structure to use in the application. All I need to load to my phone is xmllib which is already required by xmlrpclib and therefore installed already. To use ElementTree I need to load [ElementTree module](http://effbot.org/downloads/index.cgi/elementtree-1.2.6-20050316.zip/docs/pythondoc-elementtree.ElementTree.html). Then because the Python release doesn't include any ready parsers yet (I haven't yet tested the [pyexpat](http://pdis.hiit.fi/pdis/download/pyexpat/)) I also need to load [SimpleXMLTreeBuilder module](http://effbot.org/downloads/index.cgi/elementtree-1.2.6-20050316.zip/docs/pythondoc-elementtree.SimpleXMLTreeBuilder.html) . This should work, although I haven't had yet time to test it on the phone, but since these modules have no other dependencies, they should work fine.
