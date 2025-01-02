---
title: Github and Bitbucket setup
date: 2024-12-10T16:42:10+05:30
lastmod: 2024-12-11T11:25:50+05:30
author: Satish Iyer
avatar: /img/author.jpg
authorlink: https://satishvis.github.io
cover: /img/cover.jpg
# images:
#   - /img/cover.jpg
categories:
  - Hugo
tags:
  - arch package
# nolastmod: true
---

Go to the .git repository and change the url from https:// to
ssh://git@github.com:xxx/<repo>
Need to remember that the bitbucket also accepts this:

git@bitbucket.org:<repo_owner>/<reponame>.git

Now one is ready to test it in any repository Good to go


The following to be done before

<!--more-->
```shell
> eval `ssh-agent -s`

$> ssh-keygen -t rsa -C <usename/email>
$> pbcopy id_rsa_bitbucket.pub # Use xsel or xclip in linux
----> go to Bitbucket and add this.
$> ssh-add -D  ---> only if you want to delete previous entries
$> ssh-add id_rsa_bitbucket
$> ssh -T bb
$> vim config ---> to write the config
```
The config file should look like this:

```
#Bitbucket (work)
  Host bb
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_rsa_bitbucket
```


