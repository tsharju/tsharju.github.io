---
author: Teemu Harju
comments: false
date: 2009-02-08 07:48:25+00:00
layout: post
slug: using-zcbuildout-in-a-twisted-project
title: Using zc.buildout in a Twisted project
wordpress_id: 248
categories:
- Programming
- Python
tags:
- Python
- twisted
- zc.buildout
---

I've been learning how to use
[zc.buildout](http://pypi.python.org/pypi/zc.buildout) in my Python
projects. It seems to be used mostly by the
[Zope](http://www.zope.org/) and the [Plone](http://plone.org/)
communities, but I feel that it might be a useful tool to be used in
any Python project. I do lots of work with
[Twisted](http://twistedmatrix.com/trac/) and I wanted to try
zc.buildout with a Twisted project. It wasn't trivial and I wasn't
able to find any good examples, so I thought I might as well document
my experiences here.

Let's start this exercise by setting a new virtual Python environment
using [virtualenv](http://pypi.python.org/pypi/virtualenv). It gives
you a nice constrained Python environment that does not mess up your
system. Easiest way to get virtualenv on your system is by using
[setuptools](http://pypi.python.org/pypi/setuptools). When you have
setuptools installed you can get virtualenv by typing:

{% highlight shell%}
$ easy_install virtualenv
{% endhighlight %}

Now that you have virtualenv installed it's time to create the virtual
environment. In this example I will create the environment in a
directory called `twistedenv`. You can place it anywhere you want in
your system.

{% highlight shell %}
$ mkdir twistedenv
$ virtualenv --no-site-packages twistedenv/
New python executable in twistedenv/bin/python
Please make sure you remove any previous custom paths from your /Users/teemu/.pydistutils.cfg file.
Installing setuptools............done.
{% endhighlight %}    

Now that the virtual environment is setup you can activate it by typing:

{% highlight shell %}
$ cd twistedenv
$ source bin/activate
{% endhighlight %}

This will activate the new Python interpreter that was installed in
`twistedenv/bin/python`.

Ok, now we can get to the actual topic, that is how to use zc.buildout
with a Twisted project. What you will need is a bootstrap script that
will install zc.buildout inside your environment and a buildout
configuration file. Let's start with the configuration file, since we
need that before running the bootstrap script. In zc.buildout the
configuration file is called `buildout.cfg`. It will tell the buildout
system, what packages are needed and setup your development system in
a blink of an eye. I'll start with an example that will install
Twisted and some dependencies and a sample Twisted application that is
under development. Here's the `buildout.cfg` that will do this.

{% highlight config %}
[buildout]
parts =
depends
twisted
twisteds
develop = ./src

[versions]
Twisted = 8.2.0
pyOpenSSL = 0.8
pyserial = 2.4
pycrypto = 2.0.1

[depends]
recipe = minitage.recipe:egg
eggs =
pyOpenSSL
pyserial
pycrypto

[twisted]
recipe = minitage.recipe:egg
eggs = Twisted

[twisteds]
recipe = minitage.recipe:scripts
interpreter = twistedpy
extra-paths = ${buildout:directory}/src
eggs =
${twisted:eggs}
${depends:eggs}
{% endhighlight %}

I'll briefly explain what it does. Section `buildout` is the first
thing to put in the config file. It lists what parts of the
configuration file are used and the location of the code that is being
developed. The section called `depends` installs the dependencies for
Twisted using recipe `minitage.recipe`. Section `twisted` naturally
installs Twisted, but it does not install the scripts that are
installed to the `bin` directory. That is what the section `twisteds`
is for. By the way, you are free to choose the names of the sections
or parts except for the `buildout` section. In the `twisteds` section
we also define the variable `interpreter` to create us a new Python
interpreter that will be used when running these scripts. For this
interpreter we define which eggs are included in the interpreter path
and using `extra-paths` variable we can include our development code
there also. It is useful that we can keep our own code separate from
3rd party code while developing it. Section `versions` can be used to
specify which versions of 3rd party libraries we want to use. If you
don't specify a version, a newest version available is used.

Now that we have the `buildout.cfg` file ready, we can bootsrap the
buildout. Download the bootstrap script from [this
URL](http://svn.zope.org/*checkout*/zc.buildout/trunk/bootstrap/bootstrap.py)
and put it in the `twistedenv` directory. Before running the bootstrap
script you can make sure that you are using the Python interpreter
from the virtual environment to run it instead of the syste Python. Do
that by typing:

{% highlight shell %}
$ which python
/Users/teemu/Programming/twistedenv/bin/python
{% endhighlight %}

As you can see, for me it shows that I'm using the Python interpreter
from inside the `twistedenv` where we created the virtual
environment. That is good. Naturally, for you the complete path is
something else. Then we can run the bootstrap script by typing:

{% highlight shell %}
$ python bootstrap.py
Creating directory '/Users/teemu/Programming/twistedenv/parts'.
Creating directory '/Users/teemu/Programming/twistedenv/eggs'.
Creating directory '/Users/teemu/Programming/twistedenv/develop-eggs'.
Generated script '/Users/teemu/Programming/twistedenv/bin/buildout'.
{% endhighlight %}

As you can see from the output the script created couple of
directories and generated a script called `buildout` to the `bin`
directory. Now our buildout environment is almost ready. The only
thing is missing is that our `./src` directory does not contain any
code. You will see an error if you now try to run `bin/buildout`.

Let's create a quick sample Twisted application to demonstrate a more
complete buildout environment. This is what we need. Create the
required directories for our code:

{% highlight shell %}
mkdir -p src/sample
mkdir -p src/twisted/plugins
touch src/sample/__init__.py
{% endhighlight %}

Then we need the following files:

`src/sample/tap.py`:

{% highlight python %}
from twisted.application import internet
from twisted.web import server
from twisted.web.resource import Resource
from twisted.python import usage

class SimpleResource(Resource):
isLeaf = True
def render_GET(self, request):
    return "<h1>It Works!</h1>"


class Options(usage.Options):
    pass


def makeService(config):
    site = server.Site(SimpleResource())
    return internet.TCPServer(8080, site)
{% endhighlight %}

`src/twisted/plugins/sample.py`:

    
{% highlight python %}
from twisted.application.service import ServiceMaker
    
Sample = ServiceMaker(
    "Sample",
    "sample.tap",
    "Twisted sample application",
    "sample"
)
{% endhighlight %}

And last, but not least, `src/setup.py`:

{% highlight python %}
from setuptools import setup

setup(name='sample',
      version='0.0.1',
      description='Sample Twisted application',
      author='Teemu Harju',
      author_email='teemu.harju@gmail.com',
      url='http://blog.teemu.im',
      packages=['sample'],
      package_data={'twisted.plugins': ['twisted/plugins/sample.py']},
      zip_safe=False
)
{% endhighlight %}

These files will create a Twisted application called `sample` that
runs a webserver on port `8080` that responds "It works!" when the
root URL is accessed.

Now, run:

{% highlight python %}
$ bin/buildout
...
{% endhighlight %}

I've not put the output to the example since there is so much of
it. But after the buildout is ready and if everything worked fine you
can test that your environment is ready by running the `bin/twistd`
script like this:

{% highlight shell %}
$ bin/twistd --help
{% endhighlight %}

The output should contain our `sample` application and you can run it
by typing:

{% highlight shell %}
$ bin/twistd -n sample
{% endhighlight %}

Now you can start developing your Twisted application and test it in
your safe and restricted environment using virtualenv and buildout. By
the way, if you want to get out of your virtual Python environment you
can do that simply by typing:

{% highlight shell %}
$ deactivate
{% endhighlight %}

It would be probably good to include some kind of a test runner in the
buildout script to make sure that your buildout is not broken. How
that can be done is probably a blog post topic of it's own. That's all
for now. Have fun with buildout and Twisted. Feel free to ask in the
comments if this didn't work for you for some reason.
