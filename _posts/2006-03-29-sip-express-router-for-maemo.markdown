---
author: teemuharjublog
comments: true
date: 2006-03-29 15:08:18+00:00
layout: post
slug: sip-express-router-for-maemo
title: SIP Express Router for Maemo
wordpress_id: 125
categories:
- Nokia Tablets
- VoIP
---

l just finished porting [SER (SIP Express Router)](http://www.iptel.org/ser) for Maemo platform. Well, not much porting actually was needed. Just compile for ARM and create an application installer friendly .deb package.

You might be wondering that what is SER? A simple answer is that it is a very fast and configurable [SlP](http://en.wikipedia.org/wiki/Session_Initiation_Protocol) proxy server. If you still do not know what I'm talking about, this application is probably not for you. It is also a valid question that what is the use of such application for a device like [Nokia 770](http://www.nokia.com/770). Well, my intention was only to test if SER can be run on Nokia 770 and how well it works. Seems like it was easier task than what I thought. The .deb package can be downloaded from [here](http://www.saunalahti.fi/~tsharju/maemo).

The application can be used without root privileges. By default the users are not authenticated, since I've not yet had time to configure it with the dbtext module. That might come later... It is quite fun to experiment with this using SIP enabled WLAN devices and ad-hoc WLAN connections.
