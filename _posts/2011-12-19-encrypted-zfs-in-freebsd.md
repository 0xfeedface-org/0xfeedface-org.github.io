---
layout: post
title: Encrypted ZFS In FreeBSD
created: 1324313698
---
ZFS introduced encrypted datasets in zpool verison 30, which is only available in Solaris. FreeBSD and IllumOS use zpool version 28. For FreeBSD users, encrypted ZFS is still a possibility and is rather easy to do (I'm sure it is on IllumOS as well, I just haven't learned how to do it). In this article, we'll step through how to create an encrypted ZFS pool in FreeBSD.
<!--break-->
Our setup is pretty simple. We have a single pool named rpool. We'll create a number of datasets under rpool/encrypted:
<pre>
[root@fbsd-sec ~]# zfs create -omountpoint=/encrypted rpool/encrypted
[root@fbsd-sec ~]# zfs create rpool/encrypted/keys
[root@fbsd-sec ~]# zfs create -omountpoint=none rpool/encrypted/zvols
[root@fbsd-sec ~]# zfs create -ocompression=on rpool/encrypted/zvols/testvol
[root@fbsd-sec ~]# zfs create -V 1G rpool/encrypted/zvols/testvol/disk0
[root@fbsd-sec ~]# zfs create -V 1G rpool/encrypted/zvols/testvol/disk1
[root@fbsd-sec ~]# zfs create -V 1G rpool/encrypted/zvols/testvol/disk2
[root@fbsd-sec ~]# zfs create -V 1G rpool/encrypted/zvols/testvol/disk3
[root@fbsd-sec ~]# zfs create rpool/encrypted/keys/testvol
</pre>
Next, we'll create our encryption keys:
<pre>
[root@fbsd-sec ~]# dd if=/dev/random of=/encrypted/keys/testvol/disk0 bs=64 count=1
1+0 records in
1+0 records out
64 bytes transferred in 0.000094 secs (679583 bytes/sec)
[root@fbsd-sec ~]# dd if=/dev/random of=/encrypted/keys/testvol/disk1 bs=64 count=1
1+0 records in
1+0 records out
64 bytes transferred in 0.000161 secs (397682 bytes/sec)
[root@fbsd-sec ~]# dd if=/dev/random of=/encrypted/keys/testvol/disk2 bs=64 count=1
1+0 records in
1+0 records out
64 bytes transferred in 0.000094 secs (679583 bytes/sec)
[root@fbsd-sec ~]# dd if=/dev/random of=/encrypted/keys/testvol/disk3 bs=64 count=1
1+0 records in
1+0 records out
64 bytes transferred in 0.000095 secs (672771 bytes/sec)
</pre>
Now, we'll initialize geli for each of the zvols:
<pre>
[root@fbsd-sec ~]# geli init -s 4096 -K /encrypted/keys/testvol/disk0 /dev/zvol/rpool/encrypted/zvols/testvol/disk0
[...snip...]
[root@fbsd-sec ~]# geli init -s 4096 -K /encrypted/keys/testvol/disk1 /dev/zvol/rpool/encrypted/zvols/testvol/disk1
[...snip...]
[root@fbsd-sec ~]# geli init -s 4096 -K /encrypted/keys/testvol/disk2 /dev/zvol/rpool/encrypted/zvols/testvol/disk2
[...snip...]
[root@fbsd-sec ~]# geli init -s 4096 -K /encrypted/keys/testvol/disk3 /dev/zvol/rpool/encrypted/zvols/testvol/disk3
[...snip...]
</pre>
Finally, we'll attach the keys to the disks and create our encrypted ZFS pool:
<pre>
[root@fbsd-sec ~]# geli attach -k /encrypted/keys/testvol/disk0 /dev/zvol/rpool/encrypted/zvols/testvol/disk0
Enter passphrase:
[root@fbsd-sec ~]# geli attach -k /encrypted/keys/testvol/disk1 /dev/zvol/rpool/encrypted/zvols/testvol/disk1
Enter passphrase:
[root@fbsd-sec ~]# geli attach -k /encrypted/keys/testvol/disk2 /dev/zvol/rpool/encrypted/zvols/testvol/disk2
Enter passphrase:
[root@fbsd-sec ~]# geli attach -k /encrypted/keys/testvol/disk3 /dev/zvol/rpool/encrypted/zvols/testvol/disk3
Enter passphrase:
[root@fbsd-sec ~]# zpool create testvol raidz /dev/zvol/rpool/encrypted/zvols/testvol/disk0.eli /dev/zvol/rpool/encrypted/zvols/testvol/disk1.eli /dev/zvol/rpool/encrypted/zvols/testvol/disk2.eli /dev/zvol/rpool/encrypted/zvols/testvol/disk3.eli
[root@fbsd-sec ~]# zpool status testvol
  pool: testvol
 state: ONLINE
 scan: none requested
config:
	NAME                                              STATE     READ WRITE CKSUM
	testvol                                           ONLINE       0     0     0
	  raidz1-0                                        ONLINE       0     0     0
	    zvol/rpool/encrypted/zvols/testvol/disk0.eli  ONLINE       0     0     0
	    zvol/rpool/encrypted/zvols/testvol/disk1.eli  ONLINE       0     0     0
	    zvol/rpool/encrypted/zvols/testvol/disk2.eli  ONLINE       0     0     0
	    zvol/rpool/encrypted/zvols/testvol/disk3.eli  ONLINE       0     0     0
errors: No known data errors
</pre>

As you can see, creating an encrypted ZFS volume is easy on FreeBSD. Ideally, you might not want to store encryption keys in rpool, but rather a removable medium. I would likely create a zpool using a USB thumb drive and place the keys on there. I would then set the readonly ZFS property on the thumb drive zpool (zpool create enckeys /dev/device && [generate encryption keys] && zfs set readonly=on enckeys). You can always set readonly=off when you need to create, destroy, or modify stored encryption keys.
