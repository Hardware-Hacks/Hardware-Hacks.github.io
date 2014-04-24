---
author: munkee
comments: true
date: 2013-04-16 11:40:38+00:00
layout: post
slug: webmin-setup
title: Webmin Setup On Raspberry Pi
wordpress_id: 44
categories:
- Raspberry PI
- Tutorials
tags:
- lighttpd
- PHP
- Raspberry PI
- Server
- webmin
---

Todays tutorial is aimed at those of us who want to use the webmin panel on our raspberry pi. Webmin essentially gives you a great overview of all the functions you would normally need to carry out using SSH on your headless setup. Theres a raft of features ranging from memory and process usage stats right through to rebooting and shutting down your pi all from the web.

It is a great application and I highly recommend it for those that aren't so keen on using a terminal window all of the time.

In order to get this bad boy set up carry out the following steps.

First get the file.

    
    pi@raspberrypi / $ sudo wget http://prdownloads.sourceforge.net/webadmin/webmin-1.580.tar.gz


The file is a gz so we can use the gunzip command to get to the tar.

    
    pi@raspberrypi / $ sudo gunzip webmin-1.580.tar.gz


Now we have our tar we need to open it up.

    
    pi@raspberrypi / $ sudo tar xf webmin-1.580.tar


Some of the above commands may take a while as it does the unpacking but do not fear all is ok.

I always like to try and keep my web apps in one place so I will create a folder in my /var/www to hold webmin.

    
    pi@raspberrypi / $ sudo mkdir /var/www/webmin


Now we can run the simple setup script.

    
    pi@raspberrypi / $ cd webmin-1.580



    
    pi@raspberrypi /webmin-1.580 $ sudo sh setup.sh /var/www/webmin


You will be presented with the following.

    
    ***********************************************************************
    *            Welcome to the Webmin setup script, version 1.580        *
    ***********************************************************************
    Webmin is a web-based interface that allows Unix-like operating
    systems and common Unix services to be easily administered.
    
    Installing Webmin from /webmin-1.580 to /var/www/webmin ...
    
    ***********************************************************************
    Webmin uses separate directories for configuration files and log files.
    Unless you want to run multiple versions of Webmin at the same time
    you can just accept the defaults.
    
    Config file directory [/etc/webmin]:
    Log file directory [/var/webmin]:
    
    ***********************************************************************
    Webmin is written entirely in Perl. Please enter the full path to the
    Perl 5 interpreter on your system.
    
    Full path to perl (default /usr/bin/perl):
    
    Testing Perl ...
    Perl seems to be installed ok
    
    ***********************************************************************
    cat: /etc/lsb-release: No such file or directory
    cat: /etc/lsb-release: No such file or directory
    cat: /etc/lsb-release: No such file or directory
    cat: /etc/lsb-release: No such file or directory
    cat: /etc/lsb-release: No such file or directory
    cat: /etc/lsb-release: No such file or directory
    cat: /etc/lsb-release: No such file or directory
    Operating system name:    Debian Linux
    Operating system version: 7.0
    
    ***********************************************************************
    Webmin uses its own password protected web server to provide access
    to the administration programs. The setup script needs to know :
     - What port to run the web server on. There must not be another
       web server already using this port.
     - The login name required to access the web server.
     - The password required to access the web server.
     - If the webserver should use SSL (if your system supports it).
     - Whether to start webmin at boot time.
    
    Web server port (default 10000):
    Login name (default admin):
    Login password:
    Password again:
    Use SSL (y/n): n
    Start Webmin at boot time (y/n): y
    ***********************************************************************
    Copying files to /var/www/webmin ..
    ..done
    
    Creating web server config files..
    ..done
    
    Creating access control file..
    ..done
    
    Inserting path to perl into scripts..
    ..done
    
    Creating start and stop scripts..
    ..done
    
    Copying config files..
    ..done
    
    Configuring Webmin to start at boot time..
    Created init script /etc/init.d/webmin
    ..done
    
    Creating uninstall script /etc/webmin/uninstall.sh ..
    ..done
    
    Changing ownership and permissions ..
    ..done
    
    Running postinstall scripts ..
    PID 3347 from file /var/webmin/miniserv.pid is not valid
    
    ..done
    
    Enabling background status collection ..
    PID 3347 from file /var/webmin/miniserv.pid is not valid
    ..done
    
    Attempting to start Webmin mini web server..
    Starting Webmin server in /var/www/webmin
    Pre-loaded WebminCore
    ..done
    
    ***********************************************************************
    Webmin has been installed and started successfully. Use your web
    browser to go to
    
      http://raspberrypi:10000/
    
    and login with the name and password you entered previously.


Thats it! Just simply follow any instructions if prompted and your app will nicely set itself up. I have to say it is excellent and requires hardly any effort to get it all working considering the amount of functionality it has.

You can access the webmin page using:

    
    http://host:10000/


Replacing host with either your domain or ip address.

You will be presented with the following type of page.

[![webmin](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/c_crop,h_491,w_491,x_207,y_0/h_150,w_150/v1390936800/webmin_bdpesh.png)](http://192.168.0.15/wordpress/wp-content/uploads/2013/04/webmin.png)

From here explore the webmin app using the drop down navigation on the left hand side. It will automatically pick up all of your installed packages and setup. It even has a nice area to control mysql without needing to install phpmyadmin. The only thing I will point out is there is nothing to directly support lighttpd. Webmin will install perfectly on a lighttpd server but it will not list it under its server options. I have a dead install of apache in mine.

Hope this offers some use, well worth getting!
