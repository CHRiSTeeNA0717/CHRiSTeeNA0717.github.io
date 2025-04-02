---
layout: post
title: "Build a DIY NAS using old PC"
date: 2025-04-01
tags: "NAS"
author: CHRiSTeeNA
categories: blog
---

## How it started

I upgraded, or should I say replaced, my old gaming PC with a new one. <br>
After that, I was thinking about how to deal with my old PC.<br>
I was going to sell it to my friend, but at the end they ends up not buying it.<br>
After that, when I was tinkering with my google drive, I thought of why not build a NAS out of that PC so that I can unsubscribe from some of the cloud storage services. <br>
And the journey started.

## How I do it
### Planning
I started from researching what do I need to build a NAS.
### PC Case
I need HDD storages, obviously, but my old gaming rig does not have many storage spaces for my HDDs. So I bought a second hand Fractal define R5 to move all my parts into it. R5 is a perfect case for NAS. It has 8 HDD bases, and sponges on the side and top of the case, although I have no idea what is the purpose but it looks premium.
### Install OS (Ubuntu)
I backuped my datas to my current PC, and wiped the Windows on the old pc and installed [Ubuntu 24.94.2 LTS](https://ubuntu.com/download/desktop) on it. <br>
### Initial setups
After the installation, I first install ssh on it so that I can connect it from my PC and laptop. 
```
sudo apt update
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```
That way, I can just put it away somewhere around the corner with a power cord and ethernet cable and don't need to worry about i/o.<br>
Using the ssh command, I can connect to it anywhere my home wifi reaches.<br>
```
ssh username@private_ip
```

On my home router, I assigned a static private IP to my NAS so that it will not be assign random ip from the router that is using DHCP.
```
ip link show #find the ether field that has the MAC address (something like aa:bb:cc:dd:ee:ff)
```
Use that Mac address to assign a ip for it on the router setting page.<br>

### Installing storages
After plugging in all my storages/HDDs, I setup by following the steps below:
Install Btrfs to config RAID
```
sudo apt install -y btrfs-progs
```
Setup RAID1 for 2 of my drives. Because I only have 2 currently
```
# -m for metadata, -d for data
sudo mkfs.btrfs -m raid1 -d raid1 /dev/sda /dev/sdb 
```
Create a directory to store data, and mount it to the drive
```
# create a directory to mount it to the HDD
sudo mkdir /mnt/nas
sudo mount /dev/sda /mnt/nas
# Verify the setup
sudo btrfs filesystem show
```
After that, setup auto mount on boot
```
# Get UUID and setup auto-mount on boot
blkid | grep btrfs
UUID=<the-uuid> /mnt/nas btrfs defaults 0 0
```

### Connecting the NAS to other devices
Now here comes the actual fun part!<br>
To create a network storage, I use Samba for the simplicity for windows and MacOs.<br>
First, install it
```
sudo apt install samba -y
```
Then, edit the config file at `/etc/samba/smb.conf`
Add the lines as below to the bottom of the file
```
[NAS]
   path = /mnt/nas # the storage created earlier
   browseable = yes
   read only = no
   guest ok = no
   create mask = 0777
   directory mask = 0777
   valid users = <the user to access this storage>
```
Create the user stated in `valid users`
```
sudo adduser <the user to access this storage>
```
After creation, set the user to use Samba, and set the password for it
```
sudo smbpasswd -a <the user to access this storage>
```
Lastly, restart Samba
```
sudo systemctl restart smbd
sudo systemctl enable smbd
```
Open windows file Explorer and type in `\\<private-ip-of-NAS>\nas`<br>
Now I can actually connect my PC to this freshly created Ubuntu NAS Server!!!

## First error
I eagerly try to copy my folders to the NAS, but Windows stated that I have no access to the folder.

It turns out that I have to configure Samba to approve read/write permissions.

I installed Samba using `sudo`, which means the ownership is the root user. 
So, I made some tweaks to the user permissions of Samba, and restarted it.
```
sudo chown -R <the user to access this storage>:<the user to access this storage> /mnt/nas
sudo chmod -R 775 /mnt/nas
sudo systemctl restart smbd
```

After that, I can happily copy all my movies, animes, musics and documents to my NAS!!

## After thoughts
This is the first time I tried to do some diy projects. If you don't count building PC.

Next step will be trying to setup other things on my NAS like VPN.

Have a nice day!