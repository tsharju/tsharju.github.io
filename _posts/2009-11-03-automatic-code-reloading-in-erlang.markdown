---
author: teemuharjublog
comments: true
date: 2009-11-03 13:24:48+00:00
layout: post
slug: automatic-code-reloading-in-erlang
title: Automatic code reloading in Erlang
wordpress_id: 282
categories:
- Erlang
- Programming
tags:
- Erlang
- Programming
- PubSubHubbub
---

I've recently got back to coding [Erlang](http://www.erlang.org) and noticed a neat module that I didn't know existed that is probably worth writing a blog entry about. I've started developing a [PubSubHubbub](http://code.google.com/p/pubsubhubbub/) hub in Erlang called [Hubbabubba](http://github.com/tsharju/hubbabubba) and I'm using the great [Mochiweb HTTP library](http://code.google.com/p/mochiweb/) as the HTTP server implementation. I discovered the [reloader.erl module](http://code.google.com/p/mochiweb/source/browse/trunk/src/reloader.erl) that comes with Mochiweb. It automatically reloads the code when you have the application running and you modify the code (remember to compile as well). This is something that I've found very useful when developing with [Django](http://www.djangoproject.com/) or [AppEngine](http://code.google.com/appengine/docs/whatisgoogleappengine.html) and I'm really satisfied that there is a similar solution for Erlang as well.
