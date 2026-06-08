---
title: Desk Cli App
date: 2026-06-08T08:44:54+05:30
lastmod: 2026-06-08T08:44:54+05:30
author: Satish Iyer
# avatar: /img/author.jpg
# authorlink: https://author.site
cover: /img/desk.jpg
# covercaption: a description of the cover image
# images:
#   - /img/cover.jpg
categories:
  - Productivity
tags:
  - shell
  - cli
  - bash
# nolastmod: true
# math: true
draft: false
---

Overview of the @desk cli environment.

Start with `desk init` to create the ~/.desk folder where all desks are
stored

`desk ls` to list all desks available. One per project.

Use `desk edit <name>` to edit the sh file

```
/home/satish/.desk/desks
├── book.sh
├── hugo.sh
└── letters.sh
```

<!--more-->

Example of the book.sh file:

```
# Description: Writing the Book on Shastra

echo -e "BOOK - Concentrate Don't get distracted....." \\
| boxes -d boy -a c
cd ~/Documents/0ShastraBook/0ShastraBookSrc
make log
PROMPT="%F{green}[Book]%F{red}%c
➤ %f"
echo « END
"Don't forget to use lvim....!"
END
```

Basically:

- change directory
- create the environment
- Can have a readme for the points to note
- custom prompt to remind us that we are in desk
