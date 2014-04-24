---
author: munkee
comments: true
date: 2013-12-27 09:48:27+00:00
layout: post
slug: raspberry-pi-mono-3-2-installation-hard-float-compatible
title: Raspberry Pi Mono 3.2+ Installation Hard Float Compatible
wordpress_id: 418
categories:
- Raspberry PI
- Tutorials
tags:
- C#
- Mono
- MVC
- Projects
- Raspberry PI
- Server
---

Big news, Raspberry Pi Mono 3.2.7 is here following on [from an old post discussing](http://c-mobberley.com/wordpress/index.php/2013/06/16/raspberry-pi-mono-mvc-asp-net-xsp-fastcgi-lighttpd-implementation/) the issues that we had to work around with the lack of hard float support which have now been fixed. We now have an armf compatible mono 3.2.7 which will give us 4.0 and 4.2 .Net engine features.

I would also like to congratulate Alex Petersen who put in the bulk of the work to get us this epic update. 

Now its time to get into the installation of this new version, which is done from the point of view of a fresh install. Firstly ssh in to your Pi or open up a console window, then ensure you are fully up to date.


    
    pi@raspberrypi ~ $ sudo apt-get update



We will then grab a bunch of dependencies:


    
    pi@raspberrypi ~ $ sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev build-essential git-core automake libtool libglib2.0-dev libxrender-dev libfontconfig1-dev libpng12-dev libgif-dev libjpeg8-dev libtiff5-dev libexif-dev



Essentially we now have everything we need to download a repository from github, build it and install it. The next part if to grab the latest version of mono available to us from our apt source.


    
    pi@raspberrypi ~ $ sudo apt-get install mono-complete



The above command can take a while to fully install and you may see some "problem" errors along the way but just ignore and let the file do what it needs. Eventually you should be presented with your command prompt again ready to grab the latest github version of mono.


    
    pi@raspberrypi ~ $ sudo git clone git://github.com/mono/mono.git



This will clone the repository from the master branch which is exactly what we want. Other tutorials may mentioned a different branch but this is where Alex was previously working from until the pull request was accepted and the changes were merged into the master mono branch.

The clone can take a while to do due to the amount of deltas but my net connection has been pretty junk lately anyway so it might have just been for me:


    
    
    pi@raspberrypi ~ $ git clone git://github.com/mono/mono.git
    Cloning into 'mono'...
    remote: Reusing existing pack: 926099, done.
    remote: Counting objects: 15, done.
    remote: Compressing objects: 100% (15/15), done.
    remote: Total 926114 (delta 3), reused 12 (delta 0)
    Receiving objects: 100% (926114/926114), 318.63 MiB | 1.28 MiB/s, done.
    Resolving deltas: 100% (752724/752724), done.
    Checking out files: 100% (45467/45467), done.
    pi@raspberrypi ~ $
    



Once the clone is complete we then move in to the mono directory.


    
    pi@raspberrypi ~ $ cd mono
    pi@raspberrypi ~/mono $
    



There is an auto generating script which has to be run to ensure we configure the make correctly.
(you may need to –prefix and –enable-nls instead of.prefix and .enable-nls)


    
    pi@raspberrypi ~/mono $ sudo ./autogen.sh .prefix=/usr/local .enable-nls=no
    Running libtoolize...
    libtoolize: putting auxiliary files in `.'.
    libtoolize: copying file `./ltmain.sh'
    libtoolize: putting macros in AC_CONFIG_MACRO_DIR, `m4'.
    libtoolize: copying file `m4/libtool.m4'
    libtoolize: copying file `m4/ltoptions.m4'
    libtoolize: copying file `m4/ltsugar.m4'
    libtoolize: copying file `m4/ltversion.m4'
    libtoolize: copying file `m4/lt~obsolete.m4'
    Running aclocal -I m4 -I .  ...
    etc.
    
    Cloning into 'external/rx'...
    remote: Counting objects: 3449, done.
    remote: Compressing objects: 100% (1807/1807), done.
    remote: Total 3449 (delta 1786), reused 3103 (delta 1443)
    Receiving objects: 100% (3449/3449), 18.01 MiB | 673 KiB/s, done.
    Resolving deltas: 100% (1786/1786), done.
    Submodule path 'external/rx': checked out '00c1aadf149334c694d2a5096983a84cf46221b8'
    Git submodules updated successfully
    
            mcs source:    mcs
    
       Engine:
            GC:            sgen and bundled Boehm GC with typed GC and parallel mark
            TLS:           __thread
            SIGALTSTACK:   yes
            Engine:        Building and using the JIT
            oprofile:      no
            BigArrays:     no
            DTrace:        no
            LLVM Back End: no (dynamically loaded: no)
    
       Libraries:
            .NET 2.0/3.5:  yes
            .NET 4.0:      yes
            .NET 4.5:      yes
            MonoDroid:     no
            MonoTouch:     no
            JNI support:   IKVM Native
            libgdiplus:    assumed to be installed
            zlib:          system zlib
    
    
    Now type `make' to compile
    



Once that has run we can then begin the "make" (Note: I actually ran the autogen.sh command twice as I had some weird errors come out the first time). For safety I like to run this within a screen session. Screen essentially allows us to run a command which is detached from our current session so if something goes wrong like a disconnection from the network the command will still run in the background.

If you have never used screen you will need to install it via apt-get.


    
    sudo apt-get install screen



Once that is complete you can carry out the following command.


    
    pi@raspberrypi ~/mono $ sudo screen -mS mono



You are then presented with a new screen session:


    
    root@raspberrypi:/home/pi/mono#



If you get disconnected from the session at any time you can simply type 
    
    sudo screen -r mono

. To exit the session at any time just hit ctrl+a then press d, the make will still run but it means you can continue to use your pi session for other things. To quit the session completely use ctrl+a then type :quit and hit enter, this will cancel the make command.

Anyway back to the installation. Once you are in the session simply run the following command.


    
    root@raspberrypi:/home/pi/mono# sudo make
    (CDPATH="${ZSH_VERSION+.}:" && cd . && /bin/bash /home/pi/mono/missing --run autoheader)
    



Once the command is running I just detached from the screen session using ctrl+a then d. I checked on the session every once in a while by typing sudo screen -r mono just to see if it was ok.

Now sit and wait for the command to complete.. it takes hours.

So after what feels like years of waiting you should be presented back with your command prompt to allow you to run the make command.


    
    root@raspberrypi:/home/pi/mono# sudo make install



Once again there is a big wait.. it took a few hours for my Pi to complete the command but eventually...:


    
    make[1]: Entering directory `/home/pi/mono'
    make[2]: Entering directory `/home/pi/mono'
    make[2]: Nothing to be done for `install-exec-am'.
    make[2]: Nothing to be done for `install-data-am'.
    make[2]: Leaving directory `/home/pi/mono'
    make[1]: Leaving directory `/home/pi/mono'
    root@raspberrypi:/home/pi/mono# mono --version
    Mono Runtime Engine version 3.2.7 (master/20d948f Thu Dec 26 18:23:16 UTC 2013)
    Copyright (C) 2002-2013 Novell, Inc, Xamarin Inc and Contributors. www.mono-project.com
            TLS:           __thread
            SIGSEGV:       normal
            Notifications: epoll
            Architecture:  armel,vfp+hard
            Disabled:      none
            Misc:          softdebug
            LLVM:          supported, not enabled.
            GC:            sgen
    



I was able to run the mono --version command and as you can see we now have version 3.2.7 running!

If you want to at this point you can also install XSP4 which is a standalone little server to run .Net web applications.


    
    root@raspberrypi:/home/pi/mono# sudo apt-get install mono-xsp4
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following extra packages will be installed:
      mono-xsp4-base
    The following NEW packages will be installed:
      mono-xsp4 mono-xsp4-base
    0 upgraded, 2 newly installed, 0 to remove and 3 not upgraded.
    Need to get 165 kB of archives.
    After this operation, 399 kB of additional disk space will be used.
    Do you want to continue [Y/n]? y
    



Once the package is installed you will be given the configuration information:


    
    Using Mono XSP 2 port: 8082
    Binding Mono XSP 2 address: 0.0.0.0
    Starting XSP 2.0 WebServer: mono-xsp2.
    Setting up asp.net-examples (2.10-2.4) ...
    



Thats it! Hopefully this has been helpful if you have any errors/notes please leave them in the comments section.
