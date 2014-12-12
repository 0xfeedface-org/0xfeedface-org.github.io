---
layout: post
title: Libhijack Progress
created: 1333490146
---
After I release a new version of libhijack, I usually take a break for a month or two to let my mind settle. Since releasing version 0.6 of libhijack, I've been hard at work. I've been reading FreeBSD's RTLD to implement the same in libhijack. I've come a long way and have a good chunk of code. However, it seems the FreeBSD developers have done a great job at keeping the RTLD secure. I can't figure out how to hook into the RTLD. Because GNU's RTLD code is uglier than ugly, I've focused current development on FreeBSD/amd64. If I can't make progress within the next two months on FreeBSD, I will switch over to Linux and focus development there.

The next release will happen when I figure out how to hook into the RTLD and have the code for it. If you'd like to help, please take a look at the <a href="https://github.com/lattera/libhijack" target="_blank">project</a>. The RTLD code I've written is pretty messy, but that's simply because I've been toying around. Feel free to send me a pull request or a patch via email.
