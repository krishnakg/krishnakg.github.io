+++
title = "Mounting Synology folder share on linux"
author = ["Krishna Gopalakrishnan"]
date = 2025-04-05T17:00:00-07:00
tags = ["linux"]
draft = false
+++

Accessing files stored on your Synology NAS directly from your Linux machine can be very convenient, whether it's for managing media libraries, accessing shared documents, or performing backups. This guide provides quick steps to mount a Synology share on Ubuntu or its derivatives (like Pop!_OS) using the CIFS protocol, ensuring it's available even after a reboot.

<!--more-->


## Prerequisites {#prerequisites}

-   On your Synology, go to Control Panel -&gt; File Services -&gt; SMB and check the 'Enable SMB service' option.
-   On your linux machine, install CIFS Utilities

<!--listend-->

```shell
sudo apt update
sudo apt install cifs-utils
```

-   Create a mount point where the Synology share will be accessible. Let's use /mnt/nas_media as an example.

`sudo mkdir /mnt/nas_media`


## Temporary mount (for testing) {#temporary-mount--for-testing}

First, let's test if we can mount the share manually. This mount will not survive a reboot. Make sure to use your synology username and password in the command.

```shell
# Find your user ID (uid) and group ID (gid) - often 1000 for the first user
id

# Mount the share
sudo mount -t cifs -o username=YourUsername,password=YourPassword,uid=1000,gid=1000 //synology_ip/Media /mnt/nas_media
```

To unmount
`sudo umount /mnt/nas_media`


## Persistent Mount {#persistent-mount}

To make the mount available after a reboot, we need to add an entry in /etc/fstab. But /etc/fstab is world readable, so let's first put the credentials in a secure file.

```shell
cat >> /root/.smbNas<<EOF
username=YourUsername
password=YourPassword
EOF

# Restrict access to the file to only root.
sudo chmod 600 /root/.smbNas
```

Now add the entry to /etc/fstab using the credentials file created above.

```shell
//synology_ip/Media /mnt/nas_media cifs credentials=/root/.smbNas,uid=1000 0 0
```

Now trigger the mount using `sudo mount -a` or by rebooting the machine.
