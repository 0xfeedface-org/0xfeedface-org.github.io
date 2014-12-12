---
layout: post
title: Automated FreeBSD Nightly Builds
created: 1383604203
---
I've started doing nightly builds of my FreeBSD ASLR work. The nightly builds are FreeBSD 11-CURRENT/amd64. I also have a pkgng repo with quite a few commonly-used packages (packages that I use on a daily basis). I'm announcing today the public availability of the nightly builds and the pkgng repo. The build runs at 1AM Eastern every night and at the end, the status of the build steps are tweeted to my <a href="https://twitter.com/lattera" target="_blank">account</a>. I cannot guarantee the reliability, stability, or security of either the base system or the packages. Run them at your own risk. I run them on four different boxes.

The distribution sets and ISO images can be found <a href="http://releases.0xfeedface.org/pub/FreeBSD/snapshots/amd64/amd64/11.0-CURRENT/" target="_blank">here</a>. The pkgng repo can be found <a href="http://amd64.11-current.pkgbuild.0xfeedface.org/" target="_blank">here</a>. Please be nice to my limited bandwidth.
