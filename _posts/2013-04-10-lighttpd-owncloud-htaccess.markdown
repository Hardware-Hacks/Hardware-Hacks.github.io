---
author: munkee
comments: true
date: 2013-04-10 10:44:27+00:00
layout: post
slug: lighttpd-owncloud-htaccess
title: Raspberry Pi Lighttpd Owncloud .htaccess Alternative
wordpress_id: 12
categories:
- Fixes
- Raspberry PI
tags:
- lighttpd
- owncloud
- Raspberry PI
---

Trouble with owncloud stating that your .htaccess file is not working correctly when using lighttpd server?

Another simple fix!

Locate your lighttpd config file for editing:

    
    
    pi@raspberrypi ~ $ sudo nano /etc/lighttpd/lighttpd.conf
    


Add the following line to your config anywhere inside the file:

    
    
    $HTTP["url"] =~ "^/data/" { url.access-deny = ( "" ) }
    


Save the file with CTRL+X, Y, Enter
Restart Lighttpd

    
    
    pi@raspberrypi ~ $ sudo /etc/init.d/lighttpd restart
    



Your warning message within owncloud admin should now have disappeared. Anyone who now tries to access http://.../owncloud/data will be served the trusty 403!
