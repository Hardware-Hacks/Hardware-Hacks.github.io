---
author: munkee
comments: true
date: 2013-05-04 16:06:22+00:00
layout: post
slug: site-re-installation-and-my-raspberry-pi-web-server-setup
title: Site Re-installation and My Raspberry Pi Web Server Setup
wordpress_id: 216
categories:
- News
- Raspberry PI
tags:
- fast_cgi
- lighttpd
- php-apc
- Raspberry PI
- Wordpress
---

The site recently had a good 12 hours of downtime due to some messing up on my behalf. After installing apache to sort out a problem I was having with lighttpd I found that my time to first byte was in the region of 8 seconds up to 20 seconds. This meant that the site was running snail pace. After trying to diagnose the problem and messing around with PHP settings I managed to break everything.

5 hours of further work later I just had to resort to taking a back up of mysql and doing a fresh raspbian image on my sd card. I am now running purely the webserver on this Pi. I have a second Pi now that I will be running owncloud, BubbleUPnP and also using it for general web browsing through the UI. It is apparent that the Pi can run phenomenaly well when carrying out single tasks but as soon as you start messing around with different packages it can quickly get clogged up and the number of services/ram usage rockets. Apache is also over kill, it is much easier to work with for the large support community out there and compatibility but it is just too big and heavy for a Pi to run using a web server. For everyones info my time to first byte is now down to around 1.5 seconds with the site loading in sub 3 seconds on average. For a wordpress blog running on such a low end bit of kit I am more than happy for now.

To give a breakdown of my current install:

- Raspbian image
- 8GB SD card
- Lighttpd
- PHP APC (brilliant performance gains)
- Fast CGI
- Latest version of Wordpress
- CloudFlare

I am not using any plugins for wordpress aside from my syntax prettifier. I have tried installing WP Minify but I have found this has a big detriment to my time to first byte, probably because of the CPU having to work at compiling. With that being said if you are wishing to optimise please take a look at [this ](http://www.webpagetest.org/)site to see your before and after speeds. It is very useful to baseline how well you currently serve pages before making changes so you can see whether the change is worth keeping.

Here is the sites current performance: [linky](http://www.webpagetest.org/result/130504_20_F1H/)

Please comment on how you feel the site experience is from your location.
