---
author: munkee
comments: true
date: 2013-06-16 18:10:07+00:00
layout: post
slug: raspberry-pi-mono-mvc-asp-net-xsp-fastcgi-lighttpd-implementation
title: Raspberry Pi Mono MVC Razor View Engine ASP.net XSP / FastCgi / Lighttpd Implementation
wordpress_id: 296
categories:
- Raspberry PI
- Tutorials
tags:
- C#
- fast_cgi
- lighttpd
- Mono
- MVC
- Projects
- Raspberry PI
- Razor
- Server
---

This tutorial will run you through creating an Asp.net website written in C# utilising MVC3 on your Raspberry Pi via Mono. Why would you want to do this? well there are many reasons but some of us enjoy the .net platform and the wealth of libraries it allows us to tap in to that some other languages out there struggle with.

To give you an outline of what asp.net is take a look at the official website [here](http://www.asp.net). Essentially though asp.net allows us to code in a few different languages such as C#, F# VB etc and it compiles all of this in to a standard format for serving to our clients. There are a mass of benefits that this has but I wont get in to that here either, lets just assume you know why you want to do this and/or you just want to experiment with what the Raspberry pi can do.

As stated this tutorial will be aiming for a C# MVC 3 implementation and here is an example of what our outcome will be: [MVC 3 Raspberry Pi Mono Website Utilising Razor Views on Lighttpd](http://82.46.242.96:81/mono/)

In order to run Asp.net applications on the Raspberry Pi we need to use a great compiler called Mono. I have talked about Mono before in some previous posts so for now I will just [point you](http://c-mobberley.com/wordpress/index.php/2013/04/22/raspberry-pi-c-mono-hello-world-setup-installation-compile-gmcs-execution/) towards [those posts](http://c-mobberley.com/wordpress/index.php/2013/04/24/raspberry-pi-c-mono-mysql-connector-installation/) but to cut a long story short mono is what allows us to move away from asp.net just being a windows centric platform to being able to run on pretty much anything.

I will present you with the option of running mono under fast_cgi and also as a standalone web server which is called XSP. Firstly though there are some issues I need to talk about and this is probably why you have found this blog post anyway:




	
  * As of today (16/06/2013) mono is still running as version 2.10 for Raspberry Pi Debian

	
  * Mono still has issues with DateTime running under the Hard Float flavours of linux os

	
  * Entity Framework is unsupported for anything running mono below version 2.11



The above information is very important to understand and you will have to bear with me during this tutorial because I will be telling you to do a lot of stuff that does not seem "right" to get around the bugs in mono 2.10. There is work being carried out on version 2.11 mono but this seems to be pushed toward ARM v7 and ofcourse the Raspberry is at ARM v6. So taking the above info into account you will not be able to use the Entity Framework until mono 2.11 becomes available and unless you re-image to a soft float linux os you are going to hit a whole heap of issues with DateTime which WILL make your Asp.net app useless. For further information on this issue take a look at the [Raspberry Pi Wiki](http://elinux.org/CSharp_on_RPi).


I am going to break the tutorial down into a number of sections so you can skip through to the relevant areas if you have already completed certain parts.



	

  1. Flashing Debian Wheezy (Soft Float Point)

	
  2. Installing Lighttpd & Fast Cgi

	
  3. Installing Mono (XSP & Fast Cgi Mono Server)

	
  4. Configuring Mono Fast Cgi

	
  5. Creating an MVC 3 Site

	
  6. Running your site under under XSP

	
  7. Running your site under fast cgi mono server

	
  8. Common Errors & How to Debug!




**1. Flashing Soft Float Debian Wheezy**

I have to say out of all of the steps in this tutorial this is the one which hurt me most. Mainly because I have been running Rasbian  Wheezy (Hard float) for some time now and I had so many things setup that I effectively had to delete. Anyway I will leave the sob story for another day.. here is how to flash the new image.

Firstly download the latest version of Debian Wheezy from the official Raspberry Pi website. Here is a [direct link](http://www.raspberrypi.org/downloads) to the site and also a [direct link](http://downloads.raspberrypi.org/download.php?file=/images/debian/7/2013-05-29-wheezy-armel/2013-05-29-wheezy-armel.zip) to the download.

In order to flash using the image using my windows machine I downloaded Disk Imager from [here](http://sourceforge.net/projects/win32diskimager/).

Our next step is to unzip the downloaded wheezy file so we can get to the .IMG file.

Next wack in your SD card and load up Disk Imager you should see a small folder icon on the right hand side which you can click to select your .img file. Then use the drop down to select your SD card drive. It is worth checking which this is using My Computer to ensure you don't overwrite another drive you may have plugged in.

All that is left to do is to select "Write". This will begin the writing process and the progress bar will keep you nicely updated with what is going on.

Once the image has been written successfully eject the SD card and it into your Pi. Set everything up exactly as you would have done when you turned it on for the very first time.

You will now be running a Raspberry Debian flavour distro utilising the soft floating point which makes mono and DateTime very happy!

**2.Installing Lighttpd & Fast Cgi**

I have chosen Lighttpd numerous times in the past for its simplicity to setup and I have to say when getting in to setting up fast cgi mono server it takes out so much of the hassle that I have seen people facing when trying to use mono alongside apache and nginx. [Here](http://c-mobberley.com/wordpress/index.php/2013/05/04/site-re-installation-and-my-raspberry-pi-web-server-setup/) are a few [links](http://c-mobberley.com/wordpress/index.php/2013/05/18/raspberry-pi-lighttpd-optimisation-for-wordpress-owncloud-and-more/) as to why I love Lighttpd.

Now lets jump in to the installation.

First get yourself up to date as it is quite likely if you followed step 1 there will be a mass of libraries that need updating or upgrading.


    
    
    pi@raspberrypi ~ $ sudo apt-get upgrade
    pi@raspberrypi ~ $ sudo apt-get update
    



Once all of that has finished we grab Lighttpd:


    
    
    pi@raspberrypi ~ $ sudo apt-get install lighttpd
    



By default Lighttpd will use the path "/var/www/" and serve from port 80. You can change any of these settings though using the Lighttpd.conf file which is located at "/etc/lighttpd/lighttpd.conf".

To check whether you have lighttpd setup correctly hit your Pi's IP address (internal is easiest 192.168...etc) and you should be presented with the lighttpd initial welcome screen which looks like [this](http://82.46.242.96:81/index.lighttpd.html).

We will now install fast_cgi support on our lighttpd webserver. I have explained previously what fast_cgi is so why not have a quick read up on their [official website](http://www.fastcgi.com/drupal/node/2). Jump into your lighttpd.conf file as shown below:


    
    
    pi@raspberrypi ~ $ sudo nano /etc/lighttpd/lighttpd.conf
    



Once you have the configuration file opened you will want to add mod_fastcgi to the top of the file to go with any other modules that are declared. For ease here is the list of my headers, you may not have all of these enabled so don't worry about the content but pay attention to the order they are declared in. Ensure you fastcgi declaration is around about the same sort of area that mine is declared for instance before mod_compress and after mod_access.



    
    
    server.modules = (
            "mod_redirect",
            "mod_alias",
            "mod_access",
            "mod_fastcgi",
            "mod_compress",
            "mod_expire"
    
    )
    



Once you have the server.modules sorted out hit Ctrl+X and then Y to save changes and close the configuration file. I will continue the configuration of fastcgi after we move on to installing mono and the mono-fastcgi-server!

**3. Installing Mono (XSP & Fast Cgi Mono Server)
**
This is really easy and can be done with the following line:

    
    
    pi@raspberrypi ~ $ sudo apt-get install mono-complete mono-fastcgi-server4 mono-xsp4
    



mono-complete will download all of the mono dependencies, libraries and other scripting information that we may hit with some of our applications.

mono-fastcgi-server4 is our Asp.net version 4.0 support for fastcgi. It extends the fastcgi methodology into asp.net and allows us to serve files without having to run a standalone server, we can utilise our lighttpd web server or our apache server etc.

mono-xsp4 is a standalone server implementation of Asp.net version 4.0. I mainly use this to help with debugging issues from the command line when I SSH in to my Pi. If you are hitting errors with your setup and the custom server errors that we see Asp.net spew out arent detailed enough then more often than not the same errors but in more detail spew out of the XSP server too.. very useful for bug killing!

**4. Configuring Mono Fast Cgi**

Now that we have all of the mono dependencies and fastcgi support lets continue with our lighttpd configuration, we want to enable the serving of Asp.net pages!

Firstly there is already a "how to" on setting up mono-fastcgi-server with lighttpd on the official mono website. However as usual there are a few things in there that become confusing when you read so I have gone through all of the pain of fixing the confusion and errors to give you the following setup:


    
    
    #uncomment below for aspx
    $HTTP["url"] =~ "^/mono/"{ fastcgi.server = (
                    "" => ((
    #To be added
    "socket" => "/tmp/fastcgi-mono-server4",
    "bin-path" => "/usr/bin/fastcgi-mono-server4",
    "bin-environment" => (
    "PATH" => "/bin:/usr/bin",
    "LD_LIBRARY_PATH" => "/usr/lib:",
    "MONO_SHARED_DIR" => "/tmp/",
    "MONO_FCGI_LOGLEVELS" => "Error",
    "MONO_FCGI_LOGFILE" => "/tmp/fastcgi.log",
    "MONO_FCGI_ROOT" => server.document-root,
    "MONO_FCGI_APPLICATIONS" => "/mono/:/var/www/mono/" ),
    "max-procs" =>4,
    "check-local" => "disable"
                            )) )
    }
    



The above configuration is the EXACT configuration I am using the run the example MVC site I linked to in my first few paragraphs of this blog. The mono website talks about having a separate configuration file and all of this other nonsense. I do not like that method because I hate having to go through multiple files just to figure out what configuration I need to change and where.

So to get the above code in to your setup carry out the following steps.

Create a directory called "mono" within our /var/www/ server home directory.


    
    pi@raspberrypi ~ $ sudo mkdir /var/www/mono
    
    



This will mean that we will be hosting our MVC site from http://ipaddress/mono/

To enable accessibility which I will go in to even further during out site setup I will set owner and chmod with the following commands.


    
    
    pi@raspberrypi ~ $ sudo chown www-data:www-data /var/www/mono
    pi@raspberrypi ~ $ sudo chmod 777 /var/www/mono
    



The chown allows our webserver to access the directory, the chmod will be important later when we want to push files to the directory.

You can now jump into your lighttpd configuration file to add in the fastcgi server code.

    
    
    pi@raspberrypi ~ $ sudo nano /etc/lighttpd/lighttpd.conf
    



Once the file is opened copy and past the following items at the bottom of the file.


    
    
    $HTTP["url"] =~ "^/mono/"{ fastcgi.server = (
                    "" => ((
    #To be added
    "socket" => "/tmp/fastcgi-mono-server4",
    "bin-path" => "/usr/bin/fastcgi-mono-server4",
    "bin-environment" => (
    "PATH" => "/bin:/usr/bin",
    "LD_LIBRARY_PATH" => "/usr/lib:",
    "MONO_SHARED_DIR" => "/tmp/",
    "MONO_FCGI_LOGLEVELS" => "Error",
    "MONO_FCGI_LOGFILE" => "/tmp/fastcgi.log",
    "MONO_FCGI_ROOT" => server.document-root,
    "MONO_FCGI_APPLICATIONS" => "/mono/:/var/www/mono/" ),
    "max-procs" =>4,
    "check-local" => "disable"
                            )) )
    }
    
    # Add index.aspx and default.aspx to the list of files to check when a directory is requested.
    index-file.names += ( "index.aspx", "default.aspx", "index.cshtml", "default.cshtml" )
    fastcgi.map-extensions = (
            ".asmx" => ".aspx",
            ".ashx" => ".aspx",
            ".asax" => ".aspx",
            ".ascx" => ".aspx",
            ".soap" => ".aspx",
            ".rem" => ".aspx",
            ".axd" => ".aspx",
            ".cs" => ".aspx",
            ".config" => ".aspx",
            ".dll" => ".aspx" )
    
    



The official mono website tries to shirk away from the fastcgi.map-extensions method and to be honest we arent actually using this method to serve files but I have added it in due to some strange spurious errors which I kept getting when trying to initialise an mvc site and this mapping actually made the code run. So as stated earlier adding this in does not feel "right" but it fixes some of the random errors you may get.

From here you can close the lighttpd.conf file and restart the server. Hit CTRL+X to close the file and Y to save changes. The following line will restart lighttpd.


    
    
    pi@raspberrypi ~ $ sudo service lighttpd restart
    [ ok ] Stopping web server: lighttpd.
    [ ok ] Starting web server: lighttpd.
    




**5. Creating an MVC 3 Site**

The creation of an MVC site can be done in many ways. Some will use Visual studio, some will use notepad and some will use Xamarin Studio. Out of all of these I have found Visual Studio the best to build with. I use the Ultimate edition at work but you can grab a copy of Visual Studio 2010 Web Express from the net for free and it will give you all of the needed functionality to get a site going.

In the rest of this section I will show you what is needed to produce a basic site using Visual Studio.




	
  1. Open Visual Studio

	
  2. File -> New Project

	
  3. ASP.net MVC 3 Web Application

	
  4. Intranet Application with Razor View Engine



From here you should have a setup that looks like the following in your solution explorer:
[![solution](http://res.cloudinary.com/raspberry-pi-awesome/image/upload/c_crop,h_158,w_158,x_0,y_40/h_150,w_150/v1390936734/solution_bbay8p.png)](http://192.168.0.15/wordpress/wp-content/uploads/2013/06/solution.png)

There are now a few items we will need to delete out to ensure we do not get errors when we try to run the program under mono. This is purely because as stated earlier we are running mono v2.10 on Debian Wheezy. If/When the newer versions of mono come out these steps will need to be evaluated. However I will leave that up to you to do with the knowledge you have gained here.

First off lets delete out the reference to the Entity Framework.




	
  1. Select references

	
  2. Right click EntityFramework

	
  3. Select Remove



Mono supports a number of Authentication methods but for v2.10 I believe this is limited to None or Forms. For our test site we will remove all authentication as it relies on SQLServerExpress and our Raspberry will not be running SQL server. Carry out the following steps to fix:

-- Double click web.config file
	-Locate 
    
    <connectionStrings>

and delete out the content between 
    
    <connectionStrings>

and 
    
    </connectionStrings>


	-Locate `<authentication mode="Windows" />` and replace with  
    
    <authentication mode="None" />


	-Locate 
    
    <authorization>

and replace 
    
    <deny users="?" />

with 
    
    <allow users="*" />


	-Locate: 
    
    
     <roleManager enabled="true" defaultProvider="AspNetWindowsTokenRoleProvider">
          <providers>
            <clear/>
            <add name="AspNetSqlRoleProvider" type="System.Web.Security.SqlRoleProvider" connectionStringName="ApplicationServices" applicationName="/" />
            <add name="AspNetWindowsTokenRoleProvider" type="System.Web.Security.WindowsTokenRoleProvider" applicationName="/" />
          </providers>
        </roleManager>
    

and delete it all.
	-Locate:

    
    
        <profile>
          <providers>
            <clear/>
            <add name="AspNetSqlProfileProvider" type="System.Web.Profile.SqlProfileProvider" connectionStringName="ApplicationServices" applicationName="/" />
          </providers>
        </profile>
    

and delete it all.
	-Save the file


Now we are near the end of our setup. Sometimes Visual Studio can place a file called global.asax in the wrong folder when using intranet applications. We will need to move this to ensure we get the correct routing. If you can see global.asax in your solution explorer as per the image below then all is ok. Otherwise it will be within your "Content" folder so just drag it out to the setup as shown in the image. All will be ok from here on in!

[![goodsetup](http://c-mobberley.com/wordpress/wp-content/uploads/2013/06/goodsetup-150x150.png)](http://c-mobberley.com/wordpress/wp-content/uploads/2013/06/goodsetup.png)

Our final step now we have just removed all authentication is to ensure that when we deploy our project it will have all of the required dependency libraries attached. Windows machines have these dependencies already but linux will not and mono may have some and not others!

Firstly we will use the add deployable dependencies tool.




	
  1. Right click solutuion name e.g. MvcApplication54 from screenshot earlier.

	
  2. Select Add Deployable Dependencies

	
  3. Select Asp.net MVC

	
  4. Click OK



You will then see a new folder created in your solution called _bin_deployableAssemblies. These are the references that will fill in any gaps mono has.

Now there is one dependency that is included called "Microsoft.Web.Infrastructure.dll". I have read a number of posts where it is mentioned that this "breaks" mono. However in my experience I have had very limited problems with this library if any at all. Sometimes I get an error such as "No Access Key" which seems to trace to this library but then other times I get nothing. It is your choice if you want to remove this but I would say leave it in for now until you see that it is causing problems, then you have the option to remove later on.

From here on in it is up to you how you want to deploy the solution. You can select to use the deployment wizard within visual studio if you set up a FTP server on your Pi and ensure the /var/www/mono dir is included or you can just copy all of the files from your solution folder and put them on to the Pi in to /var/wwww/mono directory manually. I will allow you to choose your own method here.

Whatever method you pick it is important to ensure all files are executable and also readable. So always run the following on your web directory:


    
    
    pi@raspberrypi ~ $ sudo chown www-data:www-data  /var/www/mono/ -R
    pi@raspberrypi ~ $ sudo chmod 755 /var/www/mono/ -R
    pi@raspberrypi ~ $ sudo service lighttpd restart
    





**6. Running your site under under XSP**

Before we run our site under lighttpd or if you wish to run your site without using lighttpd at all you can use the XSP stand alone server to debug any issues and also serve your site for you.

To do this, assuming your /var/www/mono server now contains all of the files from your MVC application you can simply carry out the following commands.

Firstly navigate to your folder.


    
    
    cd /var/www/mono
    



Then start the XSP server.


    
    
    pi@raspberrypi /var/www/mono $ xsp4
    xsp4
    Listening on address: 0.0.0.0
    Root directory: /var/www/test
    Listening on port: 8080 (non-secure)
    Hit Return to stop the server.
    



You can not navigate to http://ipaddress-of-pi:8080 and see your page running! It is worth noting it may take a while to load first time due to the compiling and the resources required by the xsp4 server.

Also I had a No Access Key error when hitting the address first time this is related to the microsoft.infrastructure file as I mentioned earlier. Just refresh the page and you should see your homepage loaded and/or delete the microsoft.infrastructure file from the bin folder of your MVC project "/var/www/mono/bin".
 
Stupidly simple  to do this though right? Well yes! but the xsp server uses a lot more resources than the lighttpd implementation. So once again I would use this for debugging and just developing with. For more of a production scenario I would go with the lighttpd fastcgi mono stream.

**7. Running your site under fast cgi mono server**

We have configured near enough everything there is to do previously in step 4. So you can go ahead and go to your web pages address: http://pi-ip-address/mono/

You should see the webpage loaded! If you get any errors then it is worth adding in to your web.config file the following:


    
    
    <customErrors mode="Off"/>
    



This line will allow you to debug your page with more detailed error reports. You can also use Step 6 to run the program under XSP4 which will output any errors to the command line for you. It also often offers less cryptic clues as to where problems may be!

A final note a lot of the errors can be because you make a change to try and fix something and you haven't restarted lighttpd. Always restart lighttpd when you make a change, even a directory access change as it can make a huge difference to the way the application runs under fastcgi.

**8. Common Errors & How to Debug!**

I want to share and document some common errors I have come up against so if you have similar issues you can jump to this section without trawling through the net trying to find out what might be the solution to your problem. These are in no order and will just be added to as I continue my tests.

8.1 Access is denied to directory or file blah blah.

I always run the following lines of code in order to allow deployment from visual studio and then to ensure executable and correct ownership of files. I finish up with a restart of the webserver to flush out anything that may have been cached with the old access permissions.


    
    
    pi@raspberrypi ~ $ sudo chmod 777 /var/www/mono/ -R
    --- Upload from visual studio is then carried out now --
    pi@raspberrypi ~ $ sudo chown www-data:www-data  /var/www/Project/ -R
    pi@raspberrypi ~ $ sudo chmod 755 /var/www/Project/ -R
    pi@raspberrypi ~ $ sudo service lighttpd restart
    [ ok ] Stopping web server: lighttpd.
    [ ok ] Starting web server: lighttpd.
    



8.2 No access to the given key


    
    
    No access to the given key
    Description: HTTP 500. Error processing request.
    Stack Trace:
    System.Security.SecurityException: No access to the given key 
    



I can only see from a stack trace that this is related to the Microsoft.Infrastructure library. More often than not you can simply refresh the page and this error goes away. Alternatively it is best to just remove the file from the bin directory of your project. In our case this would be /var/www/mono/bin/Microsoft.Web.Infrastructure.dll

8.3 Invalid IL Code System.Data.Objects


    
    
    Invalid IL code in System.Data.Objects.ObjectContext:.ctor (string,string): method body is empty.
    



You are trying to use the Entity Framework which as stated before is not supported at the moment for anything running sub Mono 2.11 and debian release is currently Mono 2.10.

You need to remove the Entity Framework references and find another method of modelling your objects such as NHibernate.
