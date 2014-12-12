---
layout: post
title: FreeBSD Jail Administration Project Updates
created: 1328817044
---
I've been working hard adding features and refining my FreeBSD vnet jail administration Drupal 7 module. I recently released version 0.2 of the project. It now supports using vnet to bridge directly to the physical NIC. This is an exciting release and I now consider the project usable. I will be releasing version 0.2.1 within the next day or two, which includes a few bug fixes and enhancements to existing features.

Here's the full changelog (major features in bold):
<ul>
<li><strong>Add ability to snapshot jail and upgrade jail's world (world upgrade still experimental)</strong></li>
<li><strong>Fix bug when selecting network. Add support for adding physical devices to bridges. Add support for bridges without IPs.</strong></li>
<li><strong>Support network-less jails</strong></li>
<li><strong>Make sure bridge and epair devices and IPs are unique in the UI layer</strong></li>
<li>Display network status</li>
<li>Add ability to set up ssh</li>
<li>Add sshd to rc.conf</li>
<li>Add README</li>
<li>Add ability to persist basic jail settings in jail config page</li>
<li>Properly bring up the bridge</li>
<li>Use quotation marks when configuring the epair device</li>
<li>Delete network services and mounts upon deleting a jail</li>
<li>Phase 1 of adding security</li>
<li>Add sanity checking</li>
<li>Add quotations around commands that use the jail's name</li>
</ul>
