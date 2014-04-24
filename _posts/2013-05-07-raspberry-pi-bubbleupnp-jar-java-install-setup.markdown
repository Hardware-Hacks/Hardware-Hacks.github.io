---
author: munkee
comments: true
date: 2013-05-07 11:27:04+00:00
layout: post
slug: raspberry-pi-bubbleupnp-jar-java-install-setup
title: Raspberry Pi BubbleUPnP Jar / Java Install / Setup
wordpress_id: 225
categories:
- Raspberry PI
- Tutorials
tags:
- BubbleUPnP
- daemon
- init.d
- Java
- Raspberry PI
- Server
---

BubbleUpnp is a great front end for your miniDLNA client but the installation and setup on the raspberry pi is not well documented. Here is a quick tutorial showing how I have got the system up and running against my miniDLNA server using the Java/Jar file.

Firstly you need to check whether you have java already installed on your Pi or not. It is most likely not but no harm in checking.

    
    
    pi@raspberrypi ~ $ sudo java -version
    sudo: java: command not found
    



As expected no java. Luckily though this is easy to install and we get a choice of versions! To get the list run the following command.

    
    
    pi@raspberrypi ~ $ sudo apt-get install java-runtime
    


You will get an output similar to the below showing all of the version we able to install.

    
    
    Reading package lists... Done
    Building dependency tree        
    Reading state information... Done
    Package java-runtime is a virtual package provided by:
      openjdk-7-jre 7u7-2.3.2a-1+rpi1
      openjdk-6-jre 6b27-1.12.4-1+rpi1
      gcj-jre 4:4.7.2-1
      gcj-4.7-jre 4.7.2-3+rpi1
      gcj-4.6-jre 4.6.3-1+rpi1
      gcj-4.4-jre 4.4.7-1
      default-jre 1:1.6-47
    You should explicitly select one to install.
    



In this instance you can see there are quite a few options but I will go for openjdk version 6. I have tried version 7 and it had a few incompatibility issues and to be honest im not going to be using java for much other than running BubbleUPnP so there is no need to keep up to date.

To install run the following command.

    
    
    pi@raspberrypi ~ $ sudo apt-get install openjdk-6-jre
    



This will take some time there are is a lot of dependencies and extra packages that openjdk will pull in. Just hit y to anything it asks for, better safe than trying to debug an issue later on because you missed a dependency.

Now set up a folder where you would like to download and install BubbleUPnPServer. I have elected to go with my /var/www folder as it is where I keep all of my "online" resources. BubbleUPnP has a web interface so it made sense for me to install it there.


    
    
    pi@raspberrypi ~ $ sudo mkdir /var/www/bubbleupnp
    pi@raspberrypi ~ $ cd /var/www/bubbleupnp
    



We can now download the latest version of BubbleUPnP.

    
    
    pi@raspberrypi /var/www/bubbleupnp $ sudo wget http://www.bubblesoftapps.com/bubbleupnpserver/0.6.5/BubbleUPnPServer-0.6.5.zip
    



