---
author: munkee
comments: true
date: 2013-04-30 19:02:53+00:00
layout: post
slug: raspberry-pi-best-fastest-web-browser-for-gui-usage
title: Raspberry Pi Best / Fastest Web Browser For GUI Usage
wordpress_id: 194
categories:
- Raspberry PI
- Tutorials
tags:
- GUI
- Raspberry PI
- Web Browsing
---

For some of us (including myself who now has 2 pi's) there may come a time when we want to boot in to the raspberry pi's GUI and even use the browser to surf the net. For anyone who has undertaken this ordeal you will know how much of a fight the pi has with browsing. The pages lag, the browser is slow to start up, javascript / flash / html5 and all of the fancy pants accessories the web now has in 2013 just destroy the pi's performance. As a web server or hardware control the raspberry pi shines for its money but web browsing is near enough pointless. However.. there is a fix.. even if you have to lose a few of the fancy frills of the web. So with that being said here it is "Minimal-Web-Browser".

This little app can be downloaded from google code repository and it essentially strips out a lot of the fluff you get on the web but also within browsers itself. It has a home button a back button and a refresh button. You can enable and disable scripting from the browser bar also but other than that everything else is missing. Forget your bookmarks, forget your instant searches it is back to basics. The browsing speed, loading speed and overall experience however is well worth the compromise.

Ill now show you everything you need to in order to install the program.

Firstly ensure you have git installed.

    
    
    sudo apt-get install git
    



We will now clone the git repo for the app

    
    
    git clone https://code.google.com/p/minimal-web-browser/
    



This will be installed under your home root folder, usually /home/pi so we will now locate to the folder to run the install script.

    
    
    cd ~/minimal-web-browser/web-1.0
    



Now the make install is called

    
    
    sudo make install
    


Edit: Thanks for the pointer from Kirl


> You may need to run "sudo apt-get install libwebkit-dev" prior to "sudo make install"



You will now have your minimal browser stored within /home/pi/minimal-web-browser/web-1.0
To run the browser just double click the Minimal Web Browser icon and it will load up.

Perfect!
