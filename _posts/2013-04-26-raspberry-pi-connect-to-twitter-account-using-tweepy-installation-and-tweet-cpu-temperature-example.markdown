---
author: munkee
comments: true
date: 2013-04-26 09:31:36+00:00
layout: post
slug: raspberry-pi-connect-to-twitter-account-using-tweepy-installation-and-tweet-cpu-temperature-example
title: Raspberry Pi Connect To Twitter Account Using Tweepy - Installation and Tweet
  CPU Temperature Example
wordpress_id: 175
categories:
- Raspberry PI
- Tutorials
tags:
- cron
- pip
- Python
- Raspberry PI
- Temperature
- Tweepy
- Twitter
---

After some inspiration from the pi bot on twitter I wanted to investigate how easy it is to connect my pi a spare account and tweet messages from the command line. The following tutorial will outline everything you need to get your first tweets sent. I have extended this to tweet the current cpu temperature on the pi board. There are a number of sensors and I will eventually be switching to also monitor the air temperature around the pi.


The raspberry pi will come with python pre-installed so it makes sense we use this language to do the work in this project. Before we begin it always useful to run an apt-get update to ensure all of your packages are running the best versions. We will start off by installing python-setuptools which essentially allows you to install, update, remove python packages nice and easy. We will be using a package called tweepy which has everything inside that we need to send our messages out to twitter.

To Install the setup tools and tweepy carry out the following commands:


    
    
    pi@raspberrypi ~ $ sudo apt-get install python-setuptools
    pi@raspberrypi ~ $ sudo easy_install pip
    pi@raspberrypi ~ $ sudo pip install tweepy
    



Once these have all installed correctly we will need to head over to the twitter developer area to register an application against our twitter account. The link for this can be found [here](https://dev.twitter.com/apps).

You will need to then carry out the following steps in order to get access keys and consumer keys which will allow us to authenticate the twitter account and application without having to keep entering in login data. This uses the OAuth protocol which you can find out more about using google!




	
  1. Select create a new application

	
  2. Enter name, description, website

	
  3. Select Yes I agree to the terms and conditions

	
  4. Enter captcha information and click submit

	

You will see that by default the access level is set to read only. We will need to change this to read/write to enable us to push tweets out to the world. To change this settings carry out the following steps.


	
  1. Select settings along top menu tabs

	
  2. Under application type select Read and Write

	
  3. Ensure that "Allow this application to be used to Sign in with Twitter" is checked

	
  4. Click the update this twitter applications settings button at the bottom of the page



If you now return to the Details tab you will see a number of special keys which include, consumer key/secret and access token/secret. Leave the webpage open as we will need all of this information in a minute to start tweeting.

As in my previous projects I have a directory set up already for my project files. It is housed under my /home/pi folder in a very original folder name called Projects.

I will now create a new folder called Twitter within this Projects folder but you can choose to save your files wherever you feel is relevant.


    
    
    pi@raspberrypi ~ $ sudo mkdir /home/pi/Projects/Twitter
    pi@raspberrypi ~ $ cd /home/pi/Projects/Twitter
    pi@raspberrypi ~/Projects/Twitter $ 
    



Within this director I will create a file to house my python code that will do all of the work called Tweeter.


    
    
    pi@raspberrypi ~/Projects/Twitter $ sudo nano Tweeter.py
    



This will open up a new blank file for us where we can paste the following python code.


    
    
    #!/usr/bin/env python
    
    import sys
    import tweepy
    
    CONSUMER_KEY = '***************YOUR DATA*****************'
    CONSUMER_SECRET = '***************YOUR DATA*****************'
    ACCESS_KEY = '***************YOUR DATA*****************'
    ACCESS_SECRET = '***************YOUR DATA*****************'
    
    
    auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
    auth.set_access_token(ACCESS_KEY, ACCESS_SECRET)
    api = tweepy.API(auth)
    api.update_status(sys.argv[1])
    




What you now need to do is take all of the relevant key and secret information from the twitter application site we were using earlier and paste the relevant parts into the above file where I have marked "***************YOUR DATA*****************".

Once you have done this use CTRL+X then Y to save your file.

The above code will allow us to stipulate exactly what we want to send to from the command line. I like to first though ensure my file is executable so run the following.


    
    
    pi@raspberrypi ~/Projects/Twitter $ sudo chmod 755 Tweeter.py
    



We are now set! To send out your first tweet run the following command.


    
    
    pi@raspberrypi ~/Projects/Twitter $ python Tweeter.py 'Hello World!'
    pi@raspberrypi ~/Projects/Twitter $
    



As long as you get nothing returned by that call then everything has worked perfectly, head over to your twitter feed and you should see your Hello World tweet!

For now this is not all that useful however there is now the opportunity to expand on this initial example and make it in to something so much more awesome. At the start of the tutorial I said I wanted to tweet the current CPU temperature and eventually the temperature of the environment around the pi. I will show you the code required to get the current CPU temperature running.


We will first create a second python file so we do not destroy the hard work we have just carried out.





    
    
    pi@raspberrypi ~/Projects/Twitter $ sudo nano Tweet_Status.py
    



You can then copy and paste the following code directly into the file.


    
    
    #!/usr/bin/env python
    import os
    import sys
    import tweepy
    
    CONSUMER_KEY = '*****YOUR DATA******'
    CONSUMER_SECRET = '*****YOUR DATA******'
    ACCESS_KEY = '*****YOUR DATA******'
    ACCESS_SECRET = '*****YOUR DATA******'
    
    
    auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
    auth.set_access_token(ACCESS_KEY, ACCESS_SECRET)
    api = tweepy.API(auth)
    
    cmd = '/opt/vc/bin/vcgencmd measure_temp'
    line = os.popen(cmd).readline().strip()
    temp = line.split('=')[1].split("'")[0]
    
    api.update_status('My Current Processor Temperature: '+ temp + ' C')
    



Once again you will need to replace the YOUR DATA place holders with your keys/secrets etc from earlier. The above file now contains code that will ask the pi to output its current cpu temperature to the command line, read it and then format it in a nice way and push it out to twitter via the api.update_status call. There are many temperature sensors we can plug into on the pi and also even measure voltages so you can expand this script if you like to include a full system overview.

We can now save the file and close it with Ctrl + x and then Y.

Once again we need to ensure the file is executable.

    
    
    pi@raspberrypi ~/Projects/Twitter $ sudo chmod 755 Tweet_Status.py
    



If you now run this file your twitter feed should show the current CPU temperature.

    
    
    pi@raspberrypi ~/Projects/Twitter $ python Tweet_Status.py
    pi@raspberrypi ~/Projects/Twitter $
    



We can now use a cron job (Scheduled Task) to run this script every hour so we get a constant update on twitter about our CPU temp.

To edit the root cron file you must run the following command.

    
    
    pi@raspberrypi ~/Projects/Twitter $ sudo crontab -e
    



It is very important to ensure the sudo command is used!
Once you are in the file you can then add the following line, just change the directory to point to where your file is saved.


    
    
    */60 * * * * python /home/pi/Projects/Twitter/Tweet_Status.py
    



Thats it all done.. if you have any feedback please leave a comment!
