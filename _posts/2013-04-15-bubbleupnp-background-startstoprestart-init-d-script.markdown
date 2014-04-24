---
author: munkee
comments: true
date: 2013-04-15 21:17:54+00:00
layout: post
slug: bubbleupnp-background-startstoprestart-init-d-script
title: BubbleUPnP Background Start/Stop/Restart Init.D Script
wordpress_id: 59
categories:
- Fixes
- Raspberry PI
tags:
- BubbleUPnP
- daemon
- init.d
- Raspberry PI
---

Well it seems a few people have been asking how to run their bubble server in the background on starting their pi. You will first need a nice init.d script. Modify the following code to point to your folder where the .jar file for BubbleUPnPServer resides.


    
    
    #!/bin/bash
    ### BEGIN INIT INFO
    # Provides:          BubbleServer
    # Required-Start:    $remote_fs $syslog
    # Required-Stop:     $remote_fs $syslog
    # Default-Start:     2 3 4 5
    # Default-Stop:      0 1 6
    # Short-Description: BubbleUPnP Server Background Service Management
    # Description:       Used to ensure BubbleUPnP starts/stops etc
    ### END INIT INFO
    
    DAEMON_PATH="/var/www/bubbleupnp"
    
    DAEMON="java -Xss256k -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true -jar BubbleUPnPServer.jar"
    DAEMONOPTS=""
    
    NAME=BubbleUPnPServer
    DESC="Runs BubbleUPnPServer"
    PIDFILE=/var/run/$NAME.pid
    SCRIPTNAME=/etc/init.d/$NAME
    
    case "$1" in
    start)
      printf "%-50s" "Starting $NAME..."
      cd $DAEMON_PATH
      PID=`$DAEMON $DAEMONOPTS > /dev/null 2>&1 & echo $!`
      #echo "Saving PID" $PID " to " $PIDFILE
            if [ -z $PID ]; then
                printf "%sn" "Fail"
            else
                echo $PID > $PIDFILE
                printf "%sn" "Ok"
            fi
    ;;
    status)
            printf "%-50s" "Checking $NAME..."
            if [ -f $PIDFILE ]; then
                PID=`cat $PIDFILE`
                if [ -z "`ps axf | grep ${PID} | grep -v grep`" ]; then
                    printf "%sn" "Process dead but pidfile exists"
                else
                    echo "Running"
                fi
            else
                printf "%sn" "Service not running"
            fi
    ;;
    stop)
            printf "%-50s" "Stopping $NAME"
                PID=`cat $PIDFILE`
                cd $DAEMON_PATH
            if [ -f $PIDFILE ]; then
                kill -HUP $PID
                printf "%sn" "Ok"
                rm -f $PIDFILE
            else
                printf "%sn" "pidfile not found"
            fi
    ;;
    
    restart)
        $0 stop
        $0 start
    ;;
    
    *)
            echo "Usage: $0 {status|start|stop|restart}"
            exit 1
    esac
    



You should be creating and saving the file as 

    
    
    /etc/init.d/BubbleUPnPServer
    
    



After this you will want to make the script executable.


    
    
    sudo chmod 755 /etc/init.d/BubbleUPnPServer
    



And finally updating rc.d so the script is executed on boot up.

    
    
    sudo update-rc.d BubbleUPnPServer defaults
    



You can now go ahead and kick off your server without requiring the restart.

    
    
    sudo /etc/init.d/BubbleUPnPServer start
    



And to stop?


    
    
    sudo /etc/init.d/BubbleUPnPServer stop
    



And to check status?


    
    
    sudo /etc/init.d/BubbleUPnPServer status
    



Hope this helps someone in the future! it is a bit of a nightmare when you first look but after some init.d daemon script template googling you can soon wire up a solution.
