---
author: munkee
comments: true
date: 2013-10-03 14:19:20+00:00
layout: post
slug: raspberry-pi-hosting-node-red-take-the-crap-out-of-developing-automation-the-internet-of-things-iot
title: Raspberry Pi Hosting Node-Red - Take the crap out of developing automation!
  - The Internet Of Things IOT
wordpress_id: 400
categories:
- Raspberry PI
- Tutorials
tags:
- Automation
- Code Club
- Database Connection
- Email
- IBM
- Java
- Node-Red
- Projects
- Raspberry PI
- Server
- Temperature
- Twitter
- Xively
---

This tutorial will document setting up Node-Red on your Raspberry Pi and getting some of your first automation tasks set up! Now I could harp on a good few hours about what Node Red is and how stupidly awesome it is however they say a picture tells a thousand words.. so below are a few thousand.

[![Node](http://nodered.org/images/node-red-screenshot.png)](http://nodered.org/images/node-red-screenshot.png)

So as you can see.. its graphical, its easy and its developed by cool guys at IBM..need I say more? Well probably so here is the official take on what Node Red actually is:





> A visual tool for wiring the internet of things



Yes we need more.. so read [here](http://nodered.org/).Now that the initial blurb has been given its time to get in to the tutorial.

Firstly SSH in to your Pi which will hopefully be running Raspbian or something similar. Then we need to get ourselves a working copy of Node.js. Node is a an event driven server side javascript environment. It is essentially the foundation that Node-Red will run on.

For the Pi it has been a pain in the past to get Node.js running but luckily there have been some tweaks made by the clever people out there that make our lives easier. So go ahead and run the following command in your terminal window:

As always get the new versions of everything...

    
    sudo apt-get update



    
    sudo apt-get upgrade


Then..

    
    wget http://node-arm.herokuapp.com/node_latest_armhf.deb


Then..

    
    sudo dpkg -i node_latest_armhf.deb


Thats it.. we now have node. You can check your version at anytime by running the following.

    
    node -v


If you get anything back other than a version number then something is borked. Leave a comment with the error and I'll take a look for you.

Now that we have node we can then jump in to downloading Node-Red. There is a tutorial on their git for the Pi which can be found [here](https://github.com/node-red/node-red/wiki/ReadMe-Raspberry-Pi---Advanced). But as always I want to take a different route as I want to use git to ensure that we always stay up to date with any new commits to the node-red repository.

We will now install Git. This allows us to clone etc. git hub repositories. If you have never heard of git hub it is definitely something you should [look into](https://github.com/). In essence though it is a good way of adding some version control and open source collaboration to your projects.

Anyway enough talking lets get it.

    
    sudo apt-get install git-core



Just accept any packages it wants to pull in. Then we want to dive in to our home folder and clone in the node-red repository (repo).

    
    git clone https://github.com/node-red/node-red.git



You will then have a folder cloned called "node-red" in your home/pi directory. We will now install everything using "npm" which is the Node Package Manager.

    
    cd node-red



    
    sudo npm install



After a while everything should run through and install for you. Once again if you get any errors let me know in the comments, it has been 50/50 sometimes for me to get this running right.

Next we will try and see if we can get Node-Red running, we still have some bits to do but its worth testing first. Make sure you are still in the /home/pi/node-red directory for this one:

    
    sudo node red.js



You will see a lot of information print out on to the screen, errors about missing packages and all of the good stuff. However the one line you are looking for is related to Node-Red being available on the default 1880 port.

So next you need to navigate to your Pi's address and port. If you are sat on your home network and you have SSH'd in to your Pi just go to:


    
    http://local-ip-address:1880


or If you are doing this all through your Pi running the GUI then:

    
    http://localhost:1880



You should be welcomed to a blank canvas with all of the "nodes" available on the left hand of the screen. You can drag and drop these to start wiring things together. A simple one to start off with would be to drag an Inject and a Debug node on to the screen and drag a line between the nodes to join them up.

[![images](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/h_78,w_300/v1390936605/images_wuufir.jpg)](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/v1390936605/images_wuufir.jpg)

You can then double click on the Inject node and enter a value of "Hello World!" in to the payload box. After this you will see the deploy button turn red at the top right of the screen. Click it to deploy the setup.

So nothing happened right? Good.. You now need to test your code and since we decided we wanted a regular Inject node to be setup you will need to now do two things.

Firstly select the debug tab on the right hand side of the screen.

[![images (1)](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/h_113,w_300/v1390936625/images-1_nnwkol.jpg)](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/v1390936625/images-1_nnwkol.jpg)

Then click the coloured box/tag at the end of the inject node (left hand side of the node).

You will then see your message pop-up in the debug window. Hurrah... So essentially that is how node-red works. You drag in some clever nodes and join them up to do cool things. We now need to rewind back to our setup, remember all of those error messages when you did the "sudo node red.js" they are happening because there are nodes that are trying to setup but they can't due to missing dependencies. There are loads of them.. ranging from connections to twitter to connections to your Raspberry Pi GPIO and Arduino!

I'll use the Node-Red docs example to show you how to add in these extra nodes for simplicity but if you need help with the others just leave a comment and I'll walk you through. Firstly lets jump back to our command line session with the Pi.

Hit CTRL+C to cancel the running of node-red. To ensure there are no other node processes running in the background you can also do the following command:

    
    sudo killall node



Be careful with it though if you have used node before as it will kill all of the node processes. Now our processes are truly ended you want to ensure you are still in "home/pi/node-red" and run the following command for us to get the twitter node running.


    
    sudo npm install ntwitter oauth



Once that has all run through you can restart node-red using:

    
    sudo node red.js


Then check your node-red site and you should see the twitter icons shown in the "social" tab. Go ahead and drag it on to your canvas and double click to setup if you want.

Now as I said there are many many many others available, all of those errors when node-red was starting are all extra nodes. Where it says things like "missing pushbullet" etc you can run:

    
    sudo npm install pushbullet


Edit: Ok I feel like being nice today.. this should get you most of the nodes:

    
    sudo npm install sentiment wordpos xml2js firmata fs.notify serialport feedparser pushbullet nodemailer imap irc simple-xmpp redis mongodb



There may be times where you need to add in some settings, this changes from node to node but hopefully as Node-Red is further developed then setting these up will become much easier. It is still early days but the work that has been done so far is EPIC!

The final things you might want to do is have Node-Red run in the background. For this you will need to install "screen".

    
    sudo apt-get install screen



Then to have node-red start after every reboot carry out the following steps:

    
    sudo nano /etc/rc.local



Anywhere before the exit line add in the following:

    
    cd /home/pi/node-red
    screen -dmS red node red.js



Press Ctrl+x and hit Y to save. We then need to change the executables of the file to make sure it runs as at present it is dormant.

    
    sudo chmod u+x /etc/rc.local



Once you reboot you will then have a new "screen" session created. You can check by doing:

    
    screen -ls



You will then see 1 item with "red" in the title. For more information on how to kill a screen session, how to attach and detach then go [here](http://www.rackaid.com/resources/linux-screen-tutorial-and-how-to/).

Some closing thoughts... Node-Red is awesome. It will help you move away from the mundane interfacing to services, hardware and software and spend more time linking up all of your sensors, internet services and all of that other good stuff with ease. There are nodes for the Raspberry Pi so you can control your GPIO Pins, there are also nodes for the arduino. There are examples out there creating remotes, controlling lights, RC Cars and pushing the weather to your phone + tons more..the possibilities already seem endless!

Now go hack!
