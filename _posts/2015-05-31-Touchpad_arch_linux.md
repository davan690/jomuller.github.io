---
layout: post
title: "Macbook air 6.1 Touchpad on ArchLinux"
date: 2015-05-31
categories:
- Linux
- Arch Linux
- Informatique
- In English
- Mac
author: Joris Muller
---

After a fresh install of Arch Linux on my Macbook Air 6.1, I'm not satisfied of the way the thouchpad behave. 

The first think I have done was to install the synaptic driver. But I have some issues with it :

- When I use the mechanical clic, the curser move to the area I clicked. In MacOS X, when I does it, the cursor movement seems to be disabled. This is what I Excpect.
- Only scroll down and up gesture seems to work (with two finger).

# Method

- Write down the specifications
- Gather a maximum of information about this device on
	- Arch Wiki
	- Arch Forum
	- General web research
	- Device name
- Implement

# Specifications

Globally, the goal is to have a touchpad working as in OS X.

- Click on tap
- No cursor moving when using the mechanical click
- Two finger to scroll down and up "naturally"
- Three fingers to move objects (windows), or make selections.
- If possible, four fingers to have something close to Mission Control.

# Search results

## Arch Forum

[xf86-input-cmt-xorg](https://github.com/hugegreenbug/xf86-input-cmt) [seem's to be](https://bbs.archlinux.org/viewtopic.php?pid=1523031#p1523031) a better driver than synaptic. This driver is a _X11 ChromiumOS touchpad driver ported to Linux_ according to the description in GitHub.

[One post](https://bbs.archlinux.org/viewtopic.php?id=193771) ask exactly the same questions, if there is a way to have the same touchpad behavior in Arch than in OS X.
