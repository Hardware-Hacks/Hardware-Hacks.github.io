---
author: munkee
comments: true
date: 2014-01-12 11:57:17+00:00
layout: post
slug: raspberry-pi-node-red-startstoprestart-script
title: Raspberry Pi Node-Red Start/Stop/Restart Script
wordpress_id: 446
categories:
- Fixes
- Raspberry PI
tags:
- Automation
- cron
- daemon
- init.d
- Node-Red
- Node.js
- Projects
- Raspberry PI
---

A very quick script to allow [Node-Red](http://c-mobberley.com/wordpress/index.php/2013/10/03/raspberry-pi-hosting-node-red-take-the-crap-out-of-developing-automation-the-internet-of-things-iot/) to start at boot and stop when shutdown. You can also use the standard service commands to start/stop/restart if needed e.g. sudo service node_red restart

**UPDATE: This now contains the following argument:  --max-old-space-size=128

This ensures that the garbage collector runs in line with the memory available on the Raspberry Pi. The suspected leak in Node.js for version 0.10 was actually due to the garbage collector being set too high.**

Firstly create a new init.d file:


    
    sudo nano /etc/init.d/node_red



Then copy and paste the following code in to the file The only thing you will need to change is the directory where node-red is installed under on your Pi. For me this is ~/node-red/, for you it may be different so update the following line "cd /home/pi/node-red" to reflect the location.


    
    #! /bin/sh
    # Starts and stops node-red
    # /etc/init.d/node_red
    ### BEGIN INIT INFO
    # Provides:	node_red
    # Required-Start:	$syslog
    # Required-Stop:	$syslog
    # Default-Start:	2 3 4 5
    # Default-Stop:		0 1 6
    # Short-Description:	Node Red
    ### END INIT INFO
    
    #Load up node red when called
    case "$1" in
    
    start)
    	echo "Starting Node-Red.."
    	cd /home/pi/node-red
    	sudo screen -dmS red node --max-old-space-size=128 red.js
    ;;
    
    stop)
    	echo "Stopping Node-Red.."
    	sudo screen -S red -X quit
    ;;
    
    restart)
    	echo "Restarting Node-Red.."
    	$0 stop
    	$0 start
    ;;
    *)
    	echo "Usage: $0 {start|stop|restart}"
    	exit 1
    esac
    



Once you have pasted and updated the above script hit CTRL+X and then Y to save changes. Next we will make the file executable so that it can be run.


    
    sudo chmod +x /etc/init.d/node_red



Finally to ensure the script will start at boot and stop at shutdown we need to update the rc.d file.


    
    sudo update-rc.d node_red defaults



Thats it! Nice and simple, as stated at the start the following commands will now work:


    
    sudo service node_red start|stop|restart



You can easily use this script to work with other services too =]
