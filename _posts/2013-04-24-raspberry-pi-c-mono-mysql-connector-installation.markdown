---
author: munkee
comments: true
date: 2013-04-24 07:46:32+00:00
layout: post
slug: raspberry-pi-c-mono-mysql-connector-installation
title: Raspberry Pi C# Mono MySql Connector Installation
wordpress_id: 99
categories:
- Raspberry PI
- Tutorials
tags:
- C#
- Database Connection
- Mono
- MySql
- Raspberry PI
---

Linking with my [previous post](http://c-mobberley.com/wordpress/index.php/2013/04/22/raspberry-pi-c-mono-hello-world-setup-installation-compile-gmcs-execution/) I want to talk about utilising c# mono and the mysql connector in order to allow our raspberry pi to connect to a local instance of a mysql database. This is useful is you prefer using asp.net and c# over python and you want to now start to build upon my last tutorial and create/read/update/delete (CRUD) data using your c# console application.

We will assume you have mono and c# up and running including the compiler gmcs and you are now just looking to connect to your database and read some data.  I initially wanted to test this as I want to read data from a temperature sensor on a breadboard and push the data in to a mysql database. I will then be able to read this data and output it to one of my webpages giving a live graphical view of the current temperature in the room where my pi is installed. The potential to produce projects such as these are never ending and being able to do this in a language such as c# and not just python/java etc opens even more doors to those who are used to using the language already or want to start learning.

First off in order to allow c# to connect to mysql databases you need to get a copy of the mysql connector. It can be found [here](http://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-5.2.3-noinstall.zip)

You need to unzip this and find the file called **MySql.Data.dll** then place it on to your pi in a folder where you can get to it easy. I downloaded it through my phone as wget kept on producing a 404 error for some reason. I then connected to my raspberry pi's shared usb hard drive and placed the file there.

We will now register the MySql Data Connerctor file using the gacutil.


    
    
    pi@raspberrypi ~ $ sudo gacutil -i /media/usbhdd/MySql.Data.dll
    Installed /media/usbhdd/MySql.Data.dll into the gac (/usr/lib/mono/gac)
    



As a check you can see what is inside the mono/gac however be warned you will get a huge list.. I have tried to point to the Mysql.Data entry below >>>> <<<<<.

    
    
    pi@raspberrypi ~ $ sudo ls -a /usr/lib/mono/gac
    




    
    
    . Mono.Cairo OpenSystem.C System.Runtime.DurableInstancing
    .. Mono.Cecil PEAPI System.Runtime.Remoting
    Accessibility Mono.Cecil.Mdb RabbitMQ.Client System.Runtime.Serialization
    Commons.Xml.Relaxng Mono.CodeContracts System System.Runtime.Serialization.Formatters.Soap
    cscompmgd Mono.CompilerServices.SymbolWriter System.ComponentModel.Composition System.Security
    CustomMarshalers Mono.CSharp System.ComponentModel.DataAnnotations System.ServiceModel
    fastcgi-mono-server2 Mono.Data.Sqlite System.Configuration System.ServiceModel.Discovery
    fastcgi-mono-server4 Mono.Data.Tds System.Configuration.Install System.ServiceModel.Routing
    I18N Mono.Debugger.Soft System.Core System.ServiceModel.Web
    I18N.CJK Mono.Http System.Data System.ServiceProcess
    I18N.MidEast Mono.Management System.Data.DataSetExtensions System.Transactions
    I18N.Other Mono.Messaging System.Data.Linq System.Web
    I18N.Rare Mono.Messaging.RabbitMQ System.Data.OracleClient System.Web.Abstractions
    I18N.West Mono.Posix System.Data.Services System.Web.ApplicationServices
    IBM.Data.DB2 Mono.Security System.Data.Services.Client System.Web.DynamicData
    ICSharpCode.SharpZipLib Mono.Simd System.Design System.Web.Extensions
    Microsoft.Build.Engine Mono.Tasklets System.DirectoryServices System.Web.Extensions.Design
    Microsoft.Build.Framework Mono.Web System.Drawing System.Web.Mvc
    Microsoft.Build.Tasks Mono.WebBrowser System.Drawing.Design System.Web.Routing
    Microsoft.Build.Tasks.v3.5 Mono.WebServer2 System.Dynamic System.Web.Services
    Microsoft.Build.Tasks.v4.0 >>>>>>>>>> MySql.Data <<<<<<<<<<<< System.EnterpriseServices System.Windows.Forms
    Microsoft.Build.Utilities Novell.Directory.Ldap System.IdentityModel System.Windows.Forms.DataVisualization
    Microsoft.Build.Utilities.v3.5 Npgsql System.IdentityModel.Selectors System.Xaml
    Microsoft.Build.Utilities.v4.0 nunit.core System.Management System.Xml
    Microsoft.CSharp nunit.core.interfaces System.Messaging System.Xml.Linq
    Microsoft.VisualC nunit.framework System.Net WebMatrix.Data
    Microsoft.Web.Infrastructure nunit.mocks System.Numerics WindowsBase
    Mono.C5 nunit.util System.Runtime.Caching xsp2
    



Now we know the registration has been successful we can build upon my last tutorial where we created a simple c# console application to print out a few lines of text.

Lets navigate to the director we created last time, if you do not have this then simply use mkdir and re-create the folders.


    
    
    pi@raspberrypi ~ $ cd /home/pi/Projects/HelloWorld
    pi@raspberrypi ~/Projects/HelloWorld $ ls -a
    . .. HelloWorld.cs HelloWorld.exe
    



You can see the two hello world files from last time, we will now create a new file called MySql that will be used to connect to the MySql database.


    
    
    pi@raspberrypi ~/Projects/HelloWorld $ sudo nano MySql.cs
    



The contents of this file can be copied and pasted as per below. But essentially this code will use the mysql connector client to connect to our mysql database and query it for its version number. This is very basic but it will give you an idea of basic READ functionality.


    
    
    using System;
    using MySql.Data.MySqlClient;
    
    public class Connecting_Example
    {
    
        static void Main()
        {
            string cs = @"server=localhost;userid=testuser;
                password=******;database=TestDB";
    
            MySqlConnection conn = null;
    
            try
            {
              conn = new MySqlConnection(cs);
              conn.Open();
              Console.WriteLine("MySQL version : {0}", conn.ServerVersion);
    
            } catch (MySqlException ex)
            {
              Console.WriteLine("Error: {0}",  ex.ToString());
    
            } finally
            {
              if (conn != null)
              {
                  conn.Close();
              }
            }
        }
    }
    
    



You will need to alter the lines:

    
    
     string cs = @"server=localhost;userid=testuser;
                password=******;database=TestDB";
    



I simply created a new database and added a user to it for testing. You can connect directly to a pre-existing database if you wish with the same username and password used to access it. If anyone is interested in how to set up mysql databases, users and new databases then let me know and it can be our next tutorial ;).

