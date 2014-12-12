---
layout: post
title: Boot Environments With FreeBSD Jails
created: 1365646995
---
One of the things I've wanted to do is implement boot environment (BE) support with FreeBSD jails. That way, I can not only take a snapshot of a jail prior to making experimental changes, I can also install new updates and perform those experimental changes in a completely safe environment. If something goes haywire, I just "reboot" the jail into the old boot environment. I've recently added experimental support in the dev branch of my jail administration Drupal module. The best part is that it doesn't require any database changes. The project can be used in legacy (straight up ZFS datasets) or with BEs.

When creating a new BE, the project will create a new ZFS dataset under [root dataset]/POOL/[new BE name]. The project tracks the current BE by setting a custom ZFS property (jailadmin:be_active) to true. When activating a BE, the project sets the custom ZFS property on the old BE to false, promotes the new BE, and sets the ZFS property to true on the new BE.

I'm debating creating a www/drupal7-jailadmin-devel port and pointing it to downloading a tarball right from GitHub. But for now, feel free to either clone the <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">project</a> or <a href="https://github.com/lattera/drupal-jailadmin/archive/dev.tar.gz">download</a> the latest tarball of the dev branch.

I'm currently eating my own dogfood. After a trial period of around a month, I plan to merge the dev into the master branch and tag a new release. I'm still working (slowly) on a rewrite. There will be plenty of treats in the rewrite, but it will take a while.

<a href="http://imgur.com/UuDn5tj" target="_blank">Here's</a> a screenshot to wet your appetite.
