---
layout: post
title: The Future of libhijack
created: 1379034648
---
I was given the opportunity today to purchase a Raspberry Pi. I of course took that opportunity and I'm now a proud owner of a Model B. I've been thinking a lot about how best to use my spare time. I'm burnt out on trying to get shared object injection (without reliance on the RTLD) working. It's a very hard task, one that I simply can't seem to figure out. I've decided to let someone else finish that work. The project is under the stewardship of a talented hacking community, <a href="https://www.soldierx.com/" target="_blank">SoldierX</a>. I will be duplicating a modified version of this post over there.

With the insurgence of ARM-based devices into consumer electronics, my goal for libhijack is to port it over to ARM with initial support for Linux, then support for FreeBSD. Once that is done, I will retire from the project with the hopes that someone else will take the reigns. I would love to see two things: full shared object injection support and support for Android. Getting generic ARM support is the first step towards the latter goal. I will still act as the project maintainer and will gladly accept and merge in patches and pull requests (albeit with modifications if need be). As always, if you submit a patch or a pull request, you will be recognized and given proper credit, if desired.

To align my spare time with my career goals, I will be focusing on the goals I outlined in my <a href="/blog/lattera/2013-06-29/long-term-plans">long-term plans</a> blog post. The plans pertaining to libhijack still stand (finishing libhijack), but take on a slightly different meaning. I will consider the port to ARM as a finished product. I plan to also do extensive research into the ARM architecture and dive into Android malware analysis.
