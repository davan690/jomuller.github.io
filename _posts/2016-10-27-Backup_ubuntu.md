---
layout: post
title:  "Incremental backup in Ubuntu"
date: 2016-10-27 
categories:
    - Linux
    - Ubuntu
    - Backup
---

<img src="https://raw.githubusercontent.com/gnome-design-team/gnome-icons/master/apps/hicolor/256x256/apps/deja-dup.png" alt="dejadup_icon" style="float:left;width:100px;height:100px;">

[Backup](https://en.wikipedia.org/wiki/Backup) is vital in the digital age, even more for critical files like the data analysis for the work. I recently changed my work setup from a Mac under OSX to a tiny and very nice [ThinkCentre M](https://en.wikipedia.org/wiki/ThinkCentre#M83_Tiny) under [Ubuntu 16.04 LTS](https://en.wikipedia.org/wiki/Ubuntu_version_history#Ubuntu_16.04_LTS_.28Xenial_Xerus.29). The installation and setup my data-science toolbox was straingforward ([APT](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) is my friend). 

But I struggled to find a equivalent of [Apple's Time Machine](https://en.wikipedia.org/wiki/Time_Machine_(macOS)) to do local incremental backup. First, I tried [Back in Time](https://github.com/bit-team/backintime) until I realised it produced snapshots instead of incremental backup. After [further research](https://help.ubuntu.com/community/BackupYourSystem), I discovered that Ubuntu has already a (semi-)built-in solution. I say semi-built-in because for an unknown reason, the front-end (*Backup* application) is available but not the terminal application ([duplicity](https://en.wikipedia.org/wiki/Duplicity_(software))).

To use it :

1. Install duplicity `sudo apt-get install duplicity`
2. Launch *Backup* application
3. Select a storage location (*e.g.* an external HDD)
4. Turn on the automatic backup

For the moment it seems to works. Next step: find how to choose various target, including a remote one, to complete the [3-2-1 backup strategy](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/).
