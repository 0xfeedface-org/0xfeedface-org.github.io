---
layout: post
title: Basic Overview of ZFS
created: 1362448513
---
ZFS is both a volume manager and a filesystem. Mixing hardware RAID and ZFS is generally frowned upon. ZFS is a Copy-On-Write (COW) filesystem. That means changes to files (even deletions) never modify existing live data on the device. Devices are grouped into pools, just as hardware RAIDs are set up. You can add whole disks or even partitions. Using whole disks is preferred since that enables better performance. Pools can be set up as JBOD, mirrors, or a special RAID called RAIDZ. RAIDZ is comparable to RAID-5 and RAID-6 combined. You can mix/match different types of setups in a given pool.

I generally have two pools: one pool containing two mirrored disks for my root partition and another pool containing three disks in a RAIDZ for all my data. Here's the output of <code>zpool status</code> which shows how my two pools are laid out:
<code>
shawn-vm-host[shawn]:~/ $ zpool status
  pool: rpool
 state: ONLINE
  scan: scrub repaired 0 in 1h52m with 0 errors on Thu Feb 21 12:25:28 2013
config:
	NAME           STATE     READ WRITE CKSUM
	rpool          ONLINE       0     0     0
	  mirror-0     ONLINE       0     0     0
	    gpt/disk0  ONLINE       0     0     0
	    gpt/disk1  ONLINE       0     0     0
errors: No known data errors
  pool: tank
 state: ONLINE
  scan: none requested
config:
	NAME        STATE     READ WRITE CKSUM
	tank        ONLINE       0     0     0
	  raidz1-0  ONLINE       0     0     0
	    da1     ONLINE       0     0     0
	    da2     ONLINE       0     0     0
	    da8     ONLINE       0     0     0
errors: No known data errors
</code>
As you can see, the rpool pool contains two devices: gpt/disk0 and gpt/disk1. They're set up as a mirror. The tank pool contains three devices in a RAIDZ. That means that the tank pool will survive one disk failing. If multiple disks fail at the same time, I'm hosed. The drawback to RAIDZ pools is that expansion of the pool either has to be done device-by-device (replace one disk, wait for resilver, replace another, wait for resilver, etc.) or by adding a set of disks of the same quantity as the existing RAIDZ. (In my case, adding three more disks).
