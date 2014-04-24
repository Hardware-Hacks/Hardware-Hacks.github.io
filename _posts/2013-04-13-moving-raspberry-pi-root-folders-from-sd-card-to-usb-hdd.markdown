---
author: munkee
comments: true
date: 2013-04-13 15:05:18+00:00
layout: post
slug: moving-raspberry-pi-root-folders-from-sd-card-to-usb-hdd
title: Move Raspberry Pi Root File System (rootfs) From SD Card To Usb / HDD
wordpress_id: 23
categories:
- Raspberry PI
- Tutorials
tags:
- Raspberry PI
- USB
---

So it is a well known fact now that SD cards have a limited life with their read/writes. This poses a problem for the raspberry pi as the root file system is all sat on an SD. If you have ever been met with corruption issues or crashes you more than likely end up having to reflash a new image to the SD card which can result in you losing all of your set up. To get around this there have been a number of posts on the forums regarding moving the root folders out of the SD card and purely using a USB flash drive or HDD. 

There are a few benefits to doing this such as increased access/write speeds, being able to use much smaller SD cards (cheaper) and being able to reduce the chance of corruption of your files.

It has to be said not all the files can be moved, I have mentioned root files but the SD card has to keep the boot information. This allows the pi to mount the external drives. So.. with all of the above being said lets get down to a typical set up.

So I start off by plugging in a usb flash drive to my pi USB hub that was laying around. Its just a run of the mill 7GB data traveler 3G. Im unsure on the exact speeds initially but at the end of this post there are some benchmark results.

Once you have the drive plugged in you want to see where the root file system is currently sat. You should have one part of the SD card called /.

From the below code you can see the root directory is sat under rootfs and /dev/root. This could also be /dev/mmcblk0p2 for some people.

    
    
    pi@raspberrypi ~ $ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    rootfs          3.7G  2.0G  1.5G  58% /
    /dev/root       3.7G  2.0G  1.5G  58% /
    devtmpfs        227M     0  227M   0% /dev
    tmpfs            48M  264K   47M   1% /run
    tmpfs           5.0M     0  5.0M   0% /run/lock
    tmpfs            94M     0   94M   0% /run/shm
    /dev/mmcblk0p1   56M   19M   38M  34% /boot
    /dev/sdb1       298G   38G  260G  13% /media/usbhdd
    /dev/sda1       7.2G  145M  6.7G   3% /media/usbstick
    



You can also see from the above the usb drive is sat at /dev/sda1 I have used this in the past briefly when messing around with mounting hence why it is mounted on /media/usbstick. If yours is not mounted anywhere specifically do not worry.

To get the file system moved I first need to unmount the drive if it is mounted.


    
    
    pi@raspberrypi ~ $ sudo umount /dev/sda1
    



Now the usb drive is actually formatted in fat as it is an old one im using. For this to work all nice and dandy with linux it is best to reformat the drive using the following command. But we also first need to ensure that there is one partition on the drive which we will later expand out once the copying of the root is completed.

Type the following to bring up the partition information.


    
    
    pi@raspberrypi ~ $ sudo fdisk /dev/sda1
    



You will then need to ensure there are no partitions on the drive so press d and hit enter. Keep doing this until no partitions are present.

Now that we have a clean card we can create a single new partition by pressing n and then hitting p for primary partition then enter and enter again to take the default allocation values. Finally press w which will write all of the changes to the card.

We now have a card with a single fresh partition. The next step is to convert the fat system to a nice friendly linux ext4 format.

To do that we will re-format the drive as shown below.


    
    
    pi@raspberrypi ~ $ sudo mkfs.ext4 /dev/sda1
    



It is at this point that we should check that the file system is all set up correctly by running the following.


    
    
    pi@raspberrypi ~ $ sudo e2fsck /dev/sda1
    



The output for me was not so perfect but luckily the system offers all the assistance to get everything working correctly.


    
    
    e2fsck 1.42.5 (29-Jul-2012)
    Superblock needs_recovery flag is clear, but journal has data.
    Run journal anyway<y>? yes
    /dev/sda1: recovering journal
    Clearing orphaned inode 1281 (uid=107, gid=110, mode=0100600, size=0)
    Clearing orphaned inode 1275 (uid=107, gid=110, mode=0100600, size=0)
    Clearing orphaned inode 1267 (uid=107, gid=110, mode=0100600, size=0)
    Clearing orphaned inode 1266 (uid=107, gid=110, mode=0100600, size=0)
    Clearing orphaned inode 1254 (uid=107, gid=110, mode=0100600, size=0)
    Pass 1: Checking inodes, blocks, and sizes
    Pass 2: Checking directory structure
    Pass 3: Checking directory connectivity
    Pass 4: Checking reference counts
    Pass 5: Checking group summary information
    Free blocks count wrong (423611, counted=423580).
    Fix<y>? yes
    Free inodes count wrong (159160, counted=159154).
    Fix<y>? yes
    
    /dev/sda1: ***** FILE SYSTEM WAS MODIFIED *****
    /dev/sda1: 86606/245760 files (0.2% non-contiguous), 537188/960768 blocks
    pi@raspberrypi ~ $ sudo e2fsck /dev/sda1
    e2fsck 1.42.5 (29-Jul-2012)
    /dev/sda1: clean, 86606/245760 files, 537188/960768 blocks
    



