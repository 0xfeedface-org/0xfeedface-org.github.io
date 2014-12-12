---
layout: post
title: Libhijack 0.6 Released!
created: 1329795834
---
I'm very proud to announce libhijack 0.6 today. Version 0.6 contains no new features. However, I have ported libhijack to FreeBSD/amd64. I do not plan to support 32bit on FreeBSD. However, if someone wants to add 32bit support, I will gladly accept patches. When I first started this project a few months ago, I thought that FreeBSD's runtime linker (aka RTLD, the executable that loads your program into memory) would have been implemented similar to GNU's runtime linker. It turns out I was very wrong. FreeBSD's RTLD is much different--and, in my opinion, better--than GNU's.

Every single API call works fully on FreeBSD. No API function signatures have been changed. Meaning, the same exact calls you would make on Linux would be the same exact calls on FreeBSD. This is no easy feat on a project like libhijack. Please note that since we're still not a 1.0 release, the API is still subject to change.

Though the APIs haven't changed, you will need one new compile-time define: <code>-DFreeBSD</code> for FreeBSD or <code>-DLinux</code> for Linux (retrieved by <code>uname -s</code>). If you're compiling libhijack on FreeBSD, you will need a populated /usr/src and you will need to tack on at compile-time: <code>-I/usr/src/libexec/rtld-elf -I/usr/src/libexec/rtld-elf/`uname -m`</code>. Please refer to <code>src/Makefile</code> if you need extra help. On FreeBSD, you will need to use <code>gmake</code> instead of BSD's <code>make</code>.

I need to do a bit of code cleanup in libhijack. Porting to FreeBSD caused me to write a lot of dirty hacks in libhijack's code. I will be releasing minor releases soon with cleaned up code. The code, as usual, can be found on <a href="https://github.com/lattera/libhijack" target="_blank">GitHub</a>.
