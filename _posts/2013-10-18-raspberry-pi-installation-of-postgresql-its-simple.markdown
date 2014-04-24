---
author: munkee
comments: true
date: 2013-10-18 20:55:35+00:00
layout: post
slug: raspberry-pi-installation-of-postgresql-its-simple
title: Raspberry Pi Installation of PostgreSQL - It's Simple!
wordpress_id: 392
categories:
- Raspberry PI
- Tutorials
tags:
- Database Connection
- fast_cgi
- GUI
- lighttpd
- PHP
- PostgreSQL
- Raspberry PI
---

This is a quick tutorial to detail the installation of [PostrgeSQL ](http://www.postgresql.org/)on to the Raspberry Pi. I have seen a number of forum posts over at RaspberryPi.org detailing having to download & compile then spending hours setting up when actually it is a very simple process and you are set up in a matter of minutes.

[caption width="200" align="aligncenter"]![](http://ua.pycon.org/static/talks/vasilyev/pycon2012_postgresql/images/postgresql_logo.png)Big elephant for dramatical effect... actually its the Postgresql logo![/caption]

I will not go in to the details of why you should pick one database over another, if you are interested in that then take a look at [google](https://www.google.co.uk/search?q=which+database+should+i+use%3F&oq=which+database+should+i+use%3F&aqs=chrome..69i57j0l5.6731j0j4&sourceid=chrome&espv=210&es_sm=122&ie=UTF-8). But I will explain the steps I use to install PostgreSQL on my Pi's.

Firstly get yourself SSH'd in to the Pi or open a command window from the Pi desktop. Next get the usual updates:


    
    sudo apt-get update



Then for the very simple part which people seem to be missing:


    
    pi@raspberrypi ~/node-red $ sudo apt-get install postgresql



You will be asked to download a number of packages, around about 20 mb so make sure you have that space available. Just say yes to the download and wait a couple of minutes. After some time you should then be hit with:


    
    Configuring postgresql.conf to use port 5432...
    update-alternatives: using /usr/share/postgresql/9.1/man/man1/postmaster.1.gz to provide /usr/share/man/man1/postmaster.1.gz (postmaster.1.gz) in auto mode
    [ ok ] Starting PostgreSQL 9.1 database server: main.
    Setting up postgresql (9.1+134wheezy4) ...
    



Perfect we have ourselves a PostreSQL server set up. I will now run through how to install PhpPgAdmin which is a web interface for PostgreSQL. It makes setup and administration much easier than the command line work that has to be done. I will assume from now on that you have a webserver set up on your Pi with Php support. I run Lighttpd and there are tutorials on how to set this up on the blog just click the Lighttpd tag on the right in the tag cloud.

Firstly we will grab the package from the package manager:


    
    sudo apt-get install php5-pgsql phppgadmin



Once this has run through we need to edit our Lighttpd configuration file:

    
    sudo nano /etc/lighttpd/lighttpd.conf



And add the following line to the setup:

    
    alias.url += ( "/phppgadmin" => "/usr/share/phppgadmin/")


This will ensure our http://webserver/phppgadmin points to the right internal directory. You need to also ensure that mod_alias, mod_fastcgi and mod_cgi are enabled or added to the top of the lighttpd.conf file e.g.:


    
    
    server.modules = (
    
                        "mod_rewrite",
                         "mod_redirect",
                         "mod_alias",
                         "mod_access",
                        # "mod_auth",
                        # "mod_status",
                         "mod_simple_vhost",
                        # "mod_evhost",
                        # "mod_userdir",
                        # "mod_secdownload",
                         "mod_fastcgi",
                         "mod_proxy",
                       # "mod_cgi",
                        # "mod_ssi",
                         "mod_compress",
                        # "mod_usertrack",
                         "mod_expire",
                        # "mod_rrdtool",
                        # "mod_accesslog"
    
    
    
    )
    



I have quite a long list already but as you can see they are all enabled. Once again to get fastcgi setup working with Php take a look at my lighttpd blog posts, especially regarding improving owncloud/wordpress performance as they detail all of the installation steps required to get to this point.

Now hit CTRL+X and then Y to save the lighttpd.conf file.

We then need to restart our server:

    
    sudo service lighttpd restart



We will now need to setup a user for the web interface as by default certain users are blocked from accessing the login page.

    
    sudo nano /usr/share/phppgadmin/conf/config.inc.php



Change the following line to false:

    
     $conf['extra_login_security'] = true; 


Then hit Ctrl+X then Y and enter to close the file saved.

Now we can go to our phpPgAdmin page:

    
    http://your-url/phppgadmin/



In the login page enter postgres with a blank password. This should then log you in.

We will now create a new user account which can be used to login to the phpPgAdmin interface securely. At the moment anyone can get in using the blank password with user postgres, this will not do. Steps to create something more secure:

- Click on "PostgreSQL" on the left hand side to load the server.
- Click "Roles" in the top middle area of the page.
- Click "Create Role"
- Create username/password and give all permissions other than "Inherits Privileges". Ignore the other options under the tick boxes.
- Click "Create" button
- Click "Logout" at the top right of the page

Now you are logged out try and login with the user you have just created. You should be successful and now its time to re-secure our server.


    
    sudo nano /usr/share/phppgadmin/conf/config.inc.php



Change the following line to true:

    
     $conf['extra_login_security'] = false; 


Then hit Ctrl+X then Y and enter to close the file saved. 

Thats it! You now have a secured phpgaadmin page and a nice interface to setup your PostgreSQL server. As you can see, installing PostgreSQL is a doddle with just a package download. The fun is in the setup of a nice interface but it definitely beats trying to run everything from the command line.

Any issues please leave a comment and I will try my best to get back to you!


