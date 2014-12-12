---
layout: post
title: FreeBSD ASLR Progress
created: 1384616050
---
This morning, I just committed and pushed to <a href="https://github.com/lattera/freebsd/commit/f5b7bb7d5f9ab69586d6b7a3affee1d3778f9942" target="_blank">GitHub</a> my first major modification to Oliver Pinter's existing ASLR patches. In order to support legacy applications that won't work with ASLR, I've made it so that ASLR works on a per-jail basis. If you want global protection, you can have ASLR set in your global jail. You can stick the non-compatible application in a jail that has ASLR turned off. That way, you can provide security for everything but the applications that don't work with ASLR enabled. Additionally, if you are a jails-based hosting provider and your customer doesn't want ASLR, the customer can turn it off [him/her]self. My next goal is to get execbase randomization working. This will require a bit of additional research. Once execbase randomization is working, we'll have a technically complete ASLR implementation!
