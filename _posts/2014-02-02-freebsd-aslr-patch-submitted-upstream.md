---
layout: post
title: FreeBSD ASLR Patch Submitted Upstream
created: 1391388421
---
Over the past few months, I've had the pleasure of enhancing Oliver's original patch that implements ASLR on FreeBSD. I've added support for randomizing the address of the RTLD and changing the behavior of ASLR to be set on a per-jail basis. This means that if a user requires an application that doesn't support ASLR (crashes, exhibits bugs, etc.), then the affected application can simply be placed in a jail with ASLR turned off. The rest of the system and the rest of the jails could still have ASLR turned on.

Oliver had ported over PaX's ASLR to NetBSD a few years back, and these patches brings FreeBSD feature-for-feature complete with NetBSD's ASLR implementation. What's lacking, along with NetBSD's implementation, is exec base randomization. This needs to be done on a per-binary basis, for binaries compiled with -fPIE. Additionally, we might want to specifically mark executables with an ELF note, specifying that it's safe to relocate the exec base.

One known bug is that applications compiled with clang with -fPIC -fPIE -static combined could segfault. I can provide a sample binary (with sample code) if needed for a simple five-line test application.

I will continue to research exec base randomization, but this task might be a bit over my head skill-wise.

I've submitted a <a href="http://www.bsdcan.org/2014/" target="_blank">BSDCan</a> presentation. I hope it will get accepted. I'll run through how Oliver and I have implemented ASLR on FreeBSD and how tightly it's integrated. My favorite feature is the per-jail ASLR configuration. I'm really excited for the future of this work. However, I need to take some time away from it and focus on some other projects for the next six to twelve months. If I make more progress on exec base randomization, you can follow my <a href="https://github.com/lattera/freebsd/tree/soldierx/lattera/aslr" target="_blank">GitHub repo</a>.

The PR on FreeBSD's bug reporting (or Problem Report, PR) is <a href="http://www.freebsd.org/cgi/query-pr.cgi?pr=kern/181497" target="_blank">here</a>.
