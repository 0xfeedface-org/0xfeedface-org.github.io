---
layout: post
title: Progress on my FreeBSD Jail Admin Drupal Module
created: 1368155067
---
I just committed and pushed to <a href="https://github.com/lattera/drupal-jailadmin/commit/11ec68bdb1fb245b4422ffbc114510ff7afdd2c2" target="_blank">GitHub</a> a Drush module. Having a Drush module will allow you to administer jails via the command-line rather than do everything via the web interface. Right now, you can start and stop jails. I've also added autoboot support so that you can boot them when your server is booting up during the normal rc.d process. I'll be adding an rc.d script soon. You should expect a new release (version 0.6.0) over the weekend.

Eventually, you will be able to do everything the web interface can do through the command-line. My main goal is to get support most of the commonly-performed tasks.
