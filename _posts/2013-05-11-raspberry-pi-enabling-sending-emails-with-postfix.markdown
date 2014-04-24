---
author: munkee
comments: true
date: 2013-05-11 13:17:27+00:00
layout: post
slug: raspberry-pi-enabling-sending-emails-with-postfix
title: Raspberry Pi Enabling / Sending Emails With Postfix
wordpress_id: 260
categories:
- Raspberry PI
- Tutorials
tags:
- Automation
- Email
- Raspberry PI
- Wordpress
---

Being able to send emails from your raspberry pi can be very useful especially if you are hosting your own blog or you want to use some code to be able to send you a status update of your pi's processes, temperature etc.

Luckily it is VERY simple to enable emailing from your Pi so today I will give a short run through of how exactly it is done. We will be using a package called postfix. The beauty of postfix is in its simplicity to configure. There are other packages out there but they often have limitations which do not apply with postfix. To get a good breakdown of every feature of the package take a look at the official websiite located [here](http://www.postfix.org/).

Lets now get down to business with the installation.

First we just need to grab the postfix package.

    
    root@raspberrypi:/home/pi# sudo apt-get install postfix
    



There will be quite a few dependencies that should come with the initial package so dont worry if you see lots of other things being installed at the same time.

After a few seconds you will be presented with the postfix installation page. At this point for most installs just select "OK".

You will then be presented with a number of options of how to pre-configure the installation. In this case we will assume we just want a bog standard installation that will allow the likes of wordpress to send out some emails and maybe your phpmyadmin to send out back up emails and the regular sort of things you would expect from a webserver. If the above applies to you select "Internet Site" and then hit "Ok".

You will then be presented with a page to select the system mail name. If you leave this as "raspberrypi" it is most likely you will be receiving emails with your public WAN Ip address after the @. This is fine but most mail sites will block emails sent with an Ip address in the email address or even just push them all to a spam folder.

For my setup I want to enter the FQDN (Fully Qualified Domain Name) which is c-mobberley.com. Go ahead and enter yours then hit "OK".

At this point we will get more of the unpacking of packages:


    
    Selecting previously unselected package ssl-cert.
    (Reading database ... 69922 files and directories currently installed.)
    Unpacking ssl-cert (from .../ssl-cert_1.0.32_all.deb) ...
    Selecting previously unselected package postfix.
    Unpacking postfix (from .../postfix_2.9.6-2_armhf.deb) ...
    Processing triggers for man-db ...
    



And a few moments later it is worth noting the following will show up on the console:


    
    Postfix is now set up with a default configuration.  If you need to make 
    changes, edit
    /etc/postfix/main.cf (and others) as needed.  To view Postfix configuration
    values, see postconf(1).
    
    After modifying main.cf, be sure to run '/etc/init.d/postfix reload'.
    



The installation continues for a few more lines whilst it works on sorting our the aliases that need to be set but I thought it would be useful to have the above configuration information shown on this post if you need to make any changes in the future and missed out on keeping a copy of where everything is installed to.

In my instance the last line I see before being returned back to the normal console start is:


    
    
    postfix: warning: inet_protocols: disabling IPv6 name/address support: Address family not supported by protocol
    . ok 
    



Ignore the IPv6 stuff, I do not have IPv6 installed on my main web server and it is not a requirement for postfix. The important part is the green "ok" that we see at the end of the install. You should now be set up and ready for emails to be sent out from your server.

I know this tutorial has been a bit wordy but to be honest other than just selecting the package and entering a couple of configuration options postfix is so easy to install there is not much code to show.

If you have any questions or comments please feel free to let me know.
