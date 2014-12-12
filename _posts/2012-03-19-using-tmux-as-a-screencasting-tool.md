---
layout: post
title: Using tmux as a Screencasting Tool
created: 1332191254
---
I've been hearing a few of the folks I associate with praising tmux. For years, I used GNU's screen. I thought I'd give tmux a try. I've since fallen in love with it. I learned that tmux has a read-only option. I figured that using tmux's read-only option, I could create a useful tool for screencasting a command-line interface. This article will show you how to easily set up your box with tmux to screencast the commandline.

<strong>Requirements</strong>
<ul>
<li>bash</li>
<li>tmux</li>
<li>sshd</li>
</ul>
<strong>Setting up the System</strong>
You will need to download my helper shell script. You can find it <a href="https://github.com/lattera/helpers/blob/master/tmux-ro.sh" target="_blank">here</a>. Place it in <code>/usr/local/bin</code>, <code>/usr/bin</code>, or another place of your liking. Create a new user and set its shell to the path of the script (for example, <code>adduser -m -s /usr/local/bin/tmux-ro.sh caster</code>). In this article, we'll use <code>caster</code> as the username of our sample user. We'll set the password to be the same as its username.

<strong>Starting the Screencast</strong>
My helper script allows for multiple screencasts, using the name of the tmux session as the name of the screencast. In this article, we'll use <code>testcast</code> as the name of the screencast. Start the screencast by creating a new session of name <code>testcast</code> as the user you just set up: <code>sudo -u caster tmux new -s testcast</code>.

<strong>Sharing the Screencast</strong>
Now that the user has been created, its shell set to the helper script, and a screencast started, your viewers can now connect. Connect to your box using ssh as the <code>caster</code> user. You will be presented with a prompt asking for the session name. Enter <code>testcast</code> as the session name. You should now be able to view the screencast!

<strong>Recording the Screencast</strong>
tmux can also record your screencast. First make the directory <code>~/tmux/logs</code>. Then, In each pane you wish to record, you can execute the tmux command to record it: <code>pipe-pane -o 'cat >> ~/tmux/logs/`date +%F_%T`.#S-#I-#P.log'</code>. You're now recording your screencast!

<strong>Conclusion</strong>
Screencasting with tmux is really easy. You now have a fully-functional command-line screencasting setup.
