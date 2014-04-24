---
author: munkee
comments: true
date: 2013-05-18 12:24:41+00:00
layout: post
slug: raspberry-pi-lighttpd-optimisation-for-wordpress-owncloud-and-more
title: Raspberry Pi Lighttpd Optimisation For Wordpress Owncloud and More
wordpress_id: 258
categories:
- Raspberry PI
- Tutorials
tags:
- fast_cgi
- lighttpd
- owncloud
- Performance
- PHP
- php-apc
- Raspberry PI
- Server
- Wordpress
---

So after the popularity of my post regarding optimisation of apache in a pure LAMP setup on the Raspberry Pi I want to talk about optimising lighttpd for the Raspberry Pi also.

During this tutorial I will go through installing PHP APC, ensuring we have caching optimised for our static content, ensuring we have fast cgi enabled and setting up our compression with gzip correctly.

One of the main reasons I decided to switch away from lighttpd on my first attempt of using my Pi as a web server was that it seemed a bit more involved in setting everything up. On the other hand Apache seemed to have a lot of things enabled out of the box and there were a ton of resources out there to help along the way. However Apache has its downfalls, it is simply too resource hungry for the poor Raspberry Pi and after some botched up settings I was forced to return my Pi back to its default settings and kicked off my usage of Lighttpd once again. After using Apache for a few months it was really refreshing to see how a vanilla installation of Lighttpd was able to offer so much out of the box speed. The resource usage dropped, the number of processes dropped and all in all the amount of small nigling issues I had been facing when trying to optimise Apache on such a low end machine as the Pi has also gone away.


So.. for the rest of this tutorial I will assume that you have Lighttpd installed. It is very simply by the way you can simply do..

    
    
    sudo apt-get install lighttpd
    



Now on to the real reason you are here, optimisation!

**Fast CGI**

I will not go into the details here around what fast cgi is, just think of it as awesome and lightweight and more awesome. I explained more about it in my last optimisation tutorial for apache so if you are really interested to learn more why not take a look!

So to kick things off you will be pleased to know this is quite easy to install with lighttpd and especially on the raspberry pi. Lets start by getting the php5-cgi package.


    
    
    sudo apt-get install php5-cgi
    



Once the package and all of the dependencies have installed we need to edit our lighttpd configuration file and also our php.ini file. Do not worry to much about where I show my php.ini file location it can differ on everyones installation so it might take a few attempts to locate it correctly. 

To edit my php.ini file I carry out the following.


    
    
    sudo nano /etc/php5/cgi/php.ini
    



Once you are in the file hit Ctrl+w and search for "cgi.fix_pathinfo".
Uncomment the line where it says:

    
    
    ;cgi.fix_pathinfo=1
    



to show


    
    
    cgi.fix_pathinfo=1
    



Alternatively you can just type out the above line at the end of your file if you are less comfortable with doing a text search.


We can now dive in to lighttpd.conf.

    
    
    sudo nano /etc/lighttpd/lighttpd.conf
    



From here we need to enable our server module "mod_fastcgi". At this point you may want to jump to the end of this tutorial and copy and paste my standard module list. Comment out everything you do not need but remember to leave in "mod_fastcgi" as we now want this enabled.

The final edit is to let our lighttpd.conf file know we want to push php through fastcgi. Add the following lines to your configuration.


    
    
    fastcgi.server = ( ".php" => ((
                         "bin-path" => "/usr/bin/php5-cgi",
                         "socket" => "/tmp/php.socket"
                     )))
    



From here the final step is to restart out lighttpd server.


    
    
    sudo service lighttpd restart
    



There we go, fastcgi support for php.This will give us a nice performance increase due to a reduction in processor usage and resource usage... lovely.

**PHP APC**
We will now install PHP APC which is easily one of the best changes you can make to really give you server a speedy push in the right direction. 

We first need to install the php extension installer PECL.


    
    
    sudo apt-get install libpcre3-dev php-pear php5-dev build-essential
    



We now grab the apc extension with pecl.


    
    
    sudo pecl install apc
    



This installation takes some time and there are questions to answer. For now just take the default answer to each by hitting your enter key apart from when Apache is mentioned, just select no for any questions related to Apache.

We now create our configuration for apc.

    
    
    sudo nano /etc/php5/cgi/conf.d/apc.ini
    



Add the following lines to the file.

    
    
    extension=apc.so
    apc.enabled=1
    apc.shm_size=30
    



Now restart lighttpd

    
    
    sudo service lighttpd restart
    



BOOM! another awesome package installed and in improvement on our lighttpd speeds.

**Expiration**
Expiration is important. It essentially allows us to serve static files to a user and tell the users browser how long it should keep the file for. Why is this important? because it is useless to keep serving a user with the same file on the fly. That is extra requests, extra data transfer and extra work for our beloved Raspberry Pi that we do not need to keep doing.

By adding a date to which static content is valid for the browser will ask for the file then get the request cancelled because it realises oh yea I have this file already I do not need to ask for it. This pushes the processing of the file back on to the client where there will be a lot less rendering time.

So what do I mean by static content? Well I do not mean php, php is pretty dynamic for the most part. I'm talking more about those javascript files, stylesheets, images, html files , text files etc that rarely change. They just sit there making everything look pretty.

