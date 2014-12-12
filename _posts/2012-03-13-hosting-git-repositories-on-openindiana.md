---
layout: post
title: Hosting git Repositories On OpenIndiana
created: 1331663910
---
Around a year ago, I was introduced to git. I know, I'm pretty late to the party. <a href="http://git-scm.com/" target="_blank">git</a> is a distributed version control system (DVCS). It quickly took over subversion as my favorite version control system (VCS). I'm a big fan of the social coding site <a href="https://github.com/" target="_blank">GitHub</a>. For a long time, I've wanted to set up a server to host private repositories. GitHub allows private repos, but there's some data that I can't host on third-party sites, even if the data is marked private. My home server is running OpenIndiana. This article will show you how to set up a git server inside an OpenIndiana zone.

You will need to set up a new zone. If you don't have the instructions memorized (like me), you can follow the instructions <a href="http://blog.serverhorror.com/2009/11/24/howto-create-an-opensolaris-zone/" target="_blank">here</a>. I also delegated a ZFS dataset to the zone to store the repository files. You can learn how to do that by following <a href="http://docs.huihoo.com/opensolaris/solaris-zfs-administration-guide/html/ch08s02.html" target="_blank">this</a>.

After creating the zone and delegating the ZFS dataset, you'll need to create the git user. But first make sure that <code>/export/home</code> exists, otherwise useradd will fail. I set the home directory of the git user to the delegated ZFS dataset. After adding the user, give the user permission to snapshot the dataset: <code>zfs allow git snapshot,mount pool/dataset</code>. We will snapshot the dataset whenever a repository is created or deleted for security reasons. Create the <code>/etc/shells</code> file and place <code>/bin/sh</code> and <code>/usr/gnu/bin/git-shell</code> in it on their own lines. Set the shell of the git user to <code>/usr/gnu/bin/git-shell</code>.

Install sendmail and git. I used the SFE repo for git. If you don't have the SFE repo set up and don't know how, <a href="http://wiki.openindiana.org/oi/Spec+Files+Extra+Repository">this</a> is how you set it up. Enable the <code>smtp/sendmail</code> service. Our script will email us whenever a repo is created or deleted for security auditing purposes.

I created two scripts to help in <a href="https://github.com/lattera/helpers/blob/master/create_repo.sh" target="_blank">creating</a> and <a href="https://github.com/lattera/helpers/blob/master/delete_repo.sh" target="_blank">deleting</a> repositories. Download those scripts and place them in a new directory <code>~git/git-shell-commands</code> and name the scripts <code>create</code> and <code>delete</code> respectively. Now make the directory <code>~git/clients</code>. You will need to modify these scripts to change the email address and the dataset that gets snapshotted.

In your <code>/etc/ssh/sshd_config</code> file, you will need to add <code>PermitUserEnvironment yes</code>. Refresh and restart the service. Then, place in the file <code>~git/.ssh/environment</code> the text <code>PATH=/usr/gnu/bin:/usr/bin:/usr/sbin:/sbin:/bin</code>.

After all this is done, congratulations! You now have a working git server. You will need to add the public ssh key of your clients in <code>~git/.ssh/authorized_keys</code>. They themselves can ssh in and create/delete repositories. They can use their git client as usual: <code>git clone git@hostname:clients/client_name/repo.git</code>.
