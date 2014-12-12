---
layout: post
title: Ongoing FreeBSD Work
created: 1325629542
---
FreeBSD 9.0 is nearing release--9.0-release is now tagged in version control! This is a major milestone release for FreeBSD with enhancements to many existing features. I keep an up-to-date git repo of FreeBSD 9-stable (updated nightly) at <a href="https://github.com/lattera/freebsd/tree/svn_stable_9" target="_blank">GitHub</a>. My git repo contains a few custom patches I've written the past little while. I will continue to keep the repository up-to-date on a nightly basis as best as I can (the nightly cron job is running in a FreeBSD VM I run at work).

My existing FreeBSD work will be released with 9.0: ZFS NFSv4 ACL sets. I'm preparing a second patch to submit upstream for recursive functionality in the <code>setfacl</code> command. I'm hoping any future patches I send will be merged into 9-stable. Any new work I do will be published to my GitHub repo first and pushed upstream when the time comes and if upstream approves.

I'm still working on libhijack on FreeBSD. I'm reading FreeBSD's RTLD, trying to figure out why certain bugs are occurring. I've made a few major breakthroughs and hope for a new release within the next two months.

If anyone has any suggestions for simple features or bug fixes they'd like me to work on, let me know.
