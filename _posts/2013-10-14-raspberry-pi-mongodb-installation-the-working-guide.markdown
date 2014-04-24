---
author: munkee
comments: true
date: 2013-10-14 13:26:10+00:00
layout: post
slug: raspberry-pi-mongodb-installation-the-working-guide
title: Raspberry Pi MongoDB Installation - The working guide!
wordpress_id: 377
categories:
- Raspberry PI
- Tutorials
tags:
- Database Connection
- init.d
- MySql
- Python
- Raspberry PI
---

In this tutorial I will go through how to install MongoDb on your Raspberry Pi and also give a few tips on typical issues and fixes.
MongoDb is one of the most popular "NoSQL" databases out there. NoSQL releases you from the constraints of a typical relational database. They essentially allow you to store data in "key - value" stores which means you do not really have a typical schema as you would see in most other databases. They are often all open source and are horizontally scalable. MongoDB is said to be a document-oriented database which is designed for ease of development and scaling.

[caption width="217" align="aligncenter"]![](http://media.mongodb.org/logo-mongodb.png) MongoDB[/caption]

So why am I not considering MYSQL and the likes? Well I already run MySql on my Pi and I have to say the performance has been perfectly fine. This blog runs solely of my Pi and the wordpress data all sits on MySql. I have heard a lot of arguments against using it on the Pi due to resource usage etc, however it has run perfectly fine for me with little overhead. However with all of that being said you should never constrain yourself and I am currently working on a number of projects that require me to hold lots of random data together in a database. I want to be able to work quickly and not spend time having to set up table after table and check my relations, field types etc. I just want to store data and access it simply. This is where MongoDB comes in and really works well.

Before kicking off with the installation instructions I will add a few links for further reading below so you can get a bit more information about MongoDb, its usages, its good and bad points and also why you should consider NoSQL.

**Further Reading:**
 - [What is noSql?](http://www.mongodb.com/nosql)
 - [Advantages & Disadvantages of NoSql](http://www.techrepublic.com/blog/10-things/10-things-you-should-know-about-nosql-databases/)
 - [When NoSQL makes sense to use](http://www.informationweek.com/software/information-management/when-nosql-makes-sense/240162225)


Lets get on with the installation. Firstly get yourself logged in to a terminal window on your Pi, either from the desktop or SSH straight in. As always we will run the following two commands:


    
    sudo apt-get update
    sudo apt-get upgrade



Next we will dive in to grabbing a fair few libraries. If you have followed any of my previous blogs you will probably have a few of these already but essentially these will allow us to use github, build tools, python implementation of mongo client and various file/system extensions.


    
    
    sudo apt-get install build-essential libboost-filesystem-dev libboost-program-options-dev libboost-system-dev libboost-thread-dev scons libboost-all-dev python-pymongo git
    



Just hit yes to any of the other libraries it wants to pull in once you use the above command. We then move on to grabbing a version of MongoDb that has been edited to work on the Pi. If you try and install Mongo most other ways you get compile errors and that comes back to the same issues we have seen with the likes of Mono, at the moment quite a few products out there do not support the Pi architecture. But that is not a problem for us today as Skrabban has hosted an edited version for us.


    
    cd ~



    
    git clone https://github.com/skrabban/mongo-nonx86



This will clone the repository in to the mongo-nonx86 directory.

    
    cd mongo-nonx86



We are now sat in an uncompiled version of MongoDb for the Pi, we will have to compile this ourselves on the Pi. If you are using one of the model A Pi's this is mostly likely going to be a problem for you as the amount of RAM can be too low and max out. I will also point out that compiling takes up SD Card space so make sure you over 500mb free.

The following code should be executed within our mongo-nonx86 directory. It will execute "Scons" which is a software construction program written in python. WARNING: The below code will take hours to run on your Pi, try letting it run over night.


    
    sudo scons



This above code will build the repository for us. When I first ran this after a few hours I hit an error during compiling. I expect it was because I ran "scons" without sudo. However even with the terminating error I just ran "sudo scons" once more and everything built correctly.

We can now run the installation code.


    
    sudo scons --prefix=/opt/mongo install



Once again the above code can take a few hours to complete, for me it took about 4 hours in total. You should now have an installation of mongo on your Pi. We now need to do some general housekeeping with folders and permissions then finally an init.d script to be able to kick off mongodb easily.

Permissions for users and groups:

    
    sudo adduser --firstuid 100 --ingroup nogroup --shell /etc/false --disabled-password --gecos "" --no-create-home mongodb



A folder for our log files to go:

    
    sudo mkdir /var/log/mongodb/



Permissions for log file:

    
    sudo chown mongodb:nogroup /var/log/mongodb/



A folder for our state data:

    
    sudo mkdir /var/lib/mongodb



Permissions for the folder:

    
    sudo chown mongodb:nogroup /var/lib/mongodb



Moving our init.d script to etc:

    
    sudo cp debian/init.d /etc/init.d/mongod



Moving our config file to etc:

    
    sudo cp debian/mongodb.conf /etc/



Linking folders up:

    
    sudo ln -s /opt/mongo/bin/mongod /usr/bin/mongod



The above code as said does a lot of housekeeping but you may notice that when trying to run the init.d script you hit some errors. We need to finalise permissions and make sure the init.d script is executable so complete the installation with the final statements.


    
    sudo chmod u+x /etc/init.d/mongod



    
    sudo update-rc.d mongod defaults


You may get some warning when executing the above statement however these can be ignored.

We can now start MongoDB using the following command:

    
    sudo /etc/init.d/mongod start



To connect to the instance you can then type:

    
    mongo



You should then see connected to test:

    
    pi@raspberrypi ~ $ mongo
    MongoDB shell version: 2.1.1-pre-
    connecting to: test
    >
    



So now we have a mongo instance running on our server at http://localhost:27017 however mongo also has a very basic interface you can access at http://localhost:28017 just to give basic information.

If you click a link from that page and are hit with:

    
    REST is not enabled.  use --rest to turn on.



Then we simply need to edit the init.d script and add in --rest as a parameter. The code below will show you how to do this.


    
    sudo nano /etc/init.d/mongod



Scroll down to:

    
    DAEMONUSER=${DAEMONUSER:-mongodb}
    DAEMON_OPTS=${DAEMON_OPTS:-"--dbpath $DATA --logpath $LOGFILE run"}
    DAEMON_OPTS="$DAEMON_OPTS --config $CONF"
    



And add in --rest to give:

    
    DAEMONUSER=${DAEMONUSER:-mongodb}
    DAEMON_OPTS=${DAEMON_OPTS:-"--dbpath $DATA --logpath $LOGFILE run"}
    DAEMON_OPTS="$DAEMON_OPTS --config $CONF --rest"
    



Hit Ctrl+X, Y and then restart mongodb using:

    
    sudo /etc/init.d/mongod restart



You will then have all of the options available to you through the mongo UI webpage. Well, for now thats about it. In later tutorials I will go through creation of collections, saving data, reading data and some of the uses you can put MongoDB to. If you have any problems/comments please leave them in the comments section below and I'll get back to you with answers in no time.

Enjoy!
