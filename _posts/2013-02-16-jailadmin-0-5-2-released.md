---
layout: post
title: Jailadmin 0.5.2 Released
created: 1361024070
---
I'm proud to announce the 0.5.2 release of my FreeBSD jail administration Drupal module. This is a maintenance release that fixes a few bugs and enhances the user interface. This will be the last release prior to a major overhaul I'm working on. The tarball is <a href="http://0xfeedface.org/~shawn/projects/jailadmin/jailadmin-7.x-0.5.2.tar.gz">here</a> and the project lives on <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">GitHub</a>. A PR has been submitted to FreeBSD to update my www/drupal7-jailadmin port.

Here's what's changed between 0.5.1 and 0.5.2:
<ul>
<li>Properly bring up the loopback interface when starting the jail</li>
<li>Show Start/Stop, Snapshot, and Network Settings actions in the Status page</li>
<li>Add extra sanitization and security verification</li>
<li>Show the epair device and the virtual network it belongs to in the Status page</li>
<li>Make sure the IP isn't taken when assigning a static IP to epair or bridge devices</li>
</ul>

<a href="http://imgur.com/UrSBEIC" target="_blank">Here's</a> a nice screenshot showing the enhancements to the Status page.
