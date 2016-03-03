---
author: teemuharjublog
comments: true
date: 2006-02-24 07:19:20+00:00
layout: post
slug: porting-python-modules-to-maemo
title: Porting Python modules to Maemo
wordpress_id: 123
categories:
- Nokia Tablets
- Programming
- Python
---

In some of my earlier Python related postings someone asked if [Python Imaging Library (PIL)](http://www.pythonware.com/products/pil/index.htm) has been ported for Maemo. It seemed like it is not, so I decided to give it a try. I've never before created any Python .deb packages so, this was sort of my first experiment in this area and I decided to write here how I did it. In case it would be of use to someone.

The created PIL package for Maemo can be downloaded from [here](http://www.teemuharju.net/maemo/). Anyone interested in porting [Comix](http://comix.sourceforge.net/) for Maemo now? One point I need to mention is that I have not tested the PIL library that I've created for performance and quite frankly I have no idea how to optimize it, so if anyone is interested just go ahead.

I made a test using PIL on my Nokia 770 and created a blended image of the two default wallpapers. Here's the result. Keep on reading if you want to know how I ported the library for Maemo.

[![Blended image created with PIL](http://static.flickr.com/53/129920475_bede045674_m.jpg)](http://static.flickr.com/53/129920475_bede045674_o.jpg)

<!-- more -->

Ok, first of all you need to download the PIL source package from [here](http://effbot.org/downloads/Imaging-1.1.5.tar.gz). Remember that you have to use the Maemo SDK and use the ARM target when compiling. Use "sbox-config -st SDK_ARM" to change to ARM target if you aren't using that yet. Also, you need to have the ARM version of [Pymaemo](http://pymaemo.sf.net) installed.

Let's get to the business now. Unpack the PIL source package...

    
    
    [sbox-SDK_ARM: ~] > tar xvfz Imaging-1.1.5.tar.gz
    



Then go to the created Imaging-1.1.5/ directory and "compile" the PIL module, by giving the following command...

    
    
    [sbox-SDK_ARM: ~/Imaging-1.1.5] > python2.4 setup.py bdist_dumb
    



Wait until it finishes and then move to the created dist/ directory. Unpack the packet in that directory.

    
    
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist] > tar xvfz PIL-1.1.5.linux-armv5tel.tar.gz
    



Now, create a new directory under the Imaging-1.1.5/dist/ directory. The name of this directory will be the name of the .deb package also.

    
    
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist] > mkdir pil-1.1.5.maemo_arm
    



Move the files that were inside the PIL-1.1.5.linux-armv5tel.tar.gz packet to the newly created directory. In this case it will only be the usr/ directory.

    
    
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist] > mv usr/ pil-1.1.5.maemo-arm/
    



Change to that directory and create there a directory called "DEBIAN". Go to that directory.

    
    
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist] > cd pil-1.1.5.maemo-arm/
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist/pil-1.1.5.maemo-arm] > mkdir DEBIAN
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist/pil-1.1.5.maemo-arm] > cd DEBIAN/
    



In this debian directory you need to create a file named "control". The application installer will use this file to regognize the package. Use some text editor and type something like this.

    
    
    Package: pil
    Version: 1.1.5.maemo
    Section: unknown
    Priority: optional
    Architecture: arm
    Depends: maemo, pymaemo-runtime
    Installed-Size: 3024
    Maintainer: Your Name 
    Description: Python Imaging Library
    



Important part is the "Depends: maemo" line, since the package won't install to the device if this is not there. For python packages it is good to also include the "pymaemo-runtime" in the depends clause, so that the installer won't install the packages unless pymaemo is also installed. Other lines in the "control" file are quite self-explatory. _PS. leave out the spaces around the email address. This silly blog wouldn't have shown it otherwise._

Then you are basically ready to create the .deb package. Just to make sure, if the library contains executable python files, you need to check the path of the python interpreter in those files. It is the first line in the file and most likely you need to add the /var/lib/install part in front of the line to make it work on the device. Executables are usually located in usr/bin directory. Here is an example...

Change this...

    
    
    #!/usr/bin/python2.4
    



To this..

    
    
    #!/var/lib/install/usr/bin/python2.4
    



Now you can finally create the .deb package. To do that you have to be in the Imaging-1.1.5/dist/ folder and give this command.

    
    
    [sbox-SDK_ARM: ~/Imaging-1.1.5/dist] > dpkg-deb --build pil-1.1.5.maemo-arm
    



Then wait a sec and your package will be ready to install on the device. That's it!

Again, questions and comments are very welcome.
