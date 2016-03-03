---
author: teemuharjublog
comments: true
date: 2005-12-29 08:08:12+00:00
layout: post
slug: vnc-viewer-ported-for-nokia-770
title: VNC viewer ported for Nokia 770
wordpress_id: 93
categories:
- Nokia Tablets
---

Aaron Levinson writes on the [maemo-developers mailing list](http://maemo.org/pipermail/maemo-developers/):



<blockquote>
I'm pleased to announce the first version of a VNC Viewer maemo port. Some other developers have announced ports of VNC in the past, but these were strictly builds of the RealVNC vncviewer client and were not GTK/maemo software applications.  As such, they lacked certain GTK/maemo amenities, the biggest being no text input methods.

This new port is a true GTK/maemo port that basically provides most of the support that one might expect from a maemo-ized VNC viewer.  I'm still working on adding some features, and there certainly is room for optimizations, but I'm pretty happy with the way it has turned out so far.

It's available at
[http://www.aracnet.com/~alevinsn/vncviewer_0.1_arm.deb](http://www.aracnet.com/~alevinsn/vncviewer_0.1_arm.deb) .

Here's some brief documentation:
-- Press the Select/Confirm button to turn on and off the text input method window (just like in xterm).
-- Use the Zoom out (-) button for middle mouse button clicks.
-- Use the Zoom in (+) button for right mouse button clicks.
-- Use the Cancel/Close button to send an Esc key.
-- The other hardware buttons operate similar to how they operate in xterm.
-- Turn on/off the toolbar from the View menu.
-- It is possible to double-click, but it might require a little practice/multiple attempts.

I'm planning on writing one or more e-mails concerning my porting experiences.  Hopefully, the information in these e-mails will be useful to some.  I'll also be making the source code available once I clean it up a bit more.

Aaron Levinson
</blockquote>



![VNC port for Nokia 770](/wp-content/Vnc_logo.gif)
