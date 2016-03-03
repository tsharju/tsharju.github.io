---
author: teemuharjublog
comments: true
date: 2008-12-28 06:41:45+00:00
layout: post
slug: python-for-series-60-v19-released-for-testing
title: Python For Series 60 v.1.9 Released For Testing
wordpress_id: 233
categories:
- Programming
- Python
- Series 60
tags:
- Programming
- Python
- s60
---

Python for Nokia's Series 60 platform has been around for four years now and to be honest, not much has happened on that front during the past year. However, suddenly on 24th of December [Nokia releases a version 1.9.0 of the Python for Series 60](http://discussion.forum.nokia.com/forum/showthread.php?t=154215) that is a major rewrite of the whole thing and comes now with Python 2.5 version of the core language. As usual, the odd-number version branch means that the release is for testing purposes only and the even-numbered 2.0 version branch should be released once it becomes more stable. [Blog at Croozeus.com](http://croozeus.com/blog) has done pretty nice [wrap up of the new release](http://croozeus.com/blogs/?p=153) and here are my thoughts.



<blockquote>First of all I should mention that as I write this blog entry I've not yet had time to test the new Python for Series 60 release. So I apologize beforehand for all false comments I'm about to make. </blockquote>



It seems that they are giving a lot more focus on the ease of development in this new release. Which is a really good thing I must say. What pleases me the most is that they are planning on improving the Python runtime deployment so that the end-user or the guy that is installing Python applications should not have to worry too much about having or not having the Python runtime installed on his phone. Definitely a good thing. The new release includes a packaging tool that is not part of the S60 SDK and is basically [ensymble](http://code.google.com/p/ensymble/) with added GUI. Ensymble is an excellent tool and it is very nice to see it included in the official PyS60 release.

Like mentioned already, the new release includes the version 2.5 of the Python interpreter and most of it's standard libraries. Ok, the word "most" does not sound good here. I'll focus first on the good things. First, **the new release has Expat XML parser in it**. Definitely a good thing since XML is something that pretty much every application out there uses and so far people have had to make ugly regexp XML parsers or use 3rd party package of expat to be able to parse XML in their PyS60 applications. However, I would say that including [JSON parser](http://docs.python.org/library/json.html) as the official Python 2.6 release did lately, would probably be a good idea too.

Also, inclusion of [asyncore](http://www.python.org/doc/2.5.4/lib/module-asyncore.html) and **more compliant socket module** sounds nice. I can't wait to try [Twisted](http://twistedmatrix.com/trac/) on PyS60 and see how it works. One could do some crazy things with that on a mobile phone.

Then to the things that are not included from the standard Python 2.5 libraries. Now, I don't have too much information on this since, like said before, I haven't tried the new release yet. However, to my disappointment I noticed that [sqlite3](http://www.python.org/doc/2.5.4/lib/module-sqlite3.html) is [not included](http://discussion.forum.nokia.com/forum/showpost.php?p=521671&postcount=2). SQLite should probably be a platform component in S60 because it is kind of becoming a de facto standard in mobile platforms since both iPhone and Android platforms use it. I don't know what is the equivalent in S60 or does such exist but having some kind of storage other than just plain text file for PyS60 applications would be extremely nice thing to have.

Ok, I have to try out the new PyS60 release soon. I'm hoping that I will be pleasantly surprised. I'll definitely write more about it later.
