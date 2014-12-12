---
layout: post
title: Jailadmin 0.5 Released!
created: 1354728760
---
I'm proud to announce the 0.5 release of my FreeBSD jail administration Drupal module, jailadmin. Notable changes this release include:
<ul>
<li>DHCP support for network devices</li>
<li>Rudimentary IPv6 support</li>
<li>Full rc.d support (run the <code>/etc/rc</code> script at boot)</li>
<li>Removal of Wayfair branding</li>
</ul>

Download is <a href="http://0xfeedface.org/~shawn/projects/jailadmin/jailadmin-0.5.tar.gz">here</a>. Development is done on <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">GitHub</a>.

Please note that even though IPv6 support is there, I've noticed some bugs with epair devices and IPv6. I've filed a bug report with FreeBSD about it.
