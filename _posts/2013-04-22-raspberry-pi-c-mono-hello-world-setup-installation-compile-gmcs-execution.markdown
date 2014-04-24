---
author: munkee
comments: true
date: 2013-04-22 06:34:48+00:00
layout: post
slug: raspberry-pi-c-mono-hello-world-setup-installation-compile-gmcs-execution
title: Raspberry Pi C# Mono Hello World Setup / Installation / Compile (gmcs) / Execution
wordpress_id: 79
categories:
- Raspberry PI
- Tutorials
tags:
- C#
- Mono
- Raspberry PI
---

The raspberry pi is a brilliant bit of kit that is making hardware and software hacking and prototyping accessible by so many in the population. Whether you have a background in engineering, IT, media or theoretical physics the raspberry pi does not care and that is why it is becoming so popular. Anyone can take a raspberry pi and start working on their own projects or follow the many tutorials out there to make awesome stuff happen. It is well documented that the raspberry pi and Linux is quite python orientated but that does not mean you have to use it. Myself, I come from an engineering background but I have messed with Visual Basic, Html, Java, PHP etc etc... But quite recently I have fallen for asp.net and in particular c#.

I think it is the fluidity and also the wealth of resources available that have attracted me to c#. Whether I want to work on a simple console application or venture in to mobile web applications built on a MVC methodology I can do it all. For this very reason I wanted to continue my work with c# and took to researching how to get it up and running on my raspberry pi. So with all of the background out of the way.. Here is how to do it!

First off in order to get c# up and running you will need to install a package called mono. Without going into the details to much mono essentially is an open source version of asp.net and allows c# to run on your raspberry. It is a cross platform framework aimed at ensuring your projects can run anywhere, not just on windows machines but linux/android etc as well. Take a look at their site [here](http://www.mono-project.com/Main_Page).

So let's ssh in to the raspberry and grab mono.


    
    
    pi@raspberrypi ~ $ sudo apt-get install mono-runtime
    



