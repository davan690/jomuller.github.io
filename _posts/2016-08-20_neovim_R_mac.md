---
layout: post
title:  "Set-up NeoVim on Mac for R"
date: 2016-08-20 
categories: R data_science Vim NeoVim IDE
---

# Objective

The objective of this post is to describe step-by-step how to set up NeoVim on Mac to use it as a powerfull editor for R code.
I need also a gui version of NeoVim for fast editing.

# Pre-requise

- OSX 10.11
- Admin access
- Homebrew
- Time necessary : 

I consider there is no previous neovim install. We will not import my previous configuration, I want a fresh install.

# Why ?

# Softwares

- [NeoVim](https://github.com/neovim/neovim)
- [vim-plug](https://github.com/junegunn/vim-plug) plugin manager
- Nvim-R plugin

# Recipe

## Install NeoVim

Homebrew

## Install the plugin manager

With a single command proposed on the `vim-plug` page, create the config directory for NeoVim, the `autoload` directory and download `vim-plug`.

```
curl -fLo ~/.config/nvim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Update your NeoVim's config file `~/.config/nvim/init.vim`

```
call plug#begin()

call plug#end()
```

Then test in NeoVim if the command `Plu` is autocompleted (by *e.g.* `PlugInstall`).

## Install Nvim-R

Just add the repo of Nvim-R in the vim-plug section

```
call plug#begin()

Plug `jalvesaq/Nvim-R`

call plug#end()
```

Reload the NeoVim's config file

```
source $MYVIMRC
```

And install

```
:PlugInstall
```

That's all!
