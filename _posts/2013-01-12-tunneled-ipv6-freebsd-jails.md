---
layout: post
title: Tunneled IPv6 FreeBSD Jails
created: 1358045576
---
This past year, I've taken a special interest in IPv6. I love that NAT isn't needed. I love that I can have all the static IPs I want. I love testing out what works and what doesn't. I had some spare time today to spend on hobbyist stuff, so I decided to try to get my FreeBSD machine running IPv6. The network the FreeBSD machine is on only supports IPv4. I have a VPS that runs OpenVPN. I'll show how to use Hurricane Electric's 6in4 tunnel to get your FreeBSD machine hooked up with IPv6 along with any jails. Of course, this article assumes you're using vnet (using <code>epair</code> devices). My jail administration Drupal <a href="https://github.com/lattera/drupal-jailadmin" target="_blank">module</a> supports this configuration out-of-the-box.

<strong>The Network</strong>
I do not control the network my FreeBSD box is on. The WAN interface doesn't respond to pings. However, I have a VPS that is running OpenVPN. I use it so that I can get into my computers from anywhere. My FreeBSD VPS does respond to ICMP pings, which Hurricane Electric needs. The stable version OpenVPN does not natively support routing IPv6. So I set up NAT on my VPS, which will be my gateway only for IPv4 packets destined to Hurricane Electric. My OpenVPN setup creates a NATed network, subnet 192.168.3.0/24 with OpenVPN sitting at 192.168.3.1.

<strong>Setting Up IPv6</strong>
The endpoint of my Hurricane Electric IPv6 tunnel is at 72.52.104.74. I set up a static route to route all traffic destined to that IP through OpenVPN: <code>route add -host 72.52.104.74 192.168.3.1</code>. I then setup the <code>gif</code> tunnel to Hurricane Electric. I won't worry about pasting those commands, since you can find them at Hurricane Electric's site.

<strong>Sharing the IPv6 Connection With The Jails</strong>
You would think that you could create a <code>bridge</code> device and add the <code>gif</code> device to it. However, FreeBSD has a known bug that prevents that from working. You will need to assign the <code>bridge</code> an IPv6 address and add at least your physical NIC to the <code>bridge</code> (if you have multiple NICs, just one of them will be fine). After you do that, you can add as many <code>epair</code> devices as you want to the bridge. The jails will use the bridge's IPv6 address as their IPv6 default route.

<strong>End Result</strong>
Here's what my setup looks like:
<code>
$ ifconfig bridge0                                                                         [9:46:20]
bridge0: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
	ether 02:fe:21:34:d3:00
	inet6 2001:470:8142:1::1 prefixlen 64 
	nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
	id 00:00:00:00:00:00 priority 32768 hellotime 2 fwddelay 15
	maxage 20 holdcnt 6 proto rstp maxaddr 2000 timeout 1200
	root id 00:00:00:00:00:00 priority 32768 ifcost 0 port 0
	member: epair1a flags=143<LEARNING,DISCOVER,AUTOEDGE,AUTOPTP>
	        ifmaxaddr 0 port 21 priority 128 path cost 2000
	member: bge0 flags=143<LEARNING,DISCOVER,AUTOEDGE,AUTOPTP>
	        ifmaxaddr 0 port 5 priority 128 path cost 200000
$ sudo jexec ClamAV_Dev ifconfig epair1b                                                   [9:50:05]
epair1b: flags=8843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST> metric 0 mtu 1500
	options=8<VLAN_MTU>
	ether 02:fb:c0:00:16:0b
	inet6 2001:470:8142:1::3 prefixlen 64 
	inet6 fe80::fb:c0ff:fe00:160b%epair1b prefixlen 64 scopeid 0x2 
	inet [snipped IPv4 address] netmask 0xfffffe00 broadcast [snipped IPv4 broadcast]
	nd6 options=21<PERFORMNUD,AUTO_LINKLOCAL>
	media: Ethernet 10Gbase-T (10Gbase-T <full-duplex>)
$ sudo jexec ClamAV_Dev ping6 -c 1 google.com                                              [9:50:18]
PING6(56=40+8+8 bytes) 2001:470:8142:1::3 --> 2607:f8b0:4004:801::100e
16 bytes from 2607:f8b0:4004:801::100e, icmp_seq=0 hlim=53 time=156.747 ms
--- google.com ping6 statistics ---
1 packets transmitted, 1 packets received, 0.0% packet loss
round-trip min/avg/max/std-dev = 156.747/156.747/156.747/0.000 ms
$ sudo jexec ClamAV_Dev netstat -rn -f inet6                                               [9:52:20]
Routing tables
Internet6:
Destination                       Gateway                       Flags      Netif Expire
default                           2001:470:8142:1::1            UGS     epair1b
</code>

Screenshots of using my jail administration Drupal module can be found <a href="http://imgur.com/a/s0fGY" target="_blank">here</a>.
