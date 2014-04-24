---
author: munkee
comments: true
date: 2014-01-14 20:45:20+00:00
layout: post
slug: raspberry-pi-ad-blocking-proxy-installation-using-privoxy
title: Raspberry Pi Ad blocking proxy installation using Privoxy
wordpress_id: 425
categories:
- Raspberry PI
- Tutorials
tags:
- Performance
- proxy
- Raspberry PI
- Server
---

So I wanted to block adverts that always seemed to pop up whenever I visited a certain few websites (non porn related). I decided to look in to what is available for the Raspberry Pi and use it as a proxy server that would filter all of my traffic for me.

The benefits of using a proxy server ad blocker over an in browser ad blocker is that the server does all the work for you and not your client (browser). This ensures that ads are removed much earlier than during the rendering phase of your browser and therefore results in quicker page loads, well that's my theory anyway.

So what did I find out? [Well there is a very simple service called Privoxy!](http://privoxy.org) As always here is the marketing information from their website (which I have to say is one of the most basic sites I have ever visited):



> Privoxy is a non-caching web proxy with advanced filtering capabilities for enhancing privacy, modifying web page data and HTTP headers, controlling access, and removing ads and other obnoxious Internet junk. Privoxy has a flexible configuration and can be customized to suit individual needs and tastes. It has application for both stand-alone systems and multi-user networks.



Excellent it ticks all of the boxes I wanted. So now all that is left to do is walk you through how to install this on to your Raspberry Pi.

Firstly SSH in to your Pi and run an update and upgrade command to ensure all libraries are up to date.


    
    pi@raspberrypi ~ $ sudo apt-get update
    pi@raspberrypi ~ $ sudo apt-get upgrade



Then we can just grab the privoxy package.


    
    pi@raspberrypi ~ $ sudo apt-get install privoxy



You should then see the package install


    
    Need to get 779 kB of archives.
    After this operation, 2,492 kB of additional disk space will be used.
    Do you want to continue [Y/n]? y
    
    Setting up privoxy (3.0.19-2) ...
    



Once completed we need to make some minor changes to the privoxy configuration file. This is easy enough to do via nano.


    
    pi@raspberrypi ~ $ sudo nano /etc/privoxy/config



You need to find the line that talks about your "listen-address" as per below:

    
    
    #
    #      Suppose you are running Privoxy on an IPv6-capable machine and
    #      you want it to listen on the IPv6 address of the loopback device:
    #
    #        listen-address [::1]:8118
    #
    #listen-address  localhost:8118
    #
    #
    #  4.2. toggle
    



Change this listen-address to be the IP address of your Pi, the internal IP that is so something along the lines of 192.168.0.10 would be mine.

Your file should look like the below extract once un-commenting and updating the IP.

    
    
    #
    #      Suppose you are running Privoxy on an IPv6-capable machine and
    #      you want it to listen on the IPv6 address of the loopback device:
    #
    #        listen-address [::1]:8118
    #
    listen-address  192.168.0.10:8118
    #
    #
    #  4.2. toggle
    



I would love to explain what else is held in the configuration file but quite honestly it is very self explanatory and also VERY long.

Next Hit Ctrl X and then Y to exit and save changes to the file.

You will then want to restart the privoxy service:


    
    pi@raspberrypi ~ $ sudo service privoxy restart



Wonderful.. we are done for initial set up. We now need to configure our laptops, tablets, phones and whatever else we want to use the advanced filtering proxy to point towards our Raspberry Pi.

To add the proxy info:
In Google Chrome: Go to Settings > Show advanced settings... > Change proxy settings... 
In Firefox: Go to Preferences > Advanced tab > Network tab > Settings button
In Internet Explorer: Go to Settings > Internet Options > Connections > LAN Settings > Tick use proxy server

Then enter the proxy server IP and Port which in our case will be the Pi IP address and Port 8118.

After that restart your browser and try and access the following page:

[http://config.privoxy.org/](http://config.privoxy.org/)

Hopefully you will see something along the lines of:



> This is Privoxy 3.0.19 on raspberrypi.local (192.168.0.10), port 8118, enabled



Thats it! I'll let you do the rest of the exploring.

It is worth noting that if you want to use this service when you are out and about, maybe using your cell phones mobile data service you can. All you need to do is open up the port 8118 on your router and instead of referencing your internal Pi IP address on your phones proxy server you just use your external router IP address. This will then pass any traffic from your phone through to the router and on to the Pi.