Since this is a zipped file we now need to unpack.

    
    
    pi@raspberrypi ~ $ sudo unzip BubbleUPnPServer-0.6.5.zip
    Archive:  BubbleUPnPServer-0.6.5.zip
       creating: ffpresets/
       creating: webapp/
       creating: webapp/BubbleUPnPServer/
       creating: webapp/BubbleUPnPServer/deferredjs/
       creating: webapp/BubbleUPnPServer/deferredjs/09A720DF56C2456FEC95B46E087C3692/
       creating: webapp/BubbleUPnPServer/deferredjs/42E62D821CCF46133B58E709821B4A12/
       creating: webapp/BubbleUPnPServer/deferredjs/4C4F8785768B33C948C4AD85573D02F0/
       creating: webapp/BubbleUPnPServer/deferredjs/886A45F995FCEA0AD1F012AE3EBB7594/
       creating: webapp/BubbleUPnPServer/deferredjs/B7D2102F38F8FFB91F616613C6FCF555/
       creating: webapp/BubbleUPnPServer/deferredjs/C1D1E957D000CD0E68C5A0D098AFD3E3/
       creating: webapp/BubbleUPnPServer/gwt/
       creating: webapp/BubbleUPnPServer/gwt/standard/
       creating: webapp/BubbleUPnPServer/gwt/standard/images/
       creating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/
       creating: webapp/doc/
      inflating: BubbleUPnPServer.jar    
      inflating: LICENCE.txt             
      inflating: bcprov-jdk16-146.jar    
      inflating: ffpresets/libx264-baseline.ffpreset  
      inflating: ffpresets/libx264-default.ffpreset  
      inflating: ffpresets/libx264-fast.ffpreset  
      inflating: ffpresets/libx264-fast_firstpass.ffpreset  
      inflating: ffpresets/libx264-faster.ffpreset  
      inflating: ffpresets/libx264-faster_firstpass.ffpreset  
      inflating: ffpresets/libx264-fastfirstpass.ffpreset  
      inflating: ffpresets/libx264-hq.ffpreset  
      inflating: ffpresets/libx264-lossless_fast.ffpreset  
      inflating: ffpresets/libx264-lossless_max.ffpreset  
      inflating: ffpresets/libx264-lossless_medium.ffpreset  
      inflating: ffpresets/libx264-lossless_slow.ffpreset  
      inflating: ffpresets/libx264-lossless_slower.ffpreset  
      inflating: ffpresets/libx264-lossless_ultrafast.ffpreset  
      inflating: ffpresets/libx264-main.ffpreset  
      inflating: ffpresets/libx264-max.ffpreset  
      inflating: ffpresets/libx264-medium.ffpreset  
      inflating: ffpresets/libx264-medium_firstpass.ffpreset  
      inflating: ffpresets/libx264-normal.ffpreset  
      inflating: ffpresets/libx264-placebo.ffpreset  
      inflating: ffpresets/libx264-placebo_firstpass.ffpreset  
      inflating: ffpresets/libx264-slow.ffpreset  
      inflating: ffpresets/libx264-slow_firstpass.ffpreset  
      inflating: ffpresets/libx264-slower.ffpreset  
      inflating: ffpresets/libx264-slower_firstpass.ffpreset  
      inflating: ffpresets/libx264-slowfirstpass.ffpreset  
      inflating: ffpresets/libx264-ultrafast.ffpreset  
      inflating: ffpresets/libx264-ultrafast_firstpass.ffpreset  
      inflating: ffpresets/libx264-veryfast.ffpreset  
      inflating: ffpresets/libx264-veryfast_firstpass.ffpreset  
      inflating: ffpresets/libx264-veryslow.ffpreset  
      inflating: ffpresets/libx264-veryslow_firstpass.ffpreset  
      inflating: launch.bat              
      inflating: launch.sh               
      inflating: libx264-current.ffpreset  
      inflating: webapp/BubbleUPnPServer.css  
      inflating: webapp/BubbleUPnPServer/09A720DF56C2456FEC95B46E087C3692.cache.html  
      inflating: webapp/BubbleUPnPServer/09A720DF56C2456FEC95B46E087C3692.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/0A9476898799A150D840F0B1C3672921.cache.png  
      inflating: webapp/BubbleUPnPServer/396F806CD63ABD414BFBB9D57429F05B.cache.png  
      inflating: webapp/BubbleUPnPServer/42E62D821CCF46133B58E709821B4A12.cache.html  
      inflating: webapp/BubbleUPnPServer/42E62D821CCF46133B58E709821B4A12.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/450AEEA3AED8A012CAAD0AC1F7AE782F.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/4C4F8785768B33C948C4AD85573D02F0.cache.html  
      inflating: webapp/BubbleUPnPServer/4C4F8785768B33C948C4AD85573D02F0.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/886A45F995FCEA0AD1F012AE3EBB7594.cache.html  
      inflating: webapp/BubbleUPnPServer/886A45F995FCEA0AD1F012AE3EBB7594.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/9760B036C3B6E12FF6DEEDC917855221.cache.png  
      inflating: webapp/BubbleUPnPServer/B7D2102F38F8FFB91F616613C6FCF555.cache.html  
      inflating: webapp/BubbleUPnPServer/B7D2102F38F8FFB91F616613C6FCF555.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/BubbleUPnPServer.nocache.js  
      inflating: webapp/BubbleUPnPServer/C1D1E957D000CD0E68C5A0D098AFD3E3.cache.html  
      inflating: webapp/BubbleUPnPServer/C1D1E957D000CD0E68C5A0D098AFD3E3.gwt.rpc  
      inflating: webapp/BubbleUPnPServer/CD15EC0BBF9CD57F9198FD5C1C37122E.cache.png  
      inflating: webapp/BubbleUPnPServer/DF7764EEC1903CD03C9545B354D8D8E4.cache.png  
      inflating: webapp/BubbleUPnPServer/E44767377485D18D6B6864F65BA8EF73.cache.png  
      inflating: webapp/BubbleUPnPServer/EDC7827FEEA59EE44AD790B1C6430C45.cache.png  
      inflating: webapp/BubbleUPnPServer/clear.cache.gif  
      inflating: webapp/BubbleUPnPServer/deferredjs/09A720DF56C2456FEC95B46E087C3692/1.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/09A720DF56C2456FEC95B46E087C3692/2.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/42E62D821CCF46133B58E709821B4A12/1.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/42E62D821CCF46133B58E709821B4A12/2.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/4C4F8785768B33C948C4AD85573D02F0/1.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/4C4F8785768B33C948C4AD85573D02F0/2.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/886A45F995FCEA0AD1F012AE3EBB7594/1.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/886A45F995FCEA0AD1F012AE3EBB7594/2.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/B7D2102F38F8FFB91F616613C6FCF555/1.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/B7D2102F38F8FFB91F616613C6FCF555/2.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/C1D1E957D000CD0E68C5A0D098AFD3E3/1.cache.js  
      inflating: webapp/BubbleUPnPServer/deferredjs/C1D1E957D000CD0E68C5A0D098AFD3E3/2.cache.js  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/corner.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/corner_ie6.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/hborder.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/hborder_ie6.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/corner_dialog_topleft.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/corner_dialog_topright.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/hborder_blue_shadow.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/hborder_gray_shadow.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/vborder_blue_shadow.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/ie6/vborder_gray_shadow.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/splitPanelThumb.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/vborder.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/images/vborder_ie6.png  
      inflating: webapp/BubbleUPnPServer/gwt/standard/standard.css  
      inflating: webapp/BubbleUPnPServer/gwt/standard/standard_rtl.css  
      inflating: webapp/BubbleUPnPServer/hosted.html  
      inflating: webapp/doc/BubbleUPnPServer_changelog.html  
      inflating: webapp/doc/bds_openhome.png  
      inflating: webapp/doc/foo_remote_browser.png  
      inflating: webapp/doc/foo_remote_connect.png  
      inflating: webapp/doc/foo_remote_video.png  
      inflating: webapp/doc/foobar2000_transcode_settings.png  
      inflating: webapp/doc/index.html   
      inflating: webapp/doc/kinsky_openhome.png  
      inflating: webapp/doc/library_remote_servers.png  
      inflating: webapp/doc/main.png     
      inflating: webapp/doc/media_renderers.png  
      inflating: webapp/doc/media_servers.png  
      inflating: webapp/doc/redirect_test.html  
      inflating: webapp/doc/remote_upnp_network.png  
      inflating: webapp/doc/remote_upnp_transcode.png  
      inflating: webapp/doc/security.png  
      inflating: webapp/doc/server_foobar2000_conf.png  
      inflating: webapp/doc/status.png   
      inflating: webapp/index.html       
    



