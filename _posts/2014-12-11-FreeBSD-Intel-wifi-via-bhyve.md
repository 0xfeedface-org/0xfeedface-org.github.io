---
layout: post
title: Intel Wifi Via Bhyve on FreeBSD
---

Introduction
============

So the HardenedBSD project, which I cofounded with Oliver Pinter, has
been really moving forward at a great pace. Our old development server
needed to be replaced and we now have fully automated builds with
Jenkins. That's all besides the point for today's blog post, which is
about the laptop I just bought for HardenedBSD development. I bought a
[Lenovo
Y50-70](http://shop.lenovo.com/us/en/laptops/lenovo/y-series/y50/). It
has an Intel Core i7 (Haswell) and Intel Wifi, among other awesome
hardware features. FreeBSD 11-CURRENT doesn't support Haswell video
acceleration through the i915 KMS driver nor does it support the wifi
chip. I'm using the VESA driver for video and I'm using Linux in a
bhyve VM for wifi. This blog post will show you how to get wireless
working the way I did if Linux natively supports your wifi chip but
FreeBSD does not.

The wired ethernet device is supported, so for the installation, I
plugged the laptop into a wired connection. Once installation and
initial updates are completed, you can disconnect the ethernet cable.

I started with an Ubuntu Server 14.04 LTS ISO. Installation was very
straightforward. I followed the [FreeBSD
Handbook](https://www.freebsd.org/doc/handbook/virtualization-host-bhyve.html)
to do the initial installation. But instead of using a file, I used a
zvol:

{% highlight sh %}
zfs create -omountpoint=none rpool/bhyve
zfs create rpool/bhyve/ubuntu-server-amd64-01
zfs create -V 100g -ovolmode=dev \
    rpool/bhyve/ubuntu-server-amd64-01/disk-01
{% endhighlight %}

Follow the installation as normal. I selected the SSH server and LAMP
server options. Even though LAMP isn't necessary, it's a nice thing to
have if I ever want to repurpose this VM.

Now that the VM is installed, we'll get to our dirty work, the reason
why this blog post exists.

Setting up the Host
===================

In our particular setup, the host will have a bridge device called
bridge0. This bridge device will have an IP of 192.168.6.1/24. We will
give the Linux VM an ethernet tap device, tap0, and assign it an IP of
192.168.6.2/24 in the VM.

We're going to have the host add the bhyve vmm driver, the NULL modem
driver, and the tap driver. We also need to tell the host to not try
to use the wifi device, but to pass it on to VMs.

To find out what on what PCI device your wifi device lives, run
<code>pciconf -lv</code>. You'll see something like this if it's an Intel wifi
chip:

<pre>
somethinghere@pci0:2:0:0:        class=0x028000 card=0x82708086 chip=0x08b48086 rev=0x93 hdr=0x00
    vendor     = 'Intel Corporation'
    class      = network
</pre>

The important part is the <code>pci0:2:0:0</code>. We'll take that and
plug that into <code>/boot/loader.conf</code>. So our loader.conf
should have these entries:

<pre>
vmm_load="YES"
nmdm_load="YES"
if_tap_load="YES"
pptdevs="2/0/0"
</pre>

Next up is setting up the bridge and the tap devices. Add
<code>net.link.tap_up_on_open=1</code> to
<code>/etc/sysctl.conf</code>. This will make it so that you don't
have to manually bring the tap device up before the bhyve VM boots.

In your <code>/etc/rc.conf</code> file, we should add these lines
(change according to your situation):

{% highlight sh %}
cloned_interfaces="bridge0 tap0"
ifconfig_bridge0="inet 192.168.6.1 netmask 255.255.255.0 addm tap0 up"
defaultrouter="192.168.6.2"
{% endhighlight %}

That's it! The host is now set up. Time to get the guest set up.

Setting up the Linux VM
=======================

I decided to go with Ubuntu Server, so these instructions are specific
to that. I'm sure other Linux distributions would work just as well.
When we boot Linux up, we have to give the <code>bhyve</code> command
an extra argument: <code>-s 5,passthru,2/0/0</code>. That tells bhyve
to forward the wifi device to the VM via PCI device 5. You'll notice
the <code>2/0/0</code> that came from my <code>pciconf</code> output
as <code>pci0:2:0:0</code>.

We need to change the <code>/etc/network/interfaces</code> file to
give eth0 a static IP and to tell the networking system to use WPA for
wireless. You'll notice a pre-up script for eth0. That will be
detailed later.

<code>
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
        address 192.168.6.2
        netmask 255.255.255.0
        network 192.168.6.0
        broadcast 192.168.6.255
    pre-up /scripts/nat.zsh

iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant.conf
</code>

I had some issues with manually configuring wpa_supplicant. I had to
add these lines to <code>/etc/rc.local</code> before the exit line:

{% highlight sh %}
ifdown wlan0
ifup wlan0
{% endhighlight %}

This will cause Ubuntu to bring up the wireless device at boot time.
Of course, if your network is using WPA, then you'll need to populate
<code>/etc/wpa_supplicant.conf</code> with your specific network
configuration parameters.

The last detail is the nat.zsh script. That creates the NATing rules.
Our Ubuntu VM will share the wifi connection via NAT to our FreeBSD
host. It's a pretty simple script and you'll want to tailor it to your
specific firewalling needs.

{% highlight sh %}
#!/usr/bin/env zsh

iptables="/sbin/iptables"

${iptables} -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
${iptables} -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
${iptables} -A FORWARD -i eth0 -o wlan0 -j ACCEPT
{% endhighlight %}

Conclusion
==========

This seems like a complete hack--and it definitely is. But once you
reboot and start the VM, you should have full networking working. My
next task to research is how to boot this VM at bootup time in the
host so that I don't have to manually start it in a new tmux session
after logging in.
