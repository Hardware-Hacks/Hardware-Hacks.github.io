---
author: munkee
comments: true
date: 2014-01-30 19:49:21+00:00
layout: post
slug: raspberry-pi-ghost-blog-slow-login-sign-up-fix
title: Raspberry Pi Ghost Blog Slow Login & Sign Up Fix
wordpress_id: 488
categories:
- Fixes
- Raspberry PI
tags:
- Ghost
- NPM
- Raspberry PI
---

For anyone that has installed [ghost blogging platform](http://www.ghost.org) on their Raspberry Pi will know that the login and sign up screens take minutes to load up. I will explain briefly the very quick fix that can be put in place to make the login and sign up instant as the rest of the blog is!

The root cause of the problem is the algorithm used to hash up the passwords. Infact, the slowness is actually a feature of the algorithm used to make the password guessing take unfeasible amounts of time. The algorithm is fine for a normal computer to run through and log you in to Ghost but for the Pi it is asking a little too much.

So with that all being said here is the fix!

Firstly SSH in to your pi and get in to your ghost root folder. For me this resides in my home directory.


    
    cd ~/ghost/



Next we grab a library which will replace our "slow" algorithm.


    
    sudo npm install bcrypt



The output will be along the lines of:


    
    npm http GET https://registry.npmjs.org/bcrypt
    npm http 200 https://registry.npmjs.org/bcrypt
    npm http GET https://registry.npmjs.org/bcrypt/-/bcrypt-0.7.7.tgz
    npm http 200 https://registry.npmjs.org/bcrypt/-/bcrypt-0.7.7.tgz
    npm http GET https://registry.npmjs.org/bindings/1.0.0
    npm http 200 https://registry.npmjs.org/bindings/1.0.0
    npm http GET https://registry.npmjs.org/bindings/-/bindings-1.0.0.tgz
    npm http 200 https://registry.npmjs.org/bindings/-/bindings-1.0.0.tgz
    
    > bcrypt@0.7.7 install /home/pi/ghost/node_modules/bcrypt
    > node-gyp rebuild
    
    gyp WARN EACCES user "root" does not have permission to access the dev dir "/root/.node-gyp/0.10.24"
    gyp WARN EACCES attempting to reinstall using temporary dev dir "/home/pi/ghost/node_modules/bcrypt/.node-gyp"
    gyp http GET http://nodejs.org/dist/v0.10.24/node-v0.10.24.tar.gz
    gyp http 200 http://nodejs.org/dist/v0.10.24/node-v0.10.24.tar.gz
    make: Entering directory `/home/pi/ghost/node_modules/bcrypt/build'
      CXX(target) Release/obj.target/bcrypt_lib/src/blowfish.o
      CXX(target) Release/obj.target/bcrypt_lib/src/bcrypt.o
      CXX(target) Release/obj.target/bcrypt_lib/src/bcrypt_node.o
      SOLINK_MODULE(target) Release/obj.target/bcrypt_lib.node
      SOLINK_MODULE(target) Release/obj.target/bcrypt_lib.node: Finished
      COPY Release/bcrypt_lib.node
    make: Leaving directory `/home/pi/ghost/node_modules/bcrypt/build'
    bcrypt@0.7.7 node_modules/bcrypt
    └── bindings@1.0.0



Next we replace the old library with the new one we have just installed.


    
    sudo nano core/server/models/user.js



You want to look for the line that contains the require  statement below:


    
    bcrypt = require('bcryptjs')



and replace with

    
    bcrypt = require('bcrypt')



Once this is done hit Ctrl + X then Y and restart ghost.

You should now try and login and everything should work much quicker!

Hope this helps, any issues just let me know in the comments section.
