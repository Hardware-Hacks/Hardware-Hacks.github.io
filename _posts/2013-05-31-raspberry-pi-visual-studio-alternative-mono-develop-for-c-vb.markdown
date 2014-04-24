---
author: munkee
comments: true
date: 2013-05-31 18:20:36+00:00
layout: post
slug: raspberry-pi-visual-studio-alternative-mono-develop-for-c-vb
title: Raspberry Pi Visual Studio Alternative Mono Develop for C# & VB
wordpress_id: 288
categories:
- Raspberry PI
- Tutorials
tags:
- C#
- Display Connection
- GUI
- Mono
- Projects
- Raspberry PI
- Server
---

For anyone crazy enough to want to do some direct development of c# applications on their Raspberry Pi you can use a program called Mono Develop.

This is ideal if you want to produce small apps on the pi that you also want to make sure are compatible. A few of you may have seen my battles with getting mono up and running to host some asp.net applications from this very site. It can be frustrating when you find out that visual studio has added in some none supported references that work fine on windows but not in linux and especially not through mono.

If you want to see the full ins and outs of mono develop please take a look at the following link [here](http://www.monodevelop.com).

For those of us with much less time lets get down to the installation!

Firstly run an update as always with the following command.

    
    sudo apt-get update



Make sure you install any new packages required. We will then move on to installing the mono runtime. This is a cut down version of the full blown mono application. You may already have this if you are attempting to host applications from your pi already but it's better safe than sorry so use the following command to download.


    
    sudo apt-get install mono-runtime



The mono runtime took quite a while to download for me so do not worry if you are sat around for a while. Now we move on to the mono develop editor installation and this couldn't be any easier. Just run the following.


    
    sudo apt-get install monodevelop



There will be a mass of dependencies that you will need to install I needed an extra 30 so once again be prepared to wait for this to completely install. It takes about 10 minutes maximum.

Now you have everything installed if you are running your pi headless you will want to get that old monitor out. To switch the pi from headless to GUI just simply type the following command from your console.


    
    startx



The pi will now load your Debian style desktop. You will then be able to find your mono develop program from the "start" menu under programming.

That's it.. I will provide some tutorials at a later point on developing applications using mono develop but firstly.. I need some practice too!
