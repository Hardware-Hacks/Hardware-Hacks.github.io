---
author: munkee
comments: true
date: 2013-10-16 10:43:25+00:00
layout: post
slug: raspberry-pi-mosquitto-service-failwont-start
title: Raspberry Pi Mosquitto Service Fail/Won't Start
wordpress_id: 381
categories:
- Fixes
- Raspberry PI
tags:
- daemon
- init.d
- MQTT
- Raspberry PI
- Server
---

My Xively feeds stopped working recently which I push to via MQTT and more specifically Mosquitto. After checking the service through the console I kept seeing the following error:


    
    pi@raspberrypi ~/node-red $ sudo service mosquitto start
    [ ok ] Starting network daemon:: mosquitto.
    pi@raspberrypi ~/node-red $ sudo service mosquitto status
    [FAIL] mosquitto is not running ... failed!
    



So the service is starting and then failing straight away. After a bit of hunting around I noticed there was a mention of the mosquitto.db file causing issues being partially locked/corrupted/non-writeable.

Well the best way I thought was to just remove the file to see what would happen:


    
    pi@raspberrypi ~/node-red $ cd /var/lib/mosquitto/
    pi@raspberrypi /var/lib/mosquitto $ ls
    mosquitto.db
    pi@raspberrypi /var/lib/mosquitto $ nano mosquitto.db
    pi@raspberrypi /var/lib/mosquitto $ sudo rm mosquitto.db
    pi@raspberrypi /var/lib/mosquitto $ sudo service mosquitto start
    [ ok ] Starting network daemon:: mosquitto.
    pi@raspberrypi /var/lib/mosquitto $ sudo service mosquitto status
    [ ok ] mosquitto is running.
    



As you can see all up and running. Strange one but thought I would post it up just in case someone else out there has been hitting the same issues.
