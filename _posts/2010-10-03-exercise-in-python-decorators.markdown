---
author: Teemu Harju
comments: true
date: 2010-10-03 05:52:45+00:00
layout: post
slug: exercise-in-python-decorators
title: Exercise in Python Decorators
wordpress_id: 311
categories:
- Programming
- Python
tags:
- Programming
- Python
---

Recently, I found myself in need of a web server that I can use to
simulate a behavior of a certain website. I wanted to just copy the
output of that website and deliver it using this web server. The
problem was that serving static content is naturally way faster than
serving dynamic web application, so for my simulation I needed to make
the web server wait for a certain period of time before returning the
static file. Being a Python fan I decided to use
[Tornado](http://www.tornadoweb.org) as the web server. Now, all I
needed to do is slow it down.

Ok, I could just simply do this...

{% highlight python %}
time.sleep(0.5)
{% endhighlight %}

... to make my server wait half a second before returning, but since
Tornado is using asynchronous networking and hence runs in a single
thread, this would block all other requests made to my server. Not
good...

I need to do this asynchronously. Here is an example on how this can
be done with Tornado without blocking.

{% highlight python linenos %}
import time
 
import tornado.web
import tornado.ioloop
 
class RequestHandler(tornado.web.RequestHandler):
 
    def _finish_request(self):
        self.finish()
 
    @tornado.web.asynchronous
    def get(self):
        ioloop = tornado.ioloop.IOLoop.instance()
        ioloop.add_timeout(time.time() + 0.5, self._finish_request)
        self.add_header("Content-Type", "text/plain")
        self.write("Hello, world")
{% endhighlight %}

So, this is an example of an asynchronous Tornado request handler that
will wait for half a second before returning and will not block other
requests while doing that. Here we are using decorator
`tornado.web.asynchronous` on line 11 to tell Tornado that this
request should not be returned immediately and we need to call
`tornado.web.RequestHandler.finish()` on our own. The timeout is
implemented by calling `tornado.ioloop.IOLoop.add_timeout()` method
which is given a callback method that will finish the request.

Now, the problem with this is that, if I need other request handlers
to do the same thing, I would need to copy paste this peace of code
all over the place. And I don't like that. We can do this bit more
elegantly by using [Python
decorators](http://en.wikipedia.org/wiki/Python_syntax_and_semantics#Decorators). By
writing a decorator we can avoid duplicating the same code to every
request handler. This is how the same example will look using a
decorator.

{% highlight python %}
import tornado.web
import tornado.decorators
 
class RequestHandler(tornado.web.RequestHandler):
 
    @tornado.decorators.wait_for(milliseconds=500)                                               
    def get(self):                                                            
        self.set_header("Content-Type", "text/plain")                         
        self.write("Hello, world") 
{% endhighlight %}

Looks nice and clean. Doesn't it? Well, the complex part has been
moved now to the decorator `tornado.decorators.wait_for`. Let's look
now how we can implement that.

{% highlight python linenos %}
import time
 
from functools import partial
 
from tornado.web import asynchronous
from tornado.ioloop import IOLoop
 
def wait_for(milliseconds=0):
    def _finish_request(request, start_time):
        timeout = (time.time() - start_time) * 1000.0
	request.write("\n\nServer waited for %.3f ms" % timeout)
        request.finish()
    def _decorator(func):
	func = asynchronous(func)
        def _wrapper(*args, **kwargs):
            ioloop = IOLoop.instance()
            callback = partial(_finish_request, args[0], time.time())
            ioloop.add_timeout(time.time() + milliseconds / 1000.0, callback)
            func(*args, **kwargs)
	return _wrapper
    return _decorator
{% endhighlight %}

Here I have implemented the decorator `wait_for` that takes the number
of milliseconds to wait as a parameter.  Let's start from line 13
where the actual decorator is implemented. Decorator's parameter is
always the function that is being decorated. You can think of this...

{% highlight python %}
@my_decorator
def my_function():
    // do something
{% endhighlight %}

...being same as this...

{% highlight python %}
def my_function():
    // do something
my_function = my_decorator(my_function)
{% endhighlight %}

Now, on line 14 decorate the function with Tornado's
`tornado.web.asynchronous` decorator. Just like we did in the first
example. So that our request does not return before we call
`tornado.web.RequestHandler.finish()`. Then on line 15 we write a
wrapper method that adds the timeout and callback to
`tornado.ioloop.IOLoop` and after that calls the original function
that we are decorating. The trickiest part here probably is that we
need to give our callback function some parameters and Tornado's
`add_timeout` method only takes the callback function as the
parameter. For that we use Python's `functools.partial` to generate
the callback and give some parameters to it on line 17.

To conclude the blog post here is a complete example of a script that
you can test this with. You need to create the `tornado/decorators.py`
using the code above for this to work.

{% highlight python linenos %}
import time
 
import tornado.httpserver
import tornado.ioloop
import tornado.web
 
from tornado.decorators import wait_for
 
 
class MainHandler(tornado.web.RequestHandler):
 
    @wait_for(milliseconds=500)
    def get(self):
        self.set_header("Content-Type", "text/plain")
        self.write("Hello, world")
 
application = tornado.web.Application([(r"/", MainHandler)])
 
if __name__ == "__main__":
    http_server = tornado.httpserver.HTTPServer(application)
    http_server.listen(8080)
    tornado.ioloop.IOLoop.instance().start()
{% endhighlight %}

If everything goes right your server should return something like this...

{% highlight python %}
Hello, world
 
Server waited for 500.371 ms
{% endhighlight %}
