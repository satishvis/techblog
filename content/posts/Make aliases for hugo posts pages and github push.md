---
title: Make Aliases for Hugo Posts Pages and Github Push
date: 2024-12-11T10:18:35+05:30
lastmod: 2024-12-11T10:18:35+05:30
author: Satish Iyer
avatar: /img/author.jpg
authorlink: https://satishvis.github.io
# cover: /img/cover.jpg
images:
  - /img/cover.jpg
categories:
  - Hugo
tags:
  - aliases
  - shell
  - bash
  - zsh
# nolastmod: true
---

Certain aliases to make our hugo posts, pages and github push easier

<!--more-->

```shell
# aliases for hugo
hpost() {
   hugo new content content/posts/"$1".md
   nvim content/posts/"$1".md
}

hpage() {
   hugo new content pages/"$1".md
   nvim content/pages/"$1".md

}

hugopush() {
   cd public
   git add .
   git commit -m "$1"
   git push origin main
}
```