Now comes the hard part, which is often left unmentioned in other tutorials. Even though you have registered MySql.Data within mono you can sometimes get the issue where the c# file does not recognise the MySql.Data declaration as it has not been added as a reference.

To fix this issue we can pass a -r  in to our gmcs command during the compiling of the application.


    
    
    pi@raspberrypi ~/Projects/HelloWorld $ sudo gmcs -r /usr/lib/mono/gac/MySql.Data/5.2.3.0__c5687fc88969c44d/MySql.Data.dll MySql.cs
    



The pathway entered above is the default for the MySql.Connector plugin I linked to earlier. If you end up using an earlier or later version you will be able to find the path by running "ls -a /usr/lib/mono/gac/MySql.Data/" this will give you the version folder so you can hit the MySql.Data.Dll.

Now the referencing has been done we will have a MySql.exe file sat ready to run. We will now use mono to execute this file and hopefully output our mysql version.


    
    
    pi@raspberrypi ~/Projects/HelloWorld $ mono MySql.exe
    MySQL version : 5.5.30-1.1
    



There we have it, a console application that connects to our mysql database and checks for the version number, outputting it back to us at the end. Although this has been a very basic tutorial you now have the capability to connect to your database and further develop your c# application to be able to perform CRUD operations. You could read and write data from a sensor and then have this output to a webpage graphically for example!

Please leave any comments if you need further clarification or examples.

