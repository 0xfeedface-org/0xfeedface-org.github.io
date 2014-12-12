---
layout: post
title: Installing CM9 on an Encrypted Android Tablet
created: 1338855528
---
I love Android. I also love my Transformer Prime tablet. Time has proven to me, however, that CyanogenMod provides both a richer feature set and better stability than stock Android. Around three weeks ago, I traveled from the USA to Canada to attend BSDCan. Knowing how the TSA and US Customs agencies love to think they're above the law, I decided to encrypt my then-stock tablet. This article will explain how to move from an encrypted stock Android installation over to CM9, a not-so-easy feat to accomplish.

<strong>Disclaimer</strong>
While I hope these directions would be useful for other devices (smartphones included), the commands are specific to the Transformer Prime. You will need to do some research about the partition layout and the commands you'd need to run. This article is meant to be more of a guide and is meant to be adapted to your device. I am in no way, shape, or form responsible for any damage, repairable or not. I will not answer questions or comments about bricked devices. If you follow these steps (or adapt them to your device), you do so at your own risk.

<strong>Prerequisites</strong>
<ul>
<li>A spare SD card</li>
<li>A working adb installation</li>
<li>Knowledge of Linux shell commands</li>
<li>CM9 and gapps downloaded onto the spare SD card</li>
</ul>

<strong>The dilemma</strong>
When you encrypt your Android device, the /data partition is what gets encrypted. The partition in full is encrypted and cannot be mounted within CWM. CWM also cannot perform a wipe, since it cannot mount the /data partition. And CWM cannot mount the external SD card. This is our dilemma. No access to the internal SD. No access to the external SD. No way to perform a wipe and format the internal SD from CWM.

<strong>The Solution</strong>
What we need to do, then, is use adb while the tablet is in recovery mode to reformat the internal SD card, thereby removing the encryption. On the US Transformer Prime, the internal SD card is at <code>/dev/block/mmcblk0p8</code>. The partition for our spare SD card will be at <code>/dev/block/mmcblk1p1</code>. You will need to replace those device entries for any other device you might attempt this on. After formatting the internal SD card, we will mount it and then mount the external SD card at <code>/data/media</code>:

<ul>
<li>In CWM, wipe cache</li>
<li><code>adb shell</code></li>
<li><code>mke2fs -t ext4 /dev/block/mmcblk0p8</code> # Warning: This can take a while. Be patient.</li>
<li><code>mount /data</code></li>
<li><code>mount /dev/block/mmcblk1p1 /data/media</code></li>
</ul>

Now go back to CWM on your device, then go ahead and flash your CM9 and gapps zips like normal. You now have CM9 installed! The /data partition is not encrypted. If you do not need to flash any other ROMs (unlikely if you're reading this article), feel free to re-encrypt your device.

<strong>Conclusion</strong>
I want to give thanks to utkanos on FreeNode's irc at #cyanogenmod. He helped me put together the pieces as to why CWM wasn't able to mount /data. Because of him, I am now running CM9 on my Transformer Prime. I hope this article helps those who were in my same position.
