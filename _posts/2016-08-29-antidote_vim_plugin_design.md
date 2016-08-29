---
layout: post
title:  "Design a plugin to use Antidote in Vim"
date: 2016-08-29 
categories:
    - Vim
    - NeoVim
    - Antidote
---

<img src="/assets/antidote_neovim.png" title="Antidote and Neovim" style="display: block; margin: auto;" />

After a [quick and dirty solution to link Vim and Antidote]({% post_url 2016-08-28-antidote_vim %}), I though to a more elegant solution. It seems to be possible for me to write a small plugin for Vim to achieve this. The workflow would be:

- Create a temp file. If there is no selection, use the whole file.
- Avoid any interaction in Vim until Antidote is closed.
- Open the temp file in Antidote.
- Manullay make changes in Antidote.
- Validate in Vim and make replace the selection with the temp file.
- Remove the temp file.

Validation should be just to press "enter". A message has to appear in Vim.

Anything impossible. I just have to find time (I estimate the equivalent of one day to achieve this).
