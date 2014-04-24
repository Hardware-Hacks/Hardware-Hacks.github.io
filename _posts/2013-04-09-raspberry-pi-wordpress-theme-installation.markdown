---
author: munkee
comments: true
date: 2013-04-09 21:02:12+00:00
layout: post
slug: raspberry-pi-wordpress-theme-installation
title: Raspberry Pi Wordpress Theme Installation FTP Setting Error
wordpress_id: 334
categories:
- Fixes
- Raspberry PI
tags:
- Raspberry PI
- Wordpress
---

Asked for FTP settings when trying to install a plugin or a theme? This is usually caused by the instance not being able to access with the correct permissions. The fix is simple!

    
    pi$ cd /var/www
    pi$ sudo chown -R www-data:www-data wordpress
