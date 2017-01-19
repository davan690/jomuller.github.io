---
layout: post
title:  "Set-up NeoVim on Mac for R"
categories: 
    - R 
    - r-bloggers
    - data_science
    - Vim
    - NeoVim
    - IDE
---

The objective of this post is to describe step-by-step how to set up NeoVim on Mac to use it as a powerfull editor for R code.
I need also a gui version of NeoVim for fast editing.

<img src="/assets/neovim-mark.png" title="NeoVim mark" style="display: block; margin: auto;" />

# Pre-requise

- OSX 10.11
- Xcode
- Admin access
- Homebrew
- Time necessary : 10 min

I consider there is no previous neovim install. We will not import my previous configuration, I want a fresh install.

# Softwares to install

- [NeoVim](https://github.com/neovim/neovim)
- [vim-plug](https://github.com/junegunn/vim-plug)
- [Nvim-R plugin](https://github.com/jalvesaq/Nvim-R)
- [neovim-dot-app](https://github.com/rogual/neovim-dot-app)

# Recipe

## Install NeoVim and UI

```
brew tap neovim/neovim
brew tap rogual/neovim-dot-app
brew install neovim-dot-app
brew linkapps neovim-dot-app
```

If there is some problem with e.g. msgpack, try

```
brew install --HEAD neovim-dot-app
```

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

# Extra

Python support 

```
sudo pip3 install neovim
```

And use always system clipboard. Add in `init.vim`

```
set clipboard+=unnamedplus
```

