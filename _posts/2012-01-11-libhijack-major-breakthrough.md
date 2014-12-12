---
layout: post
title: Libhijack Major Breakthrough
created: 1326298108
---
For the past month, I've been working on fixing just a single bug in the FreeBSD port of libhijack. I finally fixed the bug last night! We're getting closer to a new release of libhijack, which will support 32bit and 64bit Linux and 64bit FreeBSD. I'm going to leave out 32bit support on FreeBSD simply because no one uses 32bit FreeBSD anymore. If anyone would like to add 32bit FreeBSD support, I'll gladly accept patches (assuming the code looks good). There's still at least a month's work left to do.
<!--break-->

What's left to do:
<ul>
<li>Update the uncached function resolution API to use the new FreeBSD backend</li>
<li>Figure out how to call the mmap syscall reliably just as I do on Linux</li>
<li>Write sample shellcode to load an evil .so and hijack PLT/GOT</li>
<li>Update the InjectShellcode API to use the new FreeBSD backend</li>
</ul>

I'm really excited for this next release. It proves that libhijack can be ported to other operating systems that use ptrace and ELF. You will have a unified API for hijacking dynamically-loaded functions in FreeBSD and Linux.

A teaser of what's to come: <a href="http://i.imgur.com/4LjMJ.png" target="_blank">Screenshot</a>.
