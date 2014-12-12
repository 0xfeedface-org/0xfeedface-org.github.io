---
layout: post
title: CVS to Git Migration of FreeBSD's Ports
created: 1332341210
---
As many of my readers know, I'm a Solaris and FreeBSD aficionado. Both operating systems bring many innovations to the table and make life easy for system administrators. Though FreeBSD switched to Subversion years ago for its code base (kernel and world), FreeBSD's Ports tree is still using CVS. In this article, I will discuss how to create your own Git repo of the FreeBSD Ports tree.

I'm going to keep an up-to-date Git repo hosted at <a href="https://github.com/lattera/freebsd-ports" target="_blank">GitHub</a>. The updating frequency will likely be at least once per day.

<strong>Requirements</strong>
<ul>
<li>csup (comes with all base installs of FreeBSD)</li>
<li>git</li>
<li><a href="http://tron.homeunix.org/cvscvt/" target="_blank">cvscvt</a> (copied to <code>/usr/local/bin</code>)</li>
</ul>

<strong>ZFS Setup</strong>
I'm a big fan of ZFS. We're going to create a few ZFS datasets:
<code>
[shawn@fbsd-sec ~]$ zfs get -r -oname,property,value name,mountpoint rpool/ports_cvs
NAME                     PROPERTY    VALUE
rpool/ports_cvs          name        rpool/ports_cvs
rpool/ports_cvs          mountpoint  /ports_cvs
rpool/ports_cvs/csup_db  name        rpool/ports_cvs/csup_db
rpool/ports_cvs/csup_db  mountpoint  /ports_cvs/csup_db
rpool/ports_cvs/cvsroot  name        rpool/ports_cvs/cvsroot
rpool/ports_cvs/cvsroot  mountpoint  /ports_cvs/cvsroot
rpool/ports_cvs/gitrepo  name        rpool/ports_cvs/gitrepo
rpool/ports_cvs/gitrepo  mountpoint  /ports_cvs/gitrepo
</code>

Give the user account (we'll use the account <code>shawn</code> in this article as an example) full access to each of these directories: <code>for dir in `find /ports_cvs -type d`; do setfacl -m user:shawn:full_set:fd:allow $dir; done</code>

<strong>Fetching the Ports Tree</strong>
FreeBSD uses a utility called csup to download from cvs the ports repo. You'll need to copy (and modify) the supfile found at <code>/usr/share/examples/cvsup/cvs-supfile</code> to <code>/ports_cvs/cvs-supfile</code>. I pasted my supfile as an example <a href="https://gist.github.com/2147398" target="_blank">here</a>. Change directory to <code>/ports_cvs</code> and run csup now and twiddle your thumbs for a couple hours: <code>cd /ports_cvs; csup -L2 cvs-supfile</code>

<strong>Converting to git</strong>
Since this is our first run, we need to make the git repo. Change directory to <code>/ports_cvs/gitrepo</code> and run <code>git init</code>: <code>cd /ports_cvs/gitrepo; git init</code>. Next we'll do the actual conversion from CVS to Git: <code>cd /ports_cvs; cvscvt -e freebsd.org -k FreeBSD /ports_cvs/cvsroot/ports | GIT_DIR=gitrepo/.git git fast-import</code>

After doing the conversion, you might have a dirty git checkout. Reset the checkout to HEAD: <code>cd /ports_cvs/gitrepo; git reset --hard HEAD</code>

<strong>Conclusion</strong>
You've now fully converted the Ports tree from CVS to Git! You can now treat the git checkout as a normal git Ports tree.

<strong>Extra Info</strong>
I've scripted doing all this so that I can run it as a nightly job. If you'd like to take a look at the script, take a look at my set of <a href="https://github.com/lattera/nightlies" target="_blank">nightly scripts</a>. Taking a look at the script will also help with understanding the commands I laid out above.
