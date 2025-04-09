---
layout: post
title: "Install Immich for media backup as Google Photo alternative"
date: 2025-04-09
author: CHRiSTeeNA
tags: ["NAS", "Docker", "Immich"]
categories: blog
---

So, I have setup a NAS to backup my datas.

Now I want to work on my media data, which I will use Immich for.

## [Immich](https://immich.app/)
Immich is an OSS for self-hosted photo and video management solution.

It is under a group of enthusiast engineers and very active on development!

### Why Immich

I stumbled on this app when looking for google photo alternative.

Because I have a lot of medias and does not feel like paying for Google One every year.

So I decided to give it a try and it turns out quite satisfying!!!

## Setting Up Immich Server
### Installation
Go to the install page of the official site.

https://immich.app/docs/install/docker-compose

It is recommended to host it using docker compose as it is just easy to use.<br>
Just follow the steps on the site, 

1. create a directory for the app
2. download the files needed
3. customize environment variables (Which is quite important as you would probably want to choose the location to store your files)
4.  start containers

And that's it!

### Optional for installation
You would probably want to setup auto start docker compose service on your server, but that's another problem for another day for now.

### Post Installation
After installation, just follow the steps on the [Post Installation steps](https://immich.app/docs/install/post-install), simply access the GUI from your browser through `you-machine-ip:2283`, create an admin user, and you are basically done!

Personally, customizing a storage template would be helpful for sorting datas as I can categorize them through the app, and for my own need.

#### Accounts
One of the best feature I like about this app is that I can create accounts for my family or for different devices. Not only that, I can assign certain storage size for specific accounts.

### Machine Learning
Immich also has face-regconition feature built-in for easy search through the albums - just like Google Photos. 

Although I am not too familiar with tweaking and fine-tuning the settings, it can be turn on by modifying few lines in docker compose yml file.

## After Thoughts
If you are looking for simple media storage solution, maybe staying with using Google Photos, or other cloud storage service solution is a better choice.

I swtiched my media storage to Immich because of 3 main reasons.

1. It has no subscription fees.
2. I have more control on my personal data, storage management, and flexibility.
3. I love the part of setting up my own servers and hardware.

If you are similar to me, maybe give it a try!