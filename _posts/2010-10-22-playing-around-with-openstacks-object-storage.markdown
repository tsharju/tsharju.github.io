---
author: Teemu Harju
comments: true
date: 2010-10-22 01:51:56+00:00
layout: post
slug: playing-around-with-openstacks-object-storage
title: Playing Around With Openstack's Object Storage
wordpress_id: 342
categories:
- Erlang
- Programming
- Python
tags:
- Erlang
- Openstack
- Programming
- Python
- Swift
---

Couple of weeks ago I found out about the [Openstack
project](http://openstack.org) and I found it immediately to be very
interesting. What I've been playing around with the most is the
[object storage](http://www.openstack.org/projects/storage/) part of
Openstack called [Swift](http://swift.openstack.org/). I'll show here
how you can use Swift with a couple of different libraries. The nice
thing about Swift is that it is basically the [Rackspace
Cloudfiles](http://www.rackspacecloud.com/cloud_hosting_products/files)
storage, so the same libraries that work with Cloudfiles, should work
with Swift as well. Well, they require some small modifications. But,
I'll show you here two libraries that I know are working already. Of
course, you will need a Swift instance running somewhere and
instructions on how to setup one you can read the ["Swift All In
One"](http://swift.openstack.org/development_saio.html) document that
shows how you can run Swift on a single server.

The first library I'll show here is the
[python-cloudfiles](http://github.com/rackspace/python-cloudfiles). I
recommend using the latest one from Github, since the one that you can
get for example from Ubuntu repositories does not support Swift and
the one you can get from [Python Package
Index](http://pypi.python.org/pypi/python-cloudfiles/1.7.0) had a bug
that made it not work with Swift.

Here I'll show you how you can connect to your local Swift instance
using the `authurl` parameter and how you can create containers and
objects using `python-cloudfiles`.

{% highlight python %}
from cloudfiles.connection import Connection
 
conn = Connection("test:test", "test", authurl="http://127.0.0.1:11000/v1.0")
 
container = conn.create_container("test")
 
obj = container.create_object("test.txt")
obj.content_type = "text/plain"
obj.write("test")
{% endhighlight %}

Pretty straightforward... right?

Next, I'll show you another library that works with Swift called
[cferl](http://github.com/ddossot/cferl). It's a Erlang library for
Cloudfiles and I made some [simple
patches](http://github.com/ddossot/cferl/commit/38689bebcc0f229b3c75d113aed9532da745667d)
to it to make it work with Swift.

Here's how you can do the same things as in previous example using
`cferl`.

{% highlight erlang %}
ibrowse:start().
{ok, Connection} = cferl:connect("test:test", "test", "http://127.0.0.1:11000/v1.0").
 
{ok, Container} = Connection:create_container(<<"test">>).
 
{ok, Object} = Container:create_object(<<"test.txt">>).
ok = Object:write_data(<<"test">>, <<"text/plain">>).
{% endhighlight %}

Ok, that's it. Now you can start playing with Swift and storing
petabytes of data in it.
