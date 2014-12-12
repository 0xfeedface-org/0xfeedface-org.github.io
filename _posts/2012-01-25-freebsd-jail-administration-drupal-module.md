---
layout: post
title: FreeBSD Jail Administration Drupal Module
created: 1327509304
---
I make heavy use of vnet jails in FreeBSD at work. Due to FreeBSD's rc.d system not supporting vnet jails well, I opted to create a little command-line PHP project to make administration of jails easier. Though the tool was useful, it lacked in flexibility and usability. After learning how to write modules for Drupal, I opted to rewrite my jail administration project as a Drupal module. Though I wouldn't recommend running this project in a production environment, it's very useful for development environments. I use different jails to keep different development projects organized. For example, I have a www-dev jail for my web development projects and a freebsd-dev jail for working on the FreeBSD userland.

The code is hosted at <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">GitHub</a>. Even though the project is still young, it is very usable. I've already tagged version 0.1. It should support IPv6 out-of-the-box. If you have any issues with IPv6, let me know. Patches will also be gladly accepted (preferably via pull requests).
