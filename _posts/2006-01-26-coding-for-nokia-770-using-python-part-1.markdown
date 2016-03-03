---
author: teemuharjublog
comments: true
date: 2006-01-26 13:59:48+00:00
layout: post
slug: coding-for-nokia-770-using-python-part-1
title: Coding for Nokia 770 using Python - Part 1
wordpress_id: 108
categories:
- Nokia Tablets
- Programming
- Python
---

I thought that I start a series of posts giving some tips on how to get started to code applications for [Nokia 770](http://www.nokia.com/770) using [Python](http://www.python.org). I have some experience on coding [Pyhon for Series 60](http://opensource.nokia.com/projects/pythonfors60/index.html) and now I've started to play with the [Python for Maemo](http://pymaemo.sf.net). My intention was to give some examples and hints how to easily develop software for Nokia 770. I'm still in the phase of learning how to use GTK/Hildon to implement user interfaces, so feel free to comment if I'm doing something totally wrong or if you have questions.

In this first part I thought that I'd give instructions how to set up an easy configuration so you can easily code Python on Nokia 770 using your PC and also very easily run your applications on the actual device. I'll also give an example of a very simple Python application that you can see below. It's not much, but it is a good start.

![Simple Python example](http://blog.teemu.im/wp-content/uploads/2006/01/pythonapp.png)

<!-- more -->

Ok, first you will naturally need to install Python for Maemo on your Nokia 770. Download it from [here](http://pymaemo.sf.net) and install it normally using the Application Installer. After installing the package, you will be able to run Python scripts on your device. If you have [Xterm](http://770.fs-security.com/xterm/) installed, you can run /var/lib/install/usr/bin/python2.4 to start the Python console and start coding.

This most probably is not the most convinient way to code. Basically you can [install Python for your computer](http://www.python.org/download/) and develop applications there. Anyhow, for testing the user interface you will need to use either the Maemo development environment or the actual device. Development environment runs only on Linux so if you don't have that, I have a simple solution for you. For this to work you will need to [enable the R&D mode on the device and become root](http://maemo.org/maemowiki/HowDoiBecomeRoot). Unfortunately you need Linux to for this, but you can use for example [Knoppix Bootable CD Linux](http://www.knoppix.org/) so you don't need to actually install it. Just burn the Knoppix image to CD and boot you PC using that.

For easy development you can [install SSH server on your Nokia 770](http://maemo.org/maemowiki/InstallSsh), so that you can connect to it using your PC. For making the SSH connection you can use for example [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html). You need to also set the password that is used when logging in. This can be sone in root mode by typing "passwd user" in Xterm.

Install also [Nano text editor](ftp://ftp.infradead.org/pub/maemo/) on your Nokia 770. You can also use [Vim](http://www.bleb.org/software/770/#vim), but I prefer Nano (It is somehow easier to use). Once you got these installed, open the WLAN connection on your Nokia 770. Check what is your IP. For that you can use Xterm (you need to be root) and run ifconfig. You can also use the [IP Home plugin](http://www.mulliner.org/nokia770/) to see your IP address. Then connect to your Nokia 770 using this IP address with Putty. Username is "user" and password is the one that you have set earlier.

Ok, now we are ready to start coding. Go to the home diredtory "cd /home/usr".Then start the nano editor and create a python file by typing "/var/lib/install/bin/nano uitest.py". Then write the following code to the file.


    
    
    #!/var/lib/install/usr/bin/python2.4
    
    import gtk
    import hildon
    
    class UITest:
    
      def __init__(self):
        self.app = hildon.App()
        self.appview = hildon.AppView("Appview Title")
        self.label = gtk.Label("My first Python App...")
    
        self.app.set_title("App Title")
        self.app.set_two_part_title(True)
        self.app.set_appview(self.appview)
    
        self.appview.connect("destroy", self.destroy)
    
        gtk.Container.add(self.appview, self.label)
    
        self.label.show()
        self.app.show()
    
      def destroy(self, widget, data=None):
        gtk.main_quit()
    
      def main(self):
        gtk.main()
    
    if __name__ == "__main__":
      uitest = UITest()
      uitest.main()
    



This code creates a similar application that is shown in the image on the top of the page. You can run it easily from your PC by typing "run-standalone.sh ./uitest.py". The application should now launch successfully on your device. This is quite convenient, since you can edit the code using your PC and then run the application on the device and you don't have to transfer any files between PC and Nokia 770.

In the future posts I'll try to explain more about what the code actually does. Meanwhile I can suggest that you check the [PyGtk](http://www.pygtk.org) site tutorial. Based on that you can do lots of cool things like add buttons etc. Also if you haven't yet checked the [Python language reference](http://www.python.org/doc/2.4.2/lib/lib.html), now is the time.

Ok... this was all this time. All questions and comments are very welcome. I hope you enjoy coding Python. ;)
