---
layout: post
title: Administering FreeBSD ASLR Through Firewall Rules
created: 1398958292
---
<strong>Why a firewall?</strong>
Over the past few months, I've been working with another developer named Oliver Pinter. He started work a few months back in porting PaX's ASLR implementation to FreeBSD and I've been enhancing his implementation in behalf of SoldierX. One of Oliver's goals was to port the <code>paxctl</code> program. When I joined him in this process, he hadn't yet started on the <code>paxctl</code> port. <code>paxctl</code> adds an ELF section header to executable files that tells the kernel ELF loader if ASLR should be turned on or off for this file. While he was fixing other bugs and implementing other features, I had started work on porting <code>paxctl</code> over. I quickly realized using ELF section headers for FreeBSD simply wasn't possible. Since they are optional, section headers are generally placed at the very end of the file. Program headers, the structures required for telling the kernel how to load the binary, are placed near the beginning of the file.

When you execute <code>execve</code>, FreeBSD's kernel will first check file existence, permissions, etc. If those checks succeed, the kernel will load only the first page of the file into memory. It uses this first page to check for a shebang line (#!/path/to/interpreter) or a binary file format. In our case, we're dealing with ELF files. The execution is passed to the ELF loader at this point.

The ELF loader loops through the program headers and figures out how to load the binary into memory. Oliver and I have implemented ASLR into this stage of the loading process. Remember, section headers are at the end of the file. Since only the first page of the file is mapped into memory, we don't have access to the PaX section headers. We're now stuck in a catch 22. If we want to get at the section headers, we have to load the rest of the file into memory. Loading the file into memory applies ASLR settings. The PaX section header might tell us to turn off ASLR for this binary. We would have to unload the binary and reload it with the ASLR settings from the PaX section applied. Loading a binary twice for every single execution is a waste.

FreeBSD has a really powerful policy-based security framework they call the <a href="http://www5.us.freebsd.org/doc/handbook/mac.html" target="_blank">MAC Framework</a>. The MAC framework rules get applied even before the first page of the binary is loaded. I figured that I could apply ASLR settings to binaries by tying into the MAC framework. One MAC module is the <a href="http://www.freebsd.org/cgi/man.cgi?query=mac_bsdextended&apropos=0&sektion=0&manpath=FreeBSD+11-current&arch=default&format=html" target="_blank">mac_bsdextended</a> module, which provides a firewall-like interface for setting privilege accesses on resources.

The mac_bsdextended module, and its userland interface, ugidfw, fits our end-goal perfectly.

<strong><code>ugidfw</code> rule changes</strong>
NOTE: I'll let you read the manual page for ugidfw that I linked to above for learning how to create generic rules. I don't want to waste time recreating the wheel.

I've modified the filesys object target to store the inode of a file if a file is specified, rather than the mountpoint of the filesystem. So if you run <code>ugidfw add subject uid lattera object filesys /bin/sh mode n</code>, that only disables access for /bin/sh, not for all executables for the filesystem /bin/sh is in. This behavior is different from before.

I've added an optional paxflags argument to the firewall ruleset. If set, this will enable or disable ASLR for a given binary. If you run <code>ugidfw add subject uid lattera object filesys /bin/sh mode rx paxflags a</code>, that will disable ASLR for /bin/sh, but still allow me to execute it.

<strong>Demo</strong>
<code>
$ id
uid=1001(lattera) gid=1001(lattera) groups=1001(lattera),0(wheel),920(vboxusers)
$ sudo ugidfw list
0 slots, 0 rules
$ ./test
1318
Address of ptr: 0x0000000001fdca56
^C
$ ./test
1319
Address of ptr: 0x0000000001d8aa56
^C
$ sudo ugidfw add subject uid lattera object filesys /usr/home/lattera/tmp/test mode rx paxflags a
0 subject uid lattera object filesys /usr/home/lattera mode rx paxflags a
$ ./test
1322
Address of ptr: 0x0000000001021a56
^C
$ ./test
1323
Address of ptr: 0x0000000001021a56
^C
$ ./test
1324
Address of ptr: 0x0000000001021a56
^C
$ sudo /usr/home/lattera/tmp/test
1326
Address of ptr: 0x0000000001d38a56
^C$ sudo /usr/home/lattera/tmp/test
1328
Address of ptr: 0x0000000001e59a56
</code>
<strong>Conclusion</strong>
Using a filesystem firewall will allow system administrators to create secure, dynamic rules to govern how ASLR is applied on a given system. There's no need to directly modify a file to turn on or off ASLR. If an application is misbehaving, simply add a <code>ugidfw</code> rule to disable ASLR for that application.
