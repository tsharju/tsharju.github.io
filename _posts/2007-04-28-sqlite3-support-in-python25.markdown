---
author: teemuharjublog
comments: true
date: 2007-04-28 09:07:19+00:00
layout: post
slug: sqlite3-support-in-python25
title: SQLite3 support in Python2.5
wordpress_id: 181
categories:
- Nokia Tablets
---

I was sort of porting PySQLite for Python2.5 since I got couple of requests from people who needed that to use SQLite in their Maemo Python applications. I had already packaged PySQLite and I was just accidentally going through the sources of Pymaemo and I noticed that Python2.5 package depends on libsqlite3. I was sort of wondering why that is and only then I remembered that SQLite3 support is there in Python2.5 by default. :) The [sqlite3 module documentation](http://docs.python.org/lib/module-sqlite3.html) shows good examples on how to use it.