As you can see at the end there I do a check once more just to be sure everything reports back clean. If your card was alright to start out with it would have just reported clean at the beginning.

What I will do now is mount the drive and see how everything looks.

    
    
    pi@raspberrypi ~ $ sudo mount /dev/sda1 /media/usbstick
    pi@raspberrypi ~ $ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    rootfs          3.7G  2.0G  1.5G  58% /
    /dev/root       3.7G  2.0G  1.5G  58% /
    devtmpfs        227M     0  227M   0% /dev
    tmpfs            48M  264K   47M   1% /run
    tmpfs           5.0M     0  5.0M   0% /run/lock
    tmpfs            94M     0   94M   0% /run/shm
    /dev/mmcblk0p1   56M   19M   38M  34% /boot
    /dev/sdb1       298G   38G  260G  13% /media/usbhdd
    /dev/sda1       3.7G  2.0G  1.5G  58% /media/usbstick
    



If you have not made the directory /media/usbstick then you will get an error saying the mount point does not exist. Just simply use the mkdir command to create the directory and then run the mount command again.

As you can see our sda1 drive is now showing 3.7G size when it is a 7.0GB stick this is because we used the default partition values. 

We can now begin copying over our root file system over to the card by carrying out the following command.


    
    
    pi@raspberrypi ~ $ sudo dd if=/dev/root of=/dev/sda1 bs=4M
    



This will take a while.. so be warned. It has taken me up to an hour for this command to complete so don't go turning off your pi midway through!

We can now expand the partition to fill the full 7GB as shown below. I have added the -p flag to the command which will give updates on how far through the command has got during execution.

    
    
    pi@raspberrypi ~ $ sudo resize2fs -p /dev/sda1
    resize2fs 1.42.5 (29-Jul-2012)
    Filesystem at /dev/sda1 is mounted on /media/usbstick; on-line resizing required
    old_desc_blocks = 1, new_desc_blocks = 1
    The filesystem on /dev/sda1 is now 1891455 blocks long.
    



This produces...


    
    
    pi@raspberrypi ~ $ df -h
    Filesystem      Size  Used Avail Use% Mounted on
    rootfs          3.7G  2.0G  1.5G  58% /
    /dev/root       3.7G  2.0G  1.5G  58% /
    devtmpfs        227M     0  227M   0% /dev
    tmpfs            48M  264K   47M   1% /run
    tmpfs           5.0M     0  5.0M   0% /run/lock
    tmpfs            94M     0   94M   0% /run/shm
    /dev/mmcblk0p1   56M   19M   38M  34% /boot
    /dev/sdb1       298G   38G  260G  13% /media/usbhdd
    /dev/sda1       7.2G  2.0G  4.9G  30% /media/usbstick
    




We can now start working on our fstab to ensure that the usb drive is mounted as root!

    
    
    pi@raspberrypi ~ $ sudo nano /media/usbstick/etc/fstab
    



Change your settings to show the following.

    
    
    proc            /proc           proc    defaults          0       0
    /dev/mmcblk0p1  /boot           vfat    defaults          0       2
    /dev/sda1       /               ext4    defaults,noatime  0       1
    



As you can see /dev/sda1 is now / (root). You can comment out the line that shows your sd card as / (root).

Our final step is to edit the boot cmdline.txt file. This is where we will tell the pi where to go looking for our root and where the fstab file exists etc.

    
    
    pi@raspberrypi ~ $ sudo nano /boot/cmdline.txt
    



You want your configuration to look as follows.

    
    
    dwc_otg.lpm_enable=0 console=ttyAMA0,115200 kgdboc=ttyAMA0,115200 console=tty1 root=/dev/sda1 rootfstype=ext4 elevator=deadline rootwait
    



The final step... reboot!

    
    
    pi@raspberrypi ~ $ sudo reboot
    
    Broadcast message from root@raspberrypi (pts/0):
    The system is going down for reboot NOW!
    



Hopefully your system has booted correctly! if you want you can now remove the root file system on the sd card and even lock the card completely so that it no longer gets written to.

As promised I said I would post some benchmarks, which were quite surprising considering I had read that one of the major benefits to moving was the increase in read/write speed. As you can see I can not say the same! To be honest though as with most of the pi projects it all comes down to the quality of the components you use.

Speed tests: 
USB

    
    
    /dev/sda:
     Timing cached reads:   258 MB in  2.00 seconds = 128.85 MB/sec
     Timing buffered disk reads:  24 MB in  3.08 seconds =   7.79 MB/sec
    



SD

    
    
    /dev/mmcblk0p1:
     Timing cached reads:   332 MB in  2.01 seconds = 165.36 MB/sec
     Timing buffered disk reads:  56 MB in  2.89 seconds =  19.40 MB/sec
    



HDD

    
    
    /dev/sdb1:
     Timing cached reads:   296 MB in  2.01 seconds = 147.56 MB/sec
     Timing buffered disk reads:  80 MB in  3.02 seconds =  26.48 MB/sec
    



I am not too bothered about the slower speed I can at least now live with the fact I should see much less corruption!
