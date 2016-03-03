---
author: teemuharjublog
comments: true
date: 2006-05-15 05:44:38+00:00
layout: post
slug: so-it-is-google-talk-then
title: So it is Google Talk then…
wordpress_id: 144
categories:
- Nokia Tablets
---

The biggest news for the last couple of days in terms of [Nokia 770](http://www.nokia.com/770) has been the new VoIP software update that supports [Google Talk](http://talk.google.com). It has been announced as a some huge co-operation deal between Nokia and Google. Well, I understand that it is a great way to get lots of free press and I surely love to see more and more people get interested in this great device, but Google Talk has been open for quite some time already. Anyone can use their [liblingle](http://code.google.com/apis/talk/index.html) to implement a Google Talk client. So I think it is not a big surprise to see Nokia 770 support Google Talk.

For a long time now I've been very pro-SIP, probably due to my engineer background. At first, when I saw GTalk using Jabber I was thinking that what the hell is Google doing here. Currently, I believe that their approach was the right one. SIP is getting far too complex and SIP presence solution is lagging seriously behind. This is probably why [Gizmo](http://www.gizmoproject.com) is using a double stack (SIP for voice and Jabber for presence) and Google's solution was to implement voice features for Jabber.

Google Talk is probably the best way to go with the Nokia 770. Currently Google has the largest "open" community and a client implementation that works. However, I bet that the new software supports also SIP. If you take a look at the [maemo roadmap](http://www.maemo.org/platform/docs/roadmap.html) you see that they are using [Telepathy](http://telepathy.freedesktop.org/wiki/) and [Farsight](http://farsight.sourceforge.net/) to implement the VoIP. Both of them supports [Nokia's Sofia SIP stack](http://sofia-sip.sourceforge.net/).

From these news it is quite clear that we don't have to wait the new software for too long anymore. And as you can see from the roadmap, the VoIP & IM are not the only new additions to the software. Probably even more than the VoIP, I'm waiting for the better support for [Python](http://www.python.org). ;)
