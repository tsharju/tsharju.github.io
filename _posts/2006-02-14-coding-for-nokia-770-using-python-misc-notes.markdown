---
author: teemuharjublog
comments: true
date: 2006-02-14 06:54:48+00:00
layout: post
slug: coding-for-nokia-770-using-python-misc-notes
title: Coding for Nokia 770 using Python - Misc Notes
wordpress_id: 119
categories:
- Nokia Tablets
- Programming
- Python
---

It is time to make a post about some great input that I've gotten from the readers of my blog. Thanks to everyone who has made comments. Keep on commenting people.

Ok, first of all thanks to [Kasper Souren](http://www.teemuharju.net/2006/01/26/coding-for-nokia-770-using-python-part-1/#comment-161) for pointing out that if you are having "permission denied" problems, or otherwise problems running the downloaded scripts, you sould do a "chmod +x *.py" in the folder where you have the script in.

Also big thanks to [Martin N](http://www.teemuharju.net/2006/02/08/coding-for-nokia-770-using-python-part-2/#comment-162) for giving a nice example on how to make the python script fully hildonized by adding it to the start menu and also how to get the icon to the task list while your app is running. Here's what he wrote...



<blockquote>
Hi Teemu, I’ve found a solution in the [Maemo SDK Tutorial](http://www.maemo.org/platform/docs/tutorials/Maemo_tutorial.html#app-to-task-navigator-menu):

Based on this I created the following two files for your example:

uitest.desktop:

>     
>     
>     [Desktop Entry]
>     Encoding=UTF-8
>     Version=1.0
>     Type=Application
>     Name=UITest
>     Exec=/home/user/python/uitest.py
>     X-Osso-Service=example.uitest
>     Icon=qgn_list_gene_default_app
>     
> 
> 
example.uitest.service:

>     
>     
>     [D-BUS Service]
>     Name=example.uitest
>     Exec=/home/user/python/uitest.py
>     
> 
> 
…and created those links:

>     
>     
>     ln -s /home/user/python/uitest.desktop 
>     /var/lib/install/etc/others-menu/extra_applications/
>     uitest.desktop
>     
>     ln -s /home/user/python/example.uitest.service 
>     /var/lib/install/usr/lib/dbus-1.0/services/
>     example.uitest.service
>     
> 
> 
Actually you don’t need the desktop file to have an icon in the task list, but I think it makes the hildonization complete. ;)
</blockquote>



**Edit:** Please note that the version parameter in the .desktop file refers to the __desktop entry specification version__ instead of the __application version__. Thanks to [Karo](http://www.teemuharju.net/2006/02/14/coding-for-nokia-770-using-python-misc-notes/#comment-167) for pointing this out.