Lets get in to configuring lighttpd to set expiration values. You may wish to take note this is just my setup, it works fine for me. You may wish to tweak settings depending on how much your website changes but for me wordpress and my other programs are pretty damn static outside of the realms of php so I set expiration on as much as I can.

First we need to open up our lighttpd configuration file.


    
    
    sudo nano /etc/lighttpd/lighttpd.conf
    



Next we need to take a look at our module list and enable mod_expire or add it in. It needs to be one of th every last modules to be enabled. Once again take a look at the end of this post to see the standard order I recommend you to use.

In my cut down version this is how my order looks.

    
    
    server.modules = (
            "mod_redirect",
            "mod_alias",
            "mod_access",
            "mod_fastcgi",
            "mod_compress",
            "mod_expire"
    
    )
    



Once again notice mod expire is one of the very first modules to be loaded (but shows as last in the list).

Now we need to work on the rest of our configuration. You will need to add in some code as shown below.


    
    
    $HTTP["url"] =~ ".(jpg|gif|png|css|js|svg)$" {
        expire.url = ( "" => "access 7 days" )
    }
    etag.use-inode = "enable"
    etag.use-mtime = "enable"
    etag.use-size = "enable"
    static-file.etags = "enable"
    



Firstly I declare that anything in my url that contains jpg,css,js etc etc will be cached. The caching/expiry will last from the day they access the file up until 7 days after. This is great when you think visitors will visit multiple pages and may come back within the next week to read more articles.

My list of files I have added expiration to are typically scripts and images. I could have added in html files but I can not guarantee that they will always remain static so I thought it best to leave these out.

You will also notice I have declared etags. These are important for our images. Without an etag some browsers end up doing a bit of extra work so we add in these tags to ensure the browser can validate the file from its cache easily.

**Compression**

Now I want to talk about compression. Browsers are able to unpack gzip files and read what is within and serve them to the user. Compression can have a huge effect on the speed of your site. I have seen my requests go from 27 down to 3 purely because I have been able to not just gzip the file down to be a few kb's instead of 100's of kb's but you can also combine files in to one gzip. I wont go too much in to how this is done but once again just remember this is awesome for our user experience. Compression is used across nearly every site out there to improve performance and it is definitely something we need to enable to ensure our Raspberry Pi is not bogged down by a number of large files having to be transmitted to the user.

To enable gzip compression we need to carry out the following.

Firstly lets jump back in to our lighttpd.conf file.


    
    sudo nano /etc/lighttpd/lighttpd.conf



From here you want to uncomment or add the line "mod_compress". Below is my setup, once again pay attention to the order of the modules!


    
    
    server.modules = (
            "mod_redirect",
            "mod_alias",
            "mod_access",
            "mod_fastcgi",
            "mod_compress",
            "mod_expire"
    
    )
    



You then need to add in some configuration settings. I will show you mine below and then go in to more detail about what they mean.


    
    
    # compression settings for gzip
    compress.cache-dir          = "/var/cache/lighttpd/compress/"
    compress.filetype           = ("text/xml","application/x-javascript", , "application/javascript", "text/javascript", "text/x-js", "text/css", "text/html", "text/plain", "image/png", "image/gif", "image/jpg", "image/svg+xml", "application/xml")
    



You may notice that I declare A LOT of difference filetypes I want to compress. You will see a lot of other tutorials out there stick with just css/javascript and html but Why? there are other resources out there that would benefit massively from compression. These include and by no means are limited to images,xml,plain text, cross domain scripts and much more. Anything you serve can be compressed and can be cached so lets do it! The only things I decided not to compress were my dynamic php files these can be compressed within php itself which I will explain in a moment.

Once you have added the above settings in to your lighttpd.conf we will now need to ensure a compression directory is available as per the compress.cache-dir declaration.

Carry out the following commands to ensure everything is available.

    
    sudo mkdir /var/cache/lighttpd/compress/
    sudo chown www-data:www-data /var/cache/lighttpd/compress/



We can now move on to PHP compression. Open up your php.ini file as per below.

    
     sudo nano /etc/php5/cgi/php.ini



You want to look for the line zlib.output_compression and set it to On as shown below.

    
    zlib.output_compression = On 



Finally we can restart lighttpd and all is set!


    
    sudo service lighttpd restart



**Important Reference Notes**

It is very important to realise with Lighttpd the declaration order of your modules has to be in the correct order. So please find below a list which is set out in the order you need. Whilst you will not need to enable every module I will say that it is probably best to copy and paste the code in to your lighttpd config file and then just comment out anything you do not need to enable.This way if you need to add in further modules to your config in the future you simply need to uncomment them and not have to keep looking up the correct order.



    
    
    server.modules = (
    "mod_rewrite",
    "mod_redirect",
    "mod_alias",
    "mod_access",
    "mod_auth",
    "mod_status",
    "mod_simple_vhost",
    "mod_evhost",
    "mod_userdir",
    "mod_secdownload",
    "mod_fastcgi",
    "mod_proxy",
    "mod_cgi",
    "mod_ssi",
    "mod_compress",
    "mod_usertrack",
    "mod_expire",
    "mod_rrdtool",
    "mod_accesslog" )
    



The execution of the above items is in reverse order. So starting from the bottom module you lighttpd will start enabling and executing. This is why there is such an important on of the order of the list.
