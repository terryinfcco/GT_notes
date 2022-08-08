---
layout: posts
title: "BroCode Tkinter Video 1 Windows!"
date: 2022-08-07 13:20:54 -0700
categories: tkinter geometry title iconphoto background_color mainloop
tags: brocode tkinter windows
---

# Bro Code Youtube Channel Tkinter

## Video 1 - Windows

``` python
from tkinter import *

window = Tk() # instantiate an instance of a tkinter window
# Change the size of the window
window.geometry("420x420")

# Change the title of the window
window.title("Bro Code First GUI Program")

# Give it a favicon - I tried both True and False in the iconphoto stmt and 
# couldn't tell any difference.

icon = PhotoImage(file="linux.png")
window.iconphoto(True, icon)

# change background color of window
window.config(background="#5cfcff")

window.mainloop() # place window on computer screen and listen for events.
```

