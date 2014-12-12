---
layout: post
title: FreeBSD Ports Officially Migrated to SVN
created: 1342448793
---
FreeBSD has officially moved their ports tree over from CVS to SVN. That change renders my CVS->Git migration script useless (after a few weeks of use, I've found that the script is mainly useless anyways). I'll be recreating my <a href="https://github.com/lattera/freebsd-ports" target="_blank">freebsd-ports</a> repo from scratch using `git svn`. I still need to get my FreeBSD development server online and I'll start doing nightly builds of 9-stable and 10-current again along with nightly ports tree updates.

If you want to do `git svn clone` yourself, beware that it will take many hours.
