---
title: mimeopen
date: 2026-05-23T18:31:17+05:30
lastmod: 2026-05-23T18:31:17+05:30
author: Satish Iyer
avatar: /img/author.jpg
authorlink: https://satishvis.github.io
cover: /img/mimeopen.jpg
# covercaption: a description of the cover image
# images:
#   - /img/cover.jpg
categories:
  - Productivity
tags:
  - Arch
  - KDE
# nolastmod: true
# math: true
draft: false
---

A few years back I had learnt about **Mimeopen** and other mime related stuff.
It is very powerful and works well if we understand the underlying stuff.

<!--more-->

I tried to understand the default applications in @Arch KDE.A whole lot of programs snucked around various places.
One of them was the /etc/environment
Another is /etc/profile
mimeopen -d filename.ext sets default application to open the file
Right click on the application in the start menu also gives some options, especially what terminal options to send
@Ranger has the rifle.conf for all the default opening settings. But it also has a commands.py file which can be used to set things up
There is another file commands-full.py that has all the other settings which one can pick and choose items to change the defaults in ranger.
Terminfo is an envronment variable that could be set in the profile and is TERMINFO=/etc/terminfo?
One can use the rifle file from the command line also to open some files.

rifle -l app.py
rifle -p 2 app.py
rifle -l test.txt
rifle -p 3 app.py
rifle -p 0 app.py
rifle -p 4 app.py

`konsole` @konsole is beautiful to work with the defaults are very nice. Nice and compact.
