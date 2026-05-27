---
title: Rclone
date: 2026-05-26T19:13:51+05:30
lastmod: 2026-05-26T19:13:51+05:30
author: Satish Iyer
avatar: /img/author.jpg
authorlink: https://satishvis.github.io
cover: /img/rclone.jpg
# covercaption: a description of the cover image
# images:
#   - /img/cover.jpg
categories:
  - Productivity
tags:
  - google_drive
  - cloud storage
  - rclone
# nolastmod: true
# math: true
draft: false
---

rclone GDrive systemd startup mount script
Rclone is a command line utility used for reading and writing to almost any type of cloud or remote storage. From Google Drive to Ceph, rclone supports almost any cloud-based remote storage platform you can think of. You can perform upload, download or synchronisation operations between local storage and remote cloud storage, or between remote storage directly.

<!--more-->

In addition to this, rclone has an experimental mount feature that lets a user mount a remote cloud storage provider, such as s3 or Google Drive, as a local filesystem. You can then use the mounted filesystem as if it were a local device, albeit with some performance considerations.

Before we get going, make sure you have rclone installed on your system and configured with a remote.

curl https://rclone.org/install.sh | sudo bash
rclone config

Once you have a remote defined, it’s time to create the mountpoint and systemd script. I’ll be using Google Drive for this example, but the mount command works for any supported remote.

Create the mount point directory to use for the remote storage:

mkdir /mnt/google-drive

Next, create the below systemd script and edit it as required:

vi /etc/systemd/system/rclone.service

# /etc/systemd/system/rclone.service

[Unit]
Description=Google Drive (rclone)
AssertPathIsDirectory=/mnt/google-drive
After=plexdrive.service

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount \
 --config=/root/.config/rclone/rclone.conf \
 --allow-other \
 --cache-tmp-upload-path=/tmp/rclone/upload \
 --cache-chunk-path=/tmp/rclone/chunks \
 --cache-workers=8 \
 --cache-writes \
 --cache-dir=/tmp/rclone/vfs \
 --cache-db-path=/tmp/rclone/db \
 --no-modtime \
 --drive-use-trash \
 --stats=0 \
 --checkers=16 \
 --bwlimit=40M \
 --dir-cache-time=60m \
 --cache-info-age=60m gdrive:/ /mnt/google-drive
ExecStop=/bin/fusermount -u /mnt/google-drive
Restart=always
RestartSec=10

[Install]
WantedBy=default.target

The important parts are detailed below, however, there are various other options are detailed on the rclone mount documentation page.

    - –config – the path to the config file created by rclone config. This is usually located in the users home directory.
    - gdrive:/ /mnt/google-drive – details two things; firstly the config name created in rclone config, and secondly the mount point on the local filesystem to use.

Once all this is in place you’ll need to start the service and enable the service at system startup (if required)

systemctl start rclone
systemctl enable rclone
