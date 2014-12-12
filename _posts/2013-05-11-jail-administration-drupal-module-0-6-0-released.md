---
layout: post
title: Jail Administration Drupal Module 0.6.0 Released!
created: 1368291745
---
I'm excited to announce the 0.6.0 release of my FreeBSD jail administration Drupal module. This release includes two new features. I've created an experimental rc.d <a href="https://github.com/lattera/freebsd-rc-scripts/blob/master/jailadmin.sh" target="_blank">script</a> to autoboot your jails when your computer boots up. I've also added a Drush module so that you can use the command-line to interact with the jail administration project. You can start jails, stop jails, manage jail snapshots, create boot environments (BEs), activate/deactivate BEs, delete BEs, and view the status of jails. As always, you can follow the project on <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">GitHub</a>. I'm waiting for RedPorts approval (to test out my Ports tree changes) to submit a PR to FreeBSD to update the Ports tree. In the meantime, you can download it <a href="http://0xfeedface.org/~shawn/projects/jailadmin/jailadmin-7.x-0.6.0.tar.gz">here</a>.

What's been changed since version 0.5.2:
<ul>
<li>Status page shows DHCP-assigned IP address</li>
<li>Add autoboot support</li>
<li>Properly set jail hostname</li>
<li>Experimental per-jail boot environment support</li>
<li>New Drush module for command-line interaction and automation</li>
</ul>
