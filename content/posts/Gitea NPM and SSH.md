---
title: Gitea NPM and SSH
date: 2026-06-28T09:05:11+05:30
lastmod: 2026-06-28T09:05:11+05:30
author: Satish Iyer
avatar: /img/author.jpg
authorlink: https://satishvis.github.io
cover: /img/gitea-npm-and-ssh.jpg
# covercaption: a description of the cover image
# images:
#   - /img/title.jpg
categories:
  - category1
tags:
  - tag1
  - tag2
# nolastmod: true
# math: true
draft: false
---

In a Proxmox Server with uses pihole as a DNS sinkhole combined with Nginx Proxy Manager as the Reverse Proxy, it is possible to get clean, secure domain names if we have a custom domain name e.g. <plex.yourdomain.com>.

But in such a scenario getting native Gitea app to host your private git repositories would pose an unique challenge, since Gitea hijacks the SSH port 22 for its purpose and Gitea does not allow ssh access to its underlying file systems.

<!--more-->

For this to work we need to make certain changes to the Gitea config file, generally located at `/etc/gitea/app.ini`.

So now with port 22 unavailabe, we need to deal with the problem as to how to enter the system. For this we can use some other ssh server to serve a port other than the default e.g. 2222 to ssh into the system. If we install openssh-server, since Gitea is better hosted on a Debian based system, we can change the port number in the /etc/ssh/sshd_config and then get access to the system.

But however there is one more problem, since pihole will direct the domain name to the NPM for reverse proxy, any ssh request to the domain name <gitea.yourdomain.com> will go to the NPM and hence fail the key authentication among other things like not known server, are you being hacked etc.

For such a situation, we need to create a Stream in NPM which will direct any request to <gitea.yourdomain.com> which comes to it in port 22 to forward it to port 22 of the ip of the Gitea server. But of course now that is done we can not log into the NPM with SSH since that is being forwarded.

For this we change the NPM Server ssh port to something else like 2222 and then we can log into the NPM server and get access to it.

One more problem remains. When we need to log into the server we need to remember to pass the ssh port `-p 2222` else we will not be able to log in.

For this we need to update the config in the .ssh directory of the host to enable us to use the ports there, so as to automatically use that port

```bash
 # NPM
  Host <192.168.xxx.xxx> 
     AddKeysToAgent yes
     IdentitiesOnly yes
     Port 2222
```

You make a similar entry for the Gitea server and then you can log into the Gitea server.
