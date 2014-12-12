---
layout: post
title: Nightly Builds of FreeBSD 9-STABLE
created: 1335385689
---
I now have my box set up to build nightly versions of FreeBSD 9-STABLE. I have it building a basic (~500 MB) ISO and memstick image. The ISO and memstick image are uploaded after the build completes to <a href="http://0xfeedface.org/~shawn/freebsd/nightlies/9-stable/" target="_blank">here</a>. The nightly build contains a few patches I've written and a custom kernel (don't worry, nothing malicious). If you use this, you'll have VIMAGE and IPFW support out-of-the-box along with recursive setfacl. I can't guarantee that a nightly build will always be successful (or usable) or that the ISO and memstick image will always be uploaded and/or kept. The processes are automatic and are at the mercy of bleeding-edge development on the 9-STABLE branch.