You will see the unpacking on your screen, this is not just the JAR file that the bubbleupnp installation page talks about, it also unpacks a lot of dependency files too.

At this stage we are pretty much ready to go! There are a few ways you can start the server such as:

    
    
    java -XX:+PrintCommandLineFlags -jar BubbleUPnPServer.jar
    



However I want the full start/stop functionality and I do not want to clog up a command line with loads of data being printed out from the server. To get around this we can do two things, unpack the jar and run the launch file OR the much cleaner method of running an init.d script. I prefer this method due to not wanting to unpack jar files and have the clutter. Some may not find this to be a problem but lets just go with my way is the best way. Oh and it also means you can run it on boot up of your pi which is a big bonus! 

Instead of pasting the code in this post all over again I will [link you directly to the script](http://c-mobberley.com/wordpress/index.php/2013/04/15/bubbleupnp-background-startstoprestart-init-d-script/) in a blog post I made a few weeks ago. If you have any comments related to the setup of BubbleUPnP please leave them here. If you have any related to the init.d script and any issues with it running please post in the other posts comments :)

Once you have followed the init.d script tutorial you can come back to this post and run the following commands.

    
    
    pi@raspberrypi /var/www/bubbleupnp $ sudo /etc/init.d/BubbleUPnPServer start
    Starting BubbleUPnPServer...                      Ok
    pi@raspberrypi /var/www/bubbleupnp $ sudo /etc/init.d/BubbleUPnPServer status
    Checking BubbleUPnPServer...                      Running
    pi@raspberrypi /var/www/bubbleupnp $ 
    



If you see that BubbleUPnPServer is up and running you can then just open up a browser window and go to the server setup page. This should be located at: http://localhost:58050 if running from the Pi or http://Pi-Ip-Address:58050

Remember to open up port 58050 on your router so a connection can be made by the server. You DO NOT need to open up 58051 if you do not want to. You will see an error message on the setup page but ignore it, there is no dependency on this port unless you want ssl. It is also worth noting the server can be slow to startup, so give it 5 minutes to sort itself out. It can also take some time to reload some of the settings you have previously selected, once again.. wait!