And the output of that command?


    
    
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following extra packages will be installed:
      binfmt-support cli-common libmono-corlib4.0-cil libmono-i18n-west4.0-cil
      libmono-i18n4.0-cil libmono-security4.0-cil
      libmono-system-configuration4.0-cil libmono-system-security4.0-cil
      libmono-system-xml4.0-cil libmono-system4.0-cil mono-4.0-gac mono-gac
    Suggested packages:
      libmono-i18n4.0-all libgamin0
    The following NEW packages will be installed:
      binfmt-support cli-common libmono-corlib4.0-cil libmono-i18n-west4.0-cil
      libmono-i18n4.0-cil libmono-security4.0-cil
      libmono-system-configuration4.0-cil libmono-system-security4.0-cil
      libmono-system-xml4.0-cil libmono-system4.0-cil mono-4.0-gac mono-gac
      mono-runtime
    0 upgraded, 13 newly installed, 0 to remove and 0 not upgraded.
    Need to get 4,158 kB of archives.
    After this operation, 10.9 MB of additional disk space will be used.
    Do you want to continue [Y/n]? y
    Get:1 http://mirrordirector.raspbian.org/raspbian/ wheezy/main binfmt-support ar                                                                                                 mhf 2.0.12 [73.5 kB]
    Get:2 http://mirrordirector.raspbian.org/raspbian/ wheezy/main cli-common all 0.                                                                                                 8.2 [178 kB]
    Get:3 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-system-xm                                                                                                 l4.0-cil all 2.10.8.1-5 [447 kB]
    Get:4 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-system-se                                                                                                 curity4.0-cil all 2.10.8.1-5 [69.7 kB]
    Get:5 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-system-co                                                                                                 nfiguration4.0-cil all 2.10.8.1-5 [67.0 kB]
    Get:6 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-system4.0                                                                                                 -cil all 2.10.8.1-5 [671 kB]
    Get:7 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-security4.0-cil all 2.10.8.1-5 [134 kB]
    Get:8 http://mirrordirector.raspbian.org/raspbian/ wheezy/main mono-4.0-gac all 2.10.8.1-5 [28.0 kB]
    Get:9 http://mirrordirector.raspbian.org/raspbian/ wheezy/main mono-gac all 2.10.8.1-5 [21.9 kB]
    Get:10 http://mirrordirector.raspbian.org/raspbian/ wheezy/main mono-runtime armhf 2.10.8.1-5 [1,392 kB]
    Get:11 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-corlib4.0-cil all 2.10.8.1-5 [1,014 kB]
    Get:12 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-i18n4.0-cil all 2.10.8.1-5 [27.8 kB]
    Get:13 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-i18n-west4.0-cil all 2.10.8.1-5 [36.0 kB]
    Fetched 4,158 kB in 10s (379 kB/s)
    Selecting previously unselected package binfmt-support.
    (Reading database ... 67141 files and directories currently installed.)
    Unpacking binfmt-support (from .../binfmt-support_2.0.12_armhf.deb) ...
    Selecting previously unselected package cli-common.
    Unpacking cli-common (from .../cli-common_0.8.2_all.deb) ...
    Selecting previously unselected package libmono-system-xml4.0-cil.
    Unpacking libmono-system-xml4.0-cil (from .../libmono-system-xml4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-system-security4.0-cil.
    Unpacking libmono-system-security4.0-cil (from .../libmono-system-security4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-system-configuration4.0-cil.
    Unpacking libmono-system-configuration4.0-cil (from .../libmono-system-configuration4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-system4.0-cil.
    Unpacking libmono-system4.0-cil (from .../libmono-system4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-security4.0-cil.
    Unpacking libmono-security4.0-cil (from .../libmono-security4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package mono-4.0-gac.
    Unpacking mono-4.0-gac (from .../mono-4.0-gac_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package mono-gac.
    Unpacking mono-gac (from .../mono-gac_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package mono-runtime.
    Unpacking mono-runtime (from .../mono-runtime_2.10.8.1-5_armhf.deb) ...
    Selecting previously unselected package libmono-corlib4.0-cil.
    Unpacking libmono-corlib4.0-cil (from .../libmono-corlib4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-i18n4.0-cil.
    Unpacking libmono-i18n4.0-cil (from .../libmono-i18n4.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-i18n-west4.0-cil.
    Unpacking libmono-i18n-west4.0-cil (from .../libmono-i18n-west4.0-cil_2.10.8.1-5_all.deb) ...
    Processing triggers for man-db ...
    Processing triggers for desktop-file-utils ...
    Setting up binfmt-support (2.0.12) ...
    update-binfmts: warning: /usr/share/binfmts/cli: no executable /usr/bin/cli found, but continuing anyway as you request
    [ ok ] Enabling additional executable binary formats: binfmt-support.
    Setting up cli-common (0.8.2) ...
    Setting up libmono-corlib4.0-cil (2.10.8.1-5) ...
    Setting up libmono-system-xml4.0-cil (2.10.8.1-5) ...
    Setting up libmono-security4.0-cil (2.10.8.1-5) ...
    Setting up mono-4.0-gac (2.10.8.1-5) ...
    Setting up mono-gac (2.10.8.1-5) ...
    update-alternatives: using /usr/bin/gacutil to provide /usr/bin/cli-gacutil (global-assembly-cache-tool) in auto mode
    Setting up mono-runtime (2.10.8.1-5) ...
    update-alternatives: using /usr/bin/mono to provide /usr/bin/cli (cli) in auto mode
    Setting up libmono-i18n4.0-cil (2.10.8.1-5) ...
    Setting up libmono-i18n-west4.0-cil (2.10.8.1-5) ...
    Setting up libmono-system-configuration4.0-cil (2.10.8.1-5) ...
    Setting up libmono-system4.0-cil (2.10.8.1-5) ...
    Setting up libmono-system-security4.0-cil (2.10.8.1-5) ...
    



That's one hell of an unpack however everything has now been done for you. Very simple and exactly how it should be!

What we will now do is create a folder in our user directory to hold some of our project files. It is up to you where you do this however I like to keep everything organised and in one place.


    
    
    pi@raspberrypi ~ $ cd /
    pi@raspberrypi / $ sudo mkdir /home/pi/Projects
    



As always to test out that our c# language is up and running on the pi we will create a very simple hello world console application.

First the helloworld project folder can be created.


    
    
    pi@raspberrypi / $ sudo mkdir /home/pi/Projects/HelloWorld
    



We will then jump into our directory and create a helloworld.cs file.

    
    
    pi@raspberrypi / $ cd ~/home/pi/Projects/HelloWorld
    pi@raspberrypi ~/Projects/HelloWorld $ sudo nano HelloWorld.cs
    



Now you have nano opened up you will need to enter the following code to create the hello world output.


    
    
    using System;
    public class HelloWorld
    {
        public static void Main()
        {
    Console.WriteLine("Hello World!");
    Console.WriteLine("Testing this again!!!111");
        }
    }
    



Once that is pasted in you can then exit out and save the file (ctrl+x and a y for yes)

So we now have ourselves a HelloWorld.cs file however the last step is to make this an executable so the pi can run the file. To do this we will have to uyse the gmcs command. This is something I often see left out of most tutorials so it is definitely worth talking about now. gmcs is the compiler specifically targeting the 2.0 mscorlib. This is quite outdated but it seems to be the one available from apt-get and works fine for testing. If you would like to use the more upto date (not required though) mcs compiler you can find more information [here](http://www.mono-project.com/CSharp_Compiler).

Lets grab the compiler!


    
    
    pi@raspberrypi ~/Projects/HelloWorld $ sudo apt-get install mono-gmcs
    



Which outputs the following.


    
    
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following extra packages will be installed:
      libmono-corlib2.0-cil libmono-i18n-west2.0-cil libmono-posix2.0-cil libmono-security2.0-cil libmono-system2.0-cil
    Suggested packages:
      libmono-i18n2.0-cil libgamin0 libgdiplus libmono-winforms2.0-cil
    The following NEW packages will be installed:
      libmono-corlib2.0-cil libmono-i18n-west2.0-cil libmono-posix2.0-cil libmono-security2.0-cil libmono-system2.0-cil mono-gmcs
    0 upgraded, 6 newly installed, 0 to remove and 4 not upgraded.
    Need to get 3,191 kB of archives.
    After this operation, 8,750 kB of additional disk space will be used.
    Do you want to continue [Y/n]? y
    Get:1 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-corlib2.0-cil all 2.10.8.1-5 [925 kB]
    Get:2 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-i18n-west2.0-cil all 2.10.8.1-5 [46.1 kB]
    Get:3 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-security2.0-cil all 2.10.8.1-5 [134 kB]
    Get:4 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-system2.0-cil all 2.10.8.1-5 [1,565 kB]
    Get:5 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libmono-posix2.0-cil all 2.10.8.1-5 [92.1 kB]
    Get:6 http://mirrordirector.raspbian.org/raspbian/ wheezy/main mono-gmcs all 2.10.8.1-5 [428 kB]
    Fetched 3,191 kB in 7s (426 kB/s)
    Selecting previously unselected package libmono-corlib2.0-cil.
    (Reading database ... 67288 files and directories currently installed.)
    Unpacking libmono-corlib2.0-cil (from .../libmono-corlib2.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-i18n-west2.0-cil.
    Unpacking libmono-i18n-west2.0-cil (from .../libmono-i18n-west2.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-security2.0-cil.
    Unpacking libmono-security2.0-cil (from .../libmono-security2.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-system2.0-cil.
    Unpacking libmono-system2.0-cil (from .../libmono-system2.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package libmono-posix2.0-cil.
    Unpacking libmono-posix2.0-cil (from .../libmono-posix2.0-cil_2.10.8.1-5_all.deb) ...
    Selecting previously unselected package mono-gmcs.
    Unpacking mono-gmcs (from .../mono-gmcs_2.10.8.1-5_all.deb) ...
    Processing triggers for man-db ...
    Setting up libmono-corlib2.0-cil (2.10.8.1-5) ...
    Setting up libmono-i18n-west2.0-cil (2.10.8.1-5) ...
    Setting up libmono-posix2.0-cil (2.10.8.1-5) ...
    Setting up libmono-system2.0-cil (2.10.8.1-5) ...
    Setting up mono-gmcs (2.10.8.1-5) ...
    Setting up libmono-security2.0-cil (2.10.8.1-5) ...
    



Another very easily install!

Now compile our HelloWorld.cs example with the gmcs command.

    
    
    pi@raspberrypi ~/Projects/HelloWorld $ sudo gmcs HelloWorld.cs
    



This will produce us a nice .exe file which we can run with mono.


    
    
    pi@raspberrypi ~/Projects/HelloWorld $ mono HelloWorld.exe
    Hello World!
    Testing this again!!!111
    



There we have it from initial installation of mono through to our first compiled console application outputting a message. In my next tutorial I will focus on why this is useful, how can we interact with our databases such as mysql and also talk about the performance of c# mono vs python.
