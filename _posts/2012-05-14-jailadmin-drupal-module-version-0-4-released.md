---
layout: post
title: Jailadmin Drupal Module Version 0.4 Released!
created: 1337051945
---
Today is a great day for releasing a new version of my Jailadmin Drupal module. Version 0.4 is a milestone release. I'm really proud of this release and excited to see its usage. I've added support for IPv6, epair and bridge aliases, and custom routes. You can download the new version directly from <a href="https://github.com/downloads/lattera/drupal-jailadmin/jailadmin-7.x-0.4.tar.gz">GitHub</a>. If you're interested in helping out, please fork the <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">project</a> and send me either patch files or pull requests.

You will notice that setting default routes is different. Instead of setting the default route in the UI, you add a new custom route with the source set as "default" (without the quotes, of course). Upon upgrading the module, your IP assignments will stay the same along with the default route. The module automatically converts the existing database structures to the new structures and copies the data over. You will want to just double-check that everything is fine. Definitely backup your database before upgrading.

If you find any bugs, please let me know.
