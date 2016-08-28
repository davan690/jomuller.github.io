---
layout: post
title:  "Call Antidote spell checker from Vim"
date: 2016-08-28 
categories:
    - Vim
    - NeoVim
---

The [Antidote spell checker](http://www.antidote.info/antidote/for-english-speakers) is from my point of view the best. But there is no integration to VIM. 

Then, I found a way – a least on Mac – to call Antidote from Vim. Simply add these lines to your Vim (or NeoVim) config file:

```
function CallAntidote9()
  :w
  call system("open -a /Applications/Antidote\\ 9.app ".bufname("%"))
endfunction
 
nmap <silent> <C-B> :call CallAntidote9()<CR>
```

This will:

- save your file
- open the file with Antidote

Then, just let Antidote do the checking, save the file in Antidote.
Finally, go back to Vim and reload the file with `:e`.

This script I still dumb, works only on mac but it’s helpful for me. I you have any suggestion to enhance it, you are welcome!
