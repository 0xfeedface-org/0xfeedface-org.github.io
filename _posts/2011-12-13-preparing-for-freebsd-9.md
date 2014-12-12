---
layout: post
title: Preparing for FreeBSD 9
created: 1323821370
---
My employer, <a href="http://www.wayfair.com/" target="_blank">Wayfair</a>, uses FreeBSD quite heavily. I like to mirror my professional environment at home. At work, I use a FreeBSD VM in which I do all my development. I run OpenIndiana at home. I've decided to go back to FreeBSD on my OpenIndiana server. I've had some issues in the past regarding ZFS on FreeBSD, but I was running some pretty hacked-up patches. I'm willing to give it another try. I'm backing up all my data right now and I plan to do the switch sometime during the week.

In the meantime, I'm going to set up a VM on my laptop that mirrors what I want my server to be like.

There are a couple personal items I'd like to focus on in FreeBSD:
<ul>
<li>Recursive functionality in `setfacl`</li>
<li>Enhance `chmod` to use the A+, A=, and A- syntax from Solaris</li>
<li>Enhance `ls` to show ACLs</li>
</ul>

I might be writing my own NFSv4 ACL tools. If I do, I'll be glad to put the code up on GitHub.

As a side note, I'm mostly on-track for releasing libhijack 0.6 by the end of the year. I've hit one major snag which might put me back into January or even February 2012. Timing also depends on how busy I am with Christmas this year.
