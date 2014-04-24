---
author: munkee
comments: true
date: 2013-10-06 20:09:12+00:00
layout: post
slug: mosquitto-the-open-source-mqtt-broker-that-can-be-installed-on-the-raspberry-pi
title: Mosquitto - The open source MQTT broker that can be installed on the Raspberry
  Pi!
wordpress_id: 358
categories:
- Raspberry PI
tags:
- Arduino
- Automation
- Electronics
- Ethernet
- MQTT
- Node-Red
- Projects
- Raspberry PI
- Server
---

So why is an MQTT broker useful on the Raspberry Pi and why should I be installing it right now? Well lets kick off with our usual "Official" definition.



> MQTT is a machine-to-machine (M2M)/"Internet of Things" connectivity protocol. It was designed as an extremely lightweight publish/subscribe messaging transport. It is useful for connections with remote locations where a small code footprint is required and/or network bandwidth is at a premium. For example, it has been used in sensors communicating to a broker via satellite link, over occasional dial-up connections with healthcare providers, and in a range of home automation and small device scenarios. It is also ideal for mobile applications because of its small size, low power usage, minimised data packets, and efficient distribution of information to one or many receivers...Blah blah blah....



Great it seems to do a lot for us when it comes to linking together all of our devices to message and transport data. But how does this all work? Well in essence you have 3 things going on which are illustrated in the diagram below and then explained in a bit more detail.

[![download (2)](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/v1390936571/download-2_mobww0.jpg)](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/v1390936571/download-2_mobww0.jpg)

**Firstly there is publishing:**
Data is sent from the devices to "topics" these topics can be anything but are more than often loosley related to what the sensor or a group of sensors are doing/their location/their relation to the wider world. A good example of this could be within your home, you may have sensors measuring temperature, moisture and light levels all upstairs. You could have all of these sensors publishing data to a topic called "data/home/upstairs" with each sensor then having its own sub topic such as "/data/home/upstairs/temp", "/data/home/upstairs/light". As you can imagine you can create topics in any form you wish!

**Then there is subscribing:**
In order to read data from a sensor you simply need to subscribe to its topic. So if we wanted to read the light levels upstairs in the house that we talked about earlier we can just subscribe to the topic "/data/home/upstairs/light". If we wanted all of the sensors you can even drop this to "/data/home/upstairs". This is what makes the machine to machine messaging really useful it really seems to have a lot of similarities to the RESTful services that have become ever so popular.

**Then there is the broker:**
This is what we are talking about today, the broker is the central hub that manages all of the published topics to allow users to request and subscribe to these services. It can control security, publishing levels, quality of service and wills. You can Google about some of the latter statements I made to find out more as I could spend another few blog posts explaining these. The Raspberry Pi in our instance is an ideal candidate for a broker. You could have a number of sensors in your home as discussed before all interfacing to arduinos which are all networking back to your Pi! If your Pi is running Node-Red you could even publish your data from these sensors up to Xively or other brokers out there. You can even use it to subscribe to some of the big brokers that are publicly available on the net to pick up further data, such as weather reports and then analyse these reports against your internal temperature compared to external temperature. What I am trying to get at here is MQTT will really open your eyes to how you can handle data between sensors and also the other sensor networks out there giving you mega data messaging!

Now for the fun part...

Lets finally get on with the reason you are here. Installing the open source broker Mosquitto to your Raspberry Pi!. Firstly you want to SSH in to your Pi and get to the command line. From here run the following:


    
    wget http://repo.mosquitto.org/debian/mosquitto-repo.gpg.key



Then we will take this repository key and add it to our Pi

    
    sudo apt-key add mosquitto-repo.gpg.key



From here switch directories to our apt sources list folder:

    
    cd /etc/apt/sources.list.d/



Then download the source list:

    
    sudo wget http://repo.mosquitto.org/debian/mosquitto-repo.list



Update our sources:

    
    sudo apt-get update



Finally the silver bullet:

    
    apt-get install mosquitto



You now have your raspberry Pi all set up as a broker using mosquitto!
