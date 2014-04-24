---
author: munkee
comments: true
date: 2013-10-20 16:28:02+00:00
layout: post
slug: raspberry-pi-installation-of-ghost-with-node-js-the-open-source-blogging-platform
title: Raspberry Pi Installation of Ghost with Node.js - The open source blogging
  platform
wordpress_id: 396
categories:
- Raspberry PI
- Tutorials
tags:
- Blog
- Ghost
- Node.js
- NPM
- Raspberry PI
---

During this tutorial I will discuss installing [Ghost - The open source blogging platform](https://en.ghost.org/) on to a Raspberry Pi. 

So what is Ghost? Well to save me going through the differences between this and Wordpress I will as usual just stoop for an easy quote from their official website: 



> Ghost is a platform dedicated to one thing: Publishing. It's beautifully designed, completely customisable and completely Open Source. Ghost allows you to write and publish your own blog, giving you the tools to make it easy and even fun to do. It's simple, elegant, and designed so that you can spend less time messing with making your blog work - and more time blogging.



So now you have an idea of what it is aiming at we will get in to the installation. Ghost runs on Node.js which I am becoming increasingly interested in. Node.js is very simple to install on the Raspberry Pi so lets get a copy of it installed.

Log yourself in to a command or ssh session and enter the following commands to install Node.js.


    
    sudo apt-get update
    sudo apt-get upgrade
    wget http://node-arm.herokuapp.com/node_latest_armhf.deb
    sudo dpkg -i node_latest_armhf.deb



Thats it! you can now carry out the following command to check your version of node and npm, the node package manager.

    
    node -v



    
    npm -v



Next we will move on to installing Ghost. Firstly create a folder to hold all of our Ghost files.

    
    sudo mkdir /var/www
    sudo mkdir /var/www/ghost
    cd /var/www/ghost



To get the source files you will need to sign up to the ghost website here..
https://en.ghost.org/signup/ and either navigate to the source files download page or just carry out the following command.


    
    sudo wget https://en.ghost.org/zip/ghost-0.3.3.zip --no-check-certificate



Once this has downloaded we will unzip the ghost file.


    
    sudo unzip ghost-0.3.3.zip



We will now ensure we are production ready for ghost and carry out the installation using the node package manager.

    
    sudo npm install --production



Ensure you are using sudo in the command above to make sure that there are no access denied errors. This command will take a lot of time to run through due to the amount of modules and the small fire power of the pi compared to a desktop however it does work and you should end up with a screen loaded with info as follows:


    
    
    etc etc etc...
    âââ range-parser@0.0.4
    âââ buffer-crc32@0.2.1
    âââ cookie@0.1.0
    âââ debug@0.7.2
    âââ mkdirp@0.3.5
    âââ commander@1.2.0 (keypress@0.1.0)
    âââ send@0.1.3 (mime@1.2.11)
    âââ connect@2.8.4 (uid2@0.0.2, pause@0.0.1, qs@0.6.5, bytes@0.2.0, formidable@1.0.14)
    
    express-hbs@0.2.2 node_modules/express-hbs
    âââ glob@3.1.21 (inherits@1.0.0, graceful-fs@1.2.3, minimatch@0.2.12)
    âââ handlebars@1.0.12 (optimist@0.3.7, uglify-js@2.3.6)
    
    mysql@2.0.0-alpha9 node_modules/mysql
    âââ require-all@0.0.3
    âââ bignumber.js@1.0.1
    
    sqlite3@2.1.16 node_modules/sqlite3
    âââ progress@1.0.1
    âââ mkdirp@0.3.5
    âââ tar.gz@0.1.1 (commander@1.1.1, fstream@0.1.24, tar@0.1.18)
    pi@raspberrypi /var/www/ghost $
    



Brilliant..now we will make a few minor edits to our ghost configuration file before starting it up in a development mode.


    
    sudo nano config.js



You want to navigate to the following lines within the "Development" area of the config file:

    
     server: {
                // Host to be passed to node's `net.Server#listen()`
                host: '127.0.0.1',
                // Port to be passed to node's `net.Server#listen()`, for iisnode s$
                port: '2368'
    



If you are SSH'd in to your Pi and you are both sat on the same network then change the host to the IP address of the Pi. For me this is 192.168.0.10, for you it may be something else. If you are running ghost directly from the Pi whilst being logged in to the gui of the Pi you can leave the IP as being 127.0.0.1. You also have the chance to change the port to something else if you wish. I have left mine the same as this is purely development environment and not the final production.

Hit Ctrl+X then Y and Enter to save.

You can now run the ghost:


    
    sudo npm start
    > ghost@0.3.3 start /var/www/ghost
    > node index
    



Go to the address of your Pi which for me would be: http://192.168.0.10:2368/ for you it may just be localhost:2368 or whatever IP you set in the config.js file.

If the web page loads up for you then you can now set up your account by going to /ghost at the end e.g. http://192.168.0.10:2368/ghost/

Complete the sign up form and wait a bit.. during development mode the server will take a while to respond but perservere and don't keep clicking join/signup etc give it chance! Once you are signed up you may have to login but you will then be directed to the ghost overview page!

From here you can edit settings, make new posts, review posts and logout. You will notice in your command window that you can see all of the GET URL requests and what is being loaded. This will do no good if we want to use this setup in a full production environment. Luckily the guys over at Ghost have provided a few ways of setting up Ghost permanently and in a production mode.

I prefer to leverage our newly installed NPM and Node.js so we will install a new package called forever.

    
    sudo npm install forever -g


Once installed run:

    
    pi@raspberrypi /var/www/ghost $ NODE_ENV=production forever start index.js
    warn:    --minUptime not set. Defaulting to: 1000ms
    warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    info:    Forever processing file: index.js
    pi@raspberrypi /var/www/ghost $
    



You will now have production version of ghost running. To make changes to this server or the development server stop the script using:

    
    forever stop index.js



Then edit your config.js file.

Well thats about all there is to it! I hope this has given enough insight in to how to install Ghost on to your raspberry pi. There are lots of options and more things that can be changed and edited but there is further info on this on the Ghost.org website. If you have any issues with the installation please leave a comment below and I will attempt to resolve with you. Have fun!
