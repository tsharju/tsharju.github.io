---
author: teemuharjublog
comments: true
date: 2006-02-08 07:01:20+00:00
layout: post
slug: coding-for-nokia-770-using-python-part-2
title: Coding for Nokia 770 using Python - Part 2
wordpress_id: 117
categories:
- Nokia Tablets
- Programming
- Python
---

Ok, the hugely popular _(a minor overstatement)_ "Coding for Nokia 770 using Python" tutorial continues. ;) The code for part 2 example is already 180 lines, but have no fear, I've included the example file for download and will also go through the code and try to explain what it does. Again the "tutorial" is about developing an user interface and the actual application doesn't do anything useful. It is meant to be more of an example than actual application. Example shows how to create buttons, panels, text area, simple dialogs and a menu using pygtk and hildon.

Since everyone loves images, I've included an image of the application that the example creates. To read more, click the link below the image. The example code can be downloaded from [here](http://www.teemuharju.net/code/uitest.py).

![UITest part 2](http://blog.teemu.im/wp-content/uploads/2006/02/uitest02.png)

<!-- more -->

I'll explain here some parts of the example code, that can be seen on the end of this post with lines numbered. First of all lets start with the 1st line, in case someone is wondering what it is. It is there only to tell what application should be executed when this script is run in a shell. So you'll only have to type "./uitest.py" to start the application from xterm.

Then something about the actual application. First we create a class for the application called "UITest". This is done on line 9. I'm not going further to the theory of [object oriented programming](http://en.wikipedia.org/wiki/Object_Oriented_Programming), but let's say that this is done to give a structure to the application that is easier to follow when application grows in size.

Classes always contain a constructor, that in our case is found starting from line 14. This is called each time an instance of the class (object) is created. UITest object is created on the line 179. Inside the constructor, the UITest application is created. For this you use hildon.App() and hildon.AppView() functions. Few quotes from the [maemo.org tutorial](http://www.maemo.org/platform/docs/tutorials/Maemo_tutorial.html#gui) help to understand what these are.



<blockquote>
The HildonApp is the base of any Hildon application. It does not replace the standard GTK+ main window but rather encapsulates it. It handles such things as application views and menus so it is required for every maemo application.
</blockquote>





<blockquote>
HildonAppView is an application view, which can be compared to a window in a normal window manager desktop. It is managed by managed by the HildonApp which is it's parent widget. HildonAppView usually fills the whole screen and one HildonApp can contain several HildonAppViews, which are considered being different “windows” of an application.

Each view has its own menu and toolbar, both of which can be configured to meet the needs of a particular view. For example calendar application might have a Day view, Month view and Add appointment view, which all are their own HildonAppViews and open at the same time.
</blockquote>



Inside the appview you can start adding elements. It can only contain one element, so you have to add a sort of an container element to it, that of course then can contain multiple elements. In this example we use a horizontal pane that divides the application in two parts. Pane is created on line 36, by creating a [HPaned](http://www.pygtk.org/pygtk2reference/class-gtkhpaned.html) object.

In this pane we will add [TextView](http://www.pygtk.org/pygtk2reference/class-gtktextview.html) and [VButtonBox](http://www.pygtk.org/pygtk2reference/class-gtkvbuttonbox.html) objects. In a textview you can type and VButtonBox is used to easily lay out the buttons. This is done on lines 44 and 45, by calling the functions that create and set some properties for both.

To get for example buttons to do something when they are clicked, you need to attach signals to them. An example of this can be seen on line 135 and 136 where two buttons will be connected to a signal called "clicked". You just define what function will be called when this "clicked" signal from the button is received. The functions in the example are self.about_callback and self.destroy.

To learn how to create a drop down menu for your application, look at the self.create_menu function at the line 59. The appview that was created in the constructor will be given as a parameter for this function, since it is needed to be able to create the menu for this view. Menu is created on the line 51, by calling this function.

Ok, this post is growing to be quite long already and I don't want to bore you people. Look at the example code and read the the comment lines to help you understand what is done and why. Please post comments if you have something to ask or would do things differently. To find out what ready functions pygtk already supports, I recommend checking [PyGTK 2.0 Reference Manual](http://www.pygtk.org/pygtk2reference/index.html). This was all this time. Let's hope that for the next part I'm able to create an application that actually does something. ;)


    
    
    #!/var/lib/install/usr/bin/python2.4
    
    # Imports gtk and hildon libraries so we can create
    # the user interface using those.
    import gtk
    import hildon
    
    # Creates a class called UITest
    class UITest:
    
      # This is the constructor. It is run each time
      # the UITest object is created, ie. when this
      # application is started.
      def __init__(self):
        # This is the main window of the application.
        self.app = hildon.App()
    
        # This is a view in the main window.
        # There can be multiple of these.
        self.appview = hildon.AppView("View One")
    
        # Set the view and titles to the application
        self.app.set_title("UITest")
        self.app.set_two_part_title(True)
        self.app.set_appview(self.appview)
    
        # This connects the X-mark in the top right
        # corner to the function "self.destroy"
        # that closes the application.
        self.appview.connect("destroy", self.destroy)
    
        # You can only add one widget inside one view.
        # This one widget then can of course contain
        # multiple widgets. We use a pane that divides
        # the view into two parts.
        self.hpane = gtk.HPaned()
    
        # Sets the separator in the pane to 450
        # pixels.
        self.hpane.set_position(450)
    
        # Now we add a TextView and a ButtonBox
        # to the pane.
        self.hpane.add1(self.create_textview())
        self.hpane.add2(self.create_buttons())
    
        # This adds the pane to the view.
        gtk.Container.add(self.appview, self.hpane)
    
        # Creates and shows the menu.
        self.create_menu(self.appview)
    
        # Shows the pane and the app itself.
        self.hpane.show()
        self.app.show()
    
      # This is our own fuction that we can use to create
      # a menu for our application.
      def create_menu(self, appview):
        # Gets this views menu so we can add stuff
        # to it.
        main_menu = hildon.AppView.get_menu(appview)
    
        # This menu will be used as a submenu.
        menu_help = gtk.Menu()
    
        # Creates items for the menu.
        item_help = gtk.MenuItem('Help')
        item_about = gtk.MenuItem('About')
        item_separator = gtk.SeparatorMenuItem()
        item_close = gtk.MenuItem('Close')
    
        # Connects signals "activate" to the menu items
        # so that something actually happens when they
        # are activated.
        item_about.connect("activate", self.about_callback)
        item_close.connect("activate", self.destroy)
    
        # Adds the items to the menus.
        gtk.Menu.append(main_menu, item_help)
        gtk.Menu.append(menu_help, item_about)
        gtk.Menu.append(main_menu, item_separator)
        gtk.Menu.append(main_menu, item_close)
    
        # Adds the submenu to the main_menu.
        gtk.MenuItem.set_submenu(item_help, menu_help)
    
        # Shows all menu widgets.
        gtk.Widget.show_all(main_menu)
    
      # Creates a TextView that can be used for typing
      # or editing text in it.
      def create_textview(self):
        # Create new TextView
        textview = gtk.TextView()
    
        # Set some properties for it.
        textview.set_editable(True)
        textview.set_cursor_visible(True)
        textview.set_wrap_mode(gtk.WRAP_WORD)
        textview.set_justification(gtk.JUSTIFY_CENTER)
    
        # Get the textbuffer that contains the text
        # in the textview.
        textbuffer = textview.get_buffer()
    
        # Set the text that shows inside the textview.
        textbuffer.set_text('You can write here someting...')
    
        # Show the textview.
        textview.show()
    
        # Return the created textview.
        return textview
    
      # Creates a ButtonBox widget that can be used to easily
      # lay out buttons.
      def create_buttons(self):
        # Creates a vertical ButtonBox where buttons are
        # laid on top of each other.
        button_box = gtk.VButtonBox()
    
        # Set some properties for the ButtonBox
        button_box.set_layout(gtk.BUTTONBOX_SPREAD)
        button_box.set_border_width(10)
        button_box.set_spacing(20)
    
        # Create two buttons that will be added to the
        # ButtonBox.
        about_button = gtk.Button('About')
        close_button = gtk.Button(None, gtk.STOCK_QUIT)
    
        # Connect signals to the buttons so that something
        # actually happens when they are pressed.
        about_button.connect("clicked", self.about_callback)
        close_button.connect("clicked", self.destroy)
    
        # Add both buttons to the ButtonBox.
        button_box.add(about_button)
        button_box.add(close_button)
    
        # Show buttons and the ButtonBox.
        gtk.Widget.show_all(button_box)
    
        # Return created ButtonBox.
        return button_box
    
      # This is a callback function that is called when about
      # menuitem or button are clicked. It shows information
      # about the application using AboutDialog.
      def about_callback(self, widget, data=None):
        # Create the about dialog.
        dialog = gtk.AboutDialog()
    
        # Set the properties for it.
        dialog.set_name('UITest')
        dialog.set_version('0.0.1')
        dialog.set_comments('Describe your app here...')
        dialog.set_website('http://www.teemuharju.net')
    
        # Show the dialog.
        dialog.show()
    
      # This is also a callback function. This is called
      # when you want to close the application.
      def destroy(self, widget, data=None):
        gtk.main_quit()
    
      # This is the main function that will start the
      # application. Note that the __init__ function
      # is always called before this one.
      def main(self):
        gtk.main()
    
    # This creates the UITest class, ie. runs the __init__
    # function and then calls the main function to start
    # the application.
    if __name__ == "__main__":
      uitest = UITest()
      uitest.main()
    
