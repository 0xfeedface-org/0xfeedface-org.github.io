---
layout: post
title: Building HardenedBSD-Based OPNSense Images
created: 1447346540
---
# Introduction

I've been doing builds of OPNSense backed by HardenedBSD for a while.
I have been asked to write a little tutorial on how I do it. By the
end of this tutorial, you should have the knowledge and ability to
create OPNSense builds based on HardenedBSD.

Please note that these steps are still not concrete. OPNSense is under
rapid development and the HardenedBSD-based builds are still extremely
experimental. If any steps do change, I will add an appendix with
updated instructions.

# Setup

I do the builds in a bhyve VM. That VM is running HardenedBSD
10-STABLE. OPNSense doesn't support 11-CURRENT at the moment. Side
note: I'm working with Franco to forward-port the base patches to
11-CURRENT. You will need to run HardenedBSD 10-STABLE in the build
environment you'll use to create these builds.

This tutorial will not step you through installing HardenedBSD
10-STABLE in bhyve (or whatever hypervisor you chose) as that is
outside the context of this tutorial. You could even do this on bare
metal, but I like to keep things separated into different VMs for
different tasks.

# Checkout "ALL THE REPOS!"

Before checking out all the git repos you'll need, create the
following directories:

* /opnsense
* /opnsense/builds
* /opnsense/packages
* /opnsense/sets

There's a number of repos you'll need to check out:

* /usr/src: git://github.com/HardenedBSD/hardenedBSD.git
  * branch: hardened/experimental/opnsense-10-stable
* /usr/ports: git://github.com/HardenedBSD/hardenedbsd-ports.git
  * branch: opnsnese-master
* /usr/core: git://github.com/HardenedBSD/opnsense-core.git
  * branch: backport/stable/15.7
* /usr/tools: git://github.com/HardenedBSD/opnsense-tools.git
  * branch: backport/stable/15.7
* /opnsense/data: git://github.com/HardenedBSD/opnsense-data.git
  * branch: master

# HardenedBSD-Specific Bits

HardenedBSD maintains its own OPNSense repos because HardenedBSD has
not been able to upstream enhancements. Some of these enhnacements
simply because the work doesn't fit OPNSense's use cases.

I've abstracted out all of the configurable bits. OPNSense requires
certain variables to be set in a certain way or certain paths to
exist. OPNSense also generates images based on Deciso's appliance
serial console settings, which don't align with Netgate's. In order to
support both, I've abstracted out a lot of those settings and even
added some basic function hooking support at image generation time.

The configuration files that HardenedBSD uses is at `/opnsense/data`,
from the opnsense-data repo you just cloned. You'll notice a file
called `common.conf`, which all of the device-specific builds use.
`netgate-apu4.conf` is for Netgate's APU4 appliances and
`netgate-rcc-ve-4860.conf` is for Netgate's RCC-VE-4860 appliances.

Take a careful look at `common.conf`. There are file and directory
paths that you'll need to ensure exist. You'll need a package signing
key. Follow the `pkg-repo(8)` manpage for creating that key.

# Performing the Build

OPNSense's documentation states that you should simply be able to run
`make` in `/usr/tools`. However, due to the changes I've made, you'll
need to go a different route.

Here's what I run. Let's pretend we're building for the Netgate
RCC-VE-4860. All these commands should be run as root.

1. cd /usr/tools/build
1. /bin/sh ./clean.sh -c /opnsense/data/netgate-rcc-ve-4860.conf packages base kernel images obj stage release
1. /bin/sh ./base.sh -c /opnsense/data/netgate-rcc-ve-4860.conf
1. /bin/sh ./kernel.sh -c /opnsense/data/netgate-rcc-ve-4860.conf
1. /bin/sh ./ports.sh -c /opnsense/data/netgate-rcc-ve-4860.conf
1. /bin/sh ./core.sh -c /opnsense/data/netgate-rcc-ve-4860.conf
1. /bin/sh ./memstick.sh -c /opnsense/data/netgate-rcc-ve-4860.conf
1. /bin/sh ./iso.sh -c /opnsense/data/netgate-rcc-ve-4860.conf

That's it! By the end of the last command, you should have installer
images built and ready to go. The `memstick.sh` and `iso.sh` scripts
will tell you where it placed the images.

NOTE: I don't generate the nano images because I've seen some users
have issues with those. I haven't added the necessary hooks and
patches to the nano image generation script as I have with memstick
and iso.
