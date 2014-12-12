---
layout: post
title: Thotcon and BSDCan
created: 1366892147
---
I'm excited to announce that I am speaking at <a href="http://thotcon.org/" target="_blank">Thotcon</a> and <a href="http://bsdcan.org/" target="_blank">BSDCan</a> about libhijack. In the last few weeks, I've fixed a number of bugs. I will be releasing a new minor version of libhijack after the BSDCan presentation.. Its bug fixes include proper <code>ptrace</code> handling on FreeBSD, lots of example code for FreeBSD, and the ability to resolve symbols in FreeBSD's RTLD. This last point is especially crucial because <code>dlopen</code> in FreeBSD's libc is a stub function that simply returns an error. The real <code>dlopen</code> is implemented entirely in the RTLD. When your program calls <code>dlopen</code>, the call gets resolved to the RTLD's version. Thus attempting to resolve <code>dlopen</code> externally via <code>ptrace</code> results in an error.
