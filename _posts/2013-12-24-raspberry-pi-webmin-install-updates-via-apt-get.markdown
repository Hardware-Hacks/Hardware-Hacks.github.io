---
author: munkee
comments: true
date: 2013-12-24 11:51:28+00:00
layout: post
slug: raspberry-pi-webmin-install-updates-via-apt-get
title: Raspberry Pi Webmin Install & Updates Via apt-get
wordpress_id: 414
categories:
- Raspberry PI
- Tutorials
tags:
- Automation
- Raspberry PI
- webmin
---

This post is an update to a method used previously to install webmin on to the Raspberry Pi. The [old post can still be found here](http://c-mobberley.com/wordpress/index.php/2013/04/16/webmin-setup/) if you run in to issues and is a fail safe method. However times move on and we can now change a few settings on the Pi to be able to install webmin and all of its dependencies using apt-get install webmin.

So enough about the blurb intro lets get on to what needs to be done.

Firstly get yourself SSH'd in to your Pi or open a command session from the desktop. We will then move on to editing our source list for APT. Carry out the following command


    
    pi@raspberrypi ~ $ sudo nano /etc/apt/sources.list



You will be presented with your source.list file. You need to add in two new lines below the current list of sources. Copy and paste in the following.


    
    deb http://download.webmin.com/download/repository sarge contrib
    deb http://webmin.mirror.somersettechsolutions.co.uk/repository sarge contrib
    



This will use the official webmin repository and its mirror to ensure we can keep webmin updated to the latest version.

We will also now be adding in key file to ensure we can access the repo. First ensure you are in your root directory.


    
    pi@raspberrypi ~ $ cd ~



As you can see I was already in there but you may not be so always worth running to be sure. We will now download the key file.


    
    pi@raspberrypi ~ $ wget http://www.webmin.com/jcameron-key.asc
    --2013-12-24 11:27:24--  http://www.webmin.com/jcameron-key.asc
    Resolving www.webmin.com (www.webmin.com)... 216.34.181.97
    Connecting to www.webmin.com (www.webmin.com)|216.34.181.97|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 1320 (1.3K) [text/plain]
    Saving to: `jcameron-key.asc'
    
    100%[======================================>] 1,320       --.-K/s   in 0s
    
    2013-12-24 11:27:25 (11.2 MB/s) - `jcameron-key.asc' saved [1320/1320]
    



Now we just need to add this to our apt key list.


    
    pi@raspberrypi ~ $ sudo apt-key add jcameron-key.asc
    OK
    



To ensure APT is up to date with all of the changes we have made you can now run the update command.


    
    pi@raspberrypi ~ $ sudo apt-get update
    Get:1 http://mirrordirector.raspbian.org wheezy Release.gpg [490 B]
    Get:2 http://archive.raspberrypi.org wheezy Release.gpg [490 B]
    Get:3 http://webmin.mirror.somersettechsolutions.co.uk sarge Release.gpg [189 B]
    Get:4 http://webmin.mirror.somersettechsolutions.co.uk sarge Release [9,542 B]
    Get:5 http://archive.raspberrypi.org wheezy Release [7,224 B]
    Get:6 http://mirrordirector.raspbian.org wheezy Release [14.4 kB]
    Get:7 http://archive.raspberrypi.org wheezy/main armhf Packages [15.9 kB]
    Get:8 http://mirrordirector.raspbian.org wheezy/main armhf Packages [7,414 kB]
    Get:9 http://webmin.mirror.somersettechsolutions.co.uk sarge/contrib armhf Packa  ges [1,218 B]
    Ign http://archive.raspberrypi.org wheezy/main Translation-en_GB
    Ign http://archive.raspberrypi.org wheezy/main Translation-en
    Ign http://webmin.mirror.somersettechsolutions.co.uk sarge/contrib Translation-e  n_GB
    Ign http://webmin.mirror.somersettechsolutions.co.uk sarge/contrib Translation-e  n
    Hit http://raspberrypi.collabora.com wheezy Release.gpg
    Hit http://raspberrypi.collabora.com wheezy Release
    Hit http://raspberrypi.collabora.com wheezy/rpi armhf Packages
    Ign http://raspberrypi.collabora.com wheezy/rpi Translation-en_GB
    Ign http://raspberrypi.collabora.com wheezy/rpi Translation-en
    Get:10 http://download.webmin.com sarge Release.gpg [189 B]
    Hit http://mirrordirector.raspbian.org wheezy/contrib armhf Packages
    Hit http://mirrordirector.raspbian.org wheezy/non-free armhf Packages
    Hit http://mirrordirector.raspbian.org wheezy/rpi armhf Packages
    Ign http://mirrordirector.raspbian.org wheezy/contrib Translation-en_GB
    Ign http://mirrordirector.raspbian.org wheezy/contrib Translation-en
    Ign http://mirrordirector.raspbian.org wheezy/main Translation-en_GB
    Ign http://mirrordirector.raspbian.org wheezy/main Translation-en
    Ign http://mirrordirector.raspbian.org wheezy/non-free Translation-en_GB
    Ign http://mirrordirector.raspbian.org wheezy/non-free Translation-en
    Ign http://mirrordirector.raspbian.org wheezy/rpi Translation-en_GB
    Ign http://mirrordirector.raspbian.org wheezy/rpi Translation-en
    Get:11 http://download.webmin.com sarge Release [9,542 B]
    Get:12 http://download.webmin.com sarge/contrib armhf Packages [1,218 B]
    Ign http://download.webmin.com sarge/contrib Translation-en_GB
    Ign http://download.webmin.com sarge/contrib Translation-en
    Fetched 7,475 kB in 47s (157 kB/s)
    Reading package lists... Done
    



Quite a list isn't it.

Now we can do the awesome and run our webmin install via apt-get .. finally haha!


    
    pi@raspberrypi ~ $ sudo apt-get install webmin
    Reading package lists... Done
    Building dependency tree
    Reading state information... Done
    The following extra packages will be installed:
      apt-show-versions libapt-pkg-perl libauthen-pam-perl libio-pty-perl
      libnet-ssleay-perl
    The following NEW packages will be installed:
      apt-show-versions libapt-pkg-perl libauthen-pam-perl libio-pty-perl
      libnet-ssleay-perl webmin
    0 upgraded, 6 newly installed, 0 to remove and 27 not upgraded.
    Need to get 22.1 MB of archives.
    After this operation, 139 MB of additional disk space will be used.
    Do you want to continue [Y/n]? y
    Get:1 http://download.webmin.com/download/repository/ sarge/contrib webmin all 1  .660 [21.6 MB]
    Get:2 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libnet-ssleay-per  l armhf 1.48-1 [317 kB]
    Get:3 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libauthen-pam-per  l armhf 0.16-2 [31.2 kB]
    Get:4 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libio-pty-perl ar  mhf 1:1.08-1 [39.3 kB]
    Get:5 http://mirrordirector.raspbian.org/raspbian/ wheezy/main libapt-pkg-perl a  rmhf 0.1.26+b1 [80.9 kB]
    Get:6 http://mirrordirector.raspbian.org/raspbian/ wheezy/main apt-show-versions   all 0.20 [34.9 kB]
    Fetched 22.1 MB in 21s (1,041 kB/s)
    Selecting previously unselected package libnet-ssleay-perl.
    (Reading database ... 72353 files and directories currently installed.)
    Unpacking libnet-ssleay-perl (from .../libnet-ssleay-perl_1.48-1_armhf.deb) ...
    Selecting previously unselected package libauthen-pam-perl.
    Unpacking libauthen-pam-perl (from .../libauthen-pam-perl_0.16-2_armhf.deb) ...
    Selecting previously unselected package libio-pty-perl.
    Unpacking libio-pty-perl (from .../libio-pty-perl_1%3a1.08-1_armhf.deb) ...
    Selecting previously unselected package libapt-pkg-perl.
    Unpacking libapt-pkg-perl (from .../libapt-pkg-perl_0.1.26+b1_armhf.deb) ...
    Selecting previously unselected package apt-show-versions.
    Unpacking apt-show-versions (from .../apt-show-versions_0.20_all.deb) ...
    Selecting previously unselected package webmin.
    Unpacking webmin (from .../archives/webmin_1.660_all.deb) ...
    Processing triggers for man-db ...
    Setting up libnet-ssleay-perl (1.48-1) ...
    Setting up libauthen-pam-perl (0.16-2) ...
    Setting up libio-pty-perl (1:1.08-1) ...
    Setting up libapt-pkg-perl (0.1.26+b1) ...
    Setting up apt-show-versions (0.20) ...
    ** initializing cache. This may take a while **
    Setting up webmin (1.660) ...
    Webmin install complete. You can now login to https://raspberrypi:10000/
    as root with your root password, or as any user who can use sudo
    to run commands as root.
    



As you can see from the above the apt-get install does EVERYTHING. Previously when using a tar file we would have had to made some changes and settings along the way. 

It is important to note when the install says "login as root" it will most likely mean for you to login with "pi" and your pi password. If you need to change ANY settings related to the webmin install just go to the webmin page and login and then select webmin --> webmin configuration.

Hope this helps everyone out if you have any issues let me know in the comments section!
