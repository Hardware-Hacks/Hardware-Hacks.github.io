---
author: munkee
comments: true
date: 2013-12-23 13:03:36+00:00
layout: post
slug: installing-seafile-cloud-file-server-on-your-raspberry-pi
title: Raspberry Pi Install Seafile Cloud File Server
wordpress_id: 407
categories:
- Raspberry PI
- Tutorials
tags:
- fast_cgi
- init.d
- owncloud
- Projects
- Python
- Raspberry PI
- Seafile
- Server
---

Todays post will be running through the installation of a personal cloud service called Seafile on to the raspberry pi. For those of you that follow the blog I have previously been using owncloud and I spent a lot of time [optimising most of the server settings](http://c-mobberley.com/wordpress/index.php/2013/05/18/raspberry-pi-lighttpd-optimisation-for-wordpress-owncloud-and-more/) to try and [get owncloud working smooth on my Pi](http://c-mobberley.com/wordpress/index.php/2013/04/30/raspberry-pi-web-server-speed-optimisation-for-slow-wordpress-owncloud-issues/). I have to say I have finally admitted defeat. Owncloud is an excellent set up in most instances but I think it really bogs the Pi down and you spend quite a few seconds waiting for a response and renders which overall makes the service a bit meh..

I decided it was time to investigate what else was out there and I noticed a previous service that I had looked at before now has a Raspberry Pi branded package for their server, it seems that Seafile has been doing some work!

If you want further information on what Seafile is and all of the options I will allow you to catch all of that info up yourself with a link to the [site here](http://www.seafile.com/). For now though this blog post will focus on the install so lets get to it!

First off as always get yourself up to date.


    
    pi@raspberrypi ~ $ sudo apt-get update



Then its time to get all of the python dependencies and the Pi version of sqlite3. if you don't carry out this step the Seafile installer will just tell you to install them anyway so its best to get it out of the way.


    
    pi@raspberrypi ~ $ sudo apt-get install python2.7 python-setuptools python-simplejson python-imaging sqlite3



Now as I mentioned earlier Seafile have produced a Raspberry Pi version of their server. Lets download it with wget. By the time you read this post it is probably useful to check if a new version is available. [You can find the versions here](http://www.seafile.com/en/download/). I will be installing version 2.0.3 of the server. Your version may differ but the process is still the same.


    
    pi@raspberrypi ~ $ cd ~
    pi@raspberrypi ~ $ wget http://seafile.googlecode.com/files/seafile-server_2.0.3_pi.tar.gz
    --2013-12-23 07:40:57--  http://seafile.googlecode.com/files/seafile-server_2.0.3_pi.tar.gz
    Resolving seafile.googlecode.com (seafile.googlecode.com)... 173.194.78.82, 2a00:1450:400c:c00::52
    Connecting to seafile.googlecode.com (seafile.googlecode.com)|173.194.78.82|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 14026899 (13M) [application/x-gzip]
    Saving to: `seafile-server_2.0.3_pi.tar.gz'
    
    100%[==============================>] 14,026,899  1.71M/s   in 9.1s
    
    2013-12-23 07:41:07 (1.48 MB/s) - `seafile-server_2.0.3_pi.tar.gz' saved [14026899/14026899]



We now have the gzipped tar file that's needed. It's now time to extract.


    
    pi@raspberrypi ~ $ tar -xzf seafile-server_*
    pi@raspberrypi ~ $ sudo mkdir seafile_installed
    pi@raspberrypi ~ $ sudo mv seafile-server-2.0.3 seafile_installed



The above commands will extract the file and also I created a directory for the installer to sit in. We can now run the setup file.


    
    pi@raspberrypi ~ $ cd seafile_installed/
    pi@raspberrypi ~/seafile_installed $ ./setup-seafile.sh



As soon as you run the setup script you are presented with a very easy to use command program. Just follow the prompts and set up your server as you wish. For illustration purposes below is how my setup went.


    
    -----------------------------------------------------------------
    This script will guide you to config and setup your seafile server.
    
    Make sure you have read seafile server manual at
    
            https://github.com/haiwen/seafile/wiki
    
    Press [ENTER] to continue
    -----------------------------------------------------------------
    
    
    Checking packages needed by seafile ...
    
    Checking python on this machine ...
    Find python: python2.7
    
      Checking python module: setuptools ... Done.
      Checking python module: python-simplejson ... Done.
      Checking python module: python-imaging ... Done.
      Checking python module: python-sqlite3 ... Done.
    
    Checking for sqlite3 ...Done.
    
    Checking Done.
    
    
    What do you want to use as the name of this seafile server?
    Your seafile users would see this name in their seafile client.
    You can use a-z, A-Z, 0-9, _ and -, and the length should be 3 ~ 15
    [server name]: #############
    
    What is the ip or domain of this server?
    For example, www.mycompany.com, or, 192.168.1.101
    
    [This server's ip or domain]: 192.168.0.10
    
    What tcp port do you want to use for ccnet server?
    10001 is the recommended port.
    [default: 10001 ]
    
    Where do you want to put your seafile data?
    Note: Please use a volume with enough free space.
    [default: /home/pi/seafile-data ]
    
    What tcp port do you want to use for seafile server?
    12001 is the recommended port.
    [default: 12001 ]
    
    What tcp port do you want to use for seafile httpserver?
    8082 is the recommended port.
    [default: 8082 ]
    
    
    This is your config information:
    
    server name:        ############
    server ip/domain:   192.168.0.10
    server port:        10001
    seafile data dir:   /home/pi/seafile-data
    seafile port:       12001
    httpserver port:    8082
    
    If you are OK with these configuration, press [ENTER] to continue.
    
    Generating ccnet configuration in /home/pi/ccnet...
    
    done
    Successly create configuration dir /home/pi/ccnet.
    
    Generating seafile configuration in /home/pi/seafile-data ...
    
    Done.
    
    -----------------------------------------------------------------
    Seahub is the web interface for seafile server.
    Now let's setup seahub configuration. Press [ENTER] to continue
    -----------------------------------------------------------------
    
    
    Please specify the email address and password for seahub admininstrator.
    You would use them to login as admin on your seahub website.
    
    Please specify the email address for seahub admininstrator:
    [seahub admin email]: blahblah@gmail.com
    
    Please specify the passwd you want to use for seahub admininstrator:
    [seahub admin password]: somesecretpass
    
    Please ensure the passwd again:
    [seahub admin password again]: somesecretpass
    
    
    
    This is your seahub admin username/password
    
    admin user name:        blahblahblah@gmail.com
    admin password:         somesecretpass
    
    
    If you are OK with these configuration, press [ENTER] to continue.
    
    Now sync seahub database ...
    
    Loading ccnet config from /home/pi/ccnet
    Loading seafile config from /home/pi/seafile-data
    Creating tables ...
    Creating table django_content_type
    Creating table django_session
    Creating table avatar_avatar
    Creating table avatar_groupavatar
    Creating table registration_registrationprofile
    Creating table base_uuidobjidmap
    Creating table base_filecomment
    Creating table base_filecontributors
    Creating table base_innerpubmsg
    Creating table base_innerpubmsgreply
    Creating table base_userstarredfiles
    Creating table base_dirfileslastmodifiedinfo
    Creating table contacts_contact
    Creating table group_groupmessage
    Creating table group_messagereply
    Creating table group_messageattachment
    Creating table group_businessgroup
    Creating table notifications_notification
    Creating table notifications_usernotification
    Creating table profile_profile
    Creating table share_anonymousshare
    Creating table share_fileshare
    Creating table api2_token
    Installing custom SQL ...
    Installing indexes ...
    No fixtures found.
    
    Done.
    
    -----------------------------------------------------------------
    Your seafile server configuration has been finished successfully.
    -----------------------------------------------------------------
    
    run seafile server:     ./seafile.sh { start | stop | restart }
    run seahub  server:     ./seahub.sh  { start <port> | stop | restart <port> }
    
    -----------------------------------------------------------------
    If you are behind a firewall, remember to allow input/output of these tcp ports:
    -----------------------------------------------------------------
    
    port of ccnet server:         10001
    port of seafile server:       12001
    port of seafile httpserver:    8082
    port of seahub:               8000
    
    When problems occur, Refer to
    
          https://github.com/haiwen/seafile/wiki
    
    for information.
    
    pi@raspberrypi ~/seafile_installed $



As you can see the setup is pretty smooth. You should be able to access the server from your Pi's IP address with the Seahub port address which is normally 8000.


    
    http://192.168.0.10:8000




Whilst you can start the server nice and easy using:


    
    sudo ./seafile.sh start


and

    
    sudo ./seahub.sh start



It would be better to start these automatically though... and that's what init.d scripts are for!


    
    sudo nano /etc/init.d/seafile-server



Then copy and paste the following in to the file (right click in the file space if you are ssh'd in to paste):


    
    
    #!/bin/sh
    
    # Change the value of "user" to your linux user name
    user=root
    
    # Change the value of "script_path" to your path of seafile installation
    seafile_dir=/home/pi/seafile_installed
    script_path=/home/pi/seafile_installed
    seafile_init_log=${seafile_dir}/logs/seafile.init.log
    seahub_init_log=${seafile_dir}/logs/seahub.init.log
    
    # Change the value of fastcgi to true if fastcgi is to be used
    fastcgi=false
    # Set the port of fastcgi, default is 8000. Change it if you need different.
    fastcgi_port=8000
    case "$1" in
            start)
                    sudo -u ${user} ${script_path}/seafile.sh start > ${seafile_init_log}
                    if [  $fastcgi = true ];
                    then
                            sudo -u ${user} ${script_path}/seahub.sh start-fastcgi ${fastcgi_port} > ${seahub_init_log}
                    else
                            sudo -u ${user} ${script_path}/seahub.sh start > ${seahub_init_log}
                    fi
            ;;
            restart)
                    sudo -u ${user} ${script_path}/seafile.sh restart > ${seafile_init_log}
                    if [  $fastcgi = true ];
                    then
                            sudo -u ${user} ${script_path}/seahub.sh restart-fastcgi ${fastcgi_port} > ${seahub_init_log}
                    else
                            sudo -u ${user} ${script_path}/seahub.sh restart > ${seahub_init_log}
                    fi
            ;;
            stop)
                    sudo -u ${user} ${script_path}/seafile.sh $1 > ${seafile_init_log}
                    sudo -u ${user} ${script_path}/seahub.sh $1 > ${seahub_init_log}
            ;;
            *)
                    echo "Usage: /etc/init.d/seafile {start|stop|restart}"
                    exit 1
            ;;
    esac
    



There will be a couple of areas where you need to change values such as root may need to be pi and also the server port may be different if you changed this during your setup. Once you have added the details above hit ctrl+x and then Y and it will save the file.

Now we move on to a bit of extra server config as shown below.


    
    sudo nano /etc/init.d/seafile-server.conf



Then add the following to the file.


    
    
    start on (started mysql
    and runlevel [2345])
    stop on (stopping mysql
    and runlevel [016])
    
    pre-start script
    /etc/init.d/seafile-server start
    end script
    
    post-stop script
    /etc/init.d/seafile-server stop
    end script
    



Finally we need to ensure the file is executable.


    
    sudo chmod +x /etc/init.d/seafile-server



That's it! You should now be able to start/stop or restart seafile much easier using:


    
    sudo /etc/init.d/seafile-server start|stop|restart



Hope this has been of use. I am really impressed with the speed of Seafile in comparison to owncloud and hopefully you will be too. Please leave feedback on any issues and I will do my best to reply ASAP. Also if you can directly compare the performance of owncloud and seafile I will be happy to hear about it too.
