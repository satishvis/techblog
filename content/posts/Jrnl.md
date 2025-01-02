---
title: Jrnl - Manage your notes and journal entries from Command line
date: 2024-12-10T16:37:07+05:30
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

Making alias for @jrnl - j in my zsh.
Earlier it was aliased to 'jobs-l'.
# Basic Usage
jrnl has two modes: composing and viewing. Basically, whenever you don't supply any arguments that start with a dash or double-dash, you're in composing mode, meaning you can write your entry on the command line or an editor of your choice.

We intentionally break a convention on command line arguments: all arguments starting with a single dash will filter your journal before viewing it, and can be combined arbitrarily. Arguments with a double dash will control how your journal is displayed or exported and are mutually exclusive (ie. you can only specify one way to display or export your journal at a time).

# Listing Journals
You can list the journals accessible by jrnl

jrnl -ls
The journals displayed correspond to those specified in the jrnl configuration file.

# Composing Entries
Composing mode is entered by either starting jrnl without any arguments – which will prompt you to write an entry or launch your editor – or by just writing an entry on the prompt, such as

jrnl today at 3am: I just met Steve Buscemi in a bar! He looked funny.
Note

Most shell contains a certain number of reserved characters, such as # and *. Unbalanced quotes, parenthesis, and so on will also get into the way of your editing. For writing longer entries, just enter jrnl and hit return. Only then enter the text of your journal entry. Alternatively, use an external editor <advanced>).

You can also import an entry directly from a file


jrnl < my_entry.txt

# Smart timestamps
Timestamps that work:

at 6am
yesterday
last monday
sunday at noon
2 march 2012
7 apr
5/20/1998 at 23:42
Starring entries
To mark an entry as a favourite, simply "star" it

jrnl last sunday *: Best day of my life.
If you don't want to add a date (ie. your entry will be dated as now), The following options are equivalent:

jrnl *: Best day of my life.
jrnl *Best day of my life.
jrnl Best day of my life.*

# Note

Just make sure that the asterisk sign is not surrounded by whitespaces, e.g. jrnl Best day of my life! * will not work (the reason being that the * sign has a special meaning on most shells).

# Viewing
jrnl -n 10
will list you the ten latest entries (if you're lazy, jrnl -10 will do the same),

jrnl -from "last year" -until march
everything that happened from the start of last year to the start of last march. To only see your favourite entries, use

jrnl -starred
Using Tags
Keep track of people, projects or locations, by tagging them with an @ in your entries

jrnl Had a wonderful day on the @beach with @Tom and @Anna.
You can filter your journal entries just like this:

jrnl @pinkie @WorldDomination
Will print all entries in which either @pinkie or @WorldDomination occurred.

jrnl -n 5 -and @pineapple @lubricant
the last five entries containing both @pineapple and @lubricant. You can change which symbols you'd like to use for tagging in the configuration.

## Note

jrnl @pinkie @WorldDomination will switch to viewing mode because although no command line arguments are given, all the input strings look like tags - jrnl will assume you want to filter by tag.

# Editing older entries
You can edit selected entries after you wrote them. This is particularly useful when your journal file is encrypted. To use this feature, you need to have an editor configured in your journal configuration file (see advanced usage <advanced>)

jrnl -until 1950 @texas -and @history --edit
Will open your editor with all entries tagged with @texas and @history before 1950. You can make any changes to them you want; after you save the file and close the editor, your journal will be updated.

Of course, if you are using multiple journals, you can also edit e.g. the latest entry of your work journal with jrnl work -n 1 --edit. In any case, this will bring up your editor and save (and, if applicable, encrypt) your edited journal after you save and exit the editor.

You can also use this feature for deleting entries from your journal

jrnl @girlfriend -until 'june 2012' --edit
Just select all text, press delete, and everything is gone...

# Advanced Usage
Configuration File
You can configure the way jrnl behaves in a configuration file. By default, this is ~/.config/jrnl/jrnl.yaml. If you have the XDG_CONFIG_HOME variable set, the configuration file will be saved as $XDG_CONFIG_HOME/jrnl/jrnl.yaml.

Note

On Windows, the configuration file is typically found at %USERPROFILE%\.config\jrnl\jrnl.yaml.

The configuration file is a YAML file with the following options and can be edited with a plain text editor.

Note

Backup your config file before editing. Changes to the config file have destructive effects on your journal!

journals paths to your journal files
editor if set, executes this command to launch an external editor for writing your entries, e.g. vim. Some editors require special options to work properly, see FAQ <recipes> for details.
encrypt if true, encrypts your journal using AES.
tagsymbols Symbols to be interpreted as tags. (See note below)
default_hour and default_minute if you supply a date, such as last thursday, but no specific time, the entry will be created at this time
timeformat how to format the timestamps in your journal, see the python docs for reference
highlight if true, tags will be highlighted in cyan.
linewrap controls the width of the output. Set to false if you don't want to wrap long lines.
Note

Although it seems intuitive to use the # character for tags, there's a drawback: on most shells, this is interpreted as a meta-character starting a comment. This means that if you type

jrnl Implemented endless scrolling on the #frontend of our website.

your bash will chop off everything after the # before passing it to jrnl. To avoid this, wrap your input into quotation marks like this:

jrnl "Implemented endless scrolling on the #frontend of our website."

Or use the built-in prompt or an external editor to compose your entries.

# Multiple journal files
You can configure jrnlto use with multiple journals (eg. private and work) by defining more journals in your jrnl.yaml, for example:

journals:
  default: ~\journal.txt
  work: ~\work.txt
The default journal gets created the first time you start jrnl Now you can access the work journal by using jrnl work instead of jrnl, eg.

jrnl work at 10am: Meeting with @Steve
jrnl work -n 3
will both use ~/work.txt, while jrnl -n 3 will display the last three entries from ~/journal.txt (and so does jrnl default -n 3).

You can also override the default options for each individual journal. If your jrnl.yaml looks like this:

encrypt: false
journals:
default: ~/journal.txt
work:
  journal: ~/work.txt
  encrypt: true
food: ~/my_recipes.txt
Your default and your food journals won't be encrypted, however your work journal will! You can override all options that are present at the top level of jrnl.yaml, just make sure that at the very least you specify a journal: ... key that points to the journal file of that journal.

Note

Changing encrypt to a different value will not encrypt or decrypt your journal file, it merely says whether or not your journal is encrypted. Hence manually changing this option will most likely result in your journal file being impossible to load.

# Known Issues
Unicode on Windows
The Windows shell prior to Windows 7 has issues with unicode encoding. To use non-ascii characters, first tweak Python to recognize the encoding by adding 'cp65001': 'utf_8', to Lib/encoding/aliases.py. Then, change the codepage with chcp 1252 before using jrnl.


