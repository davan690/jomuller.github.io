---
layout: post
title: Install last version of vim-r-plugin for MacVim
categories:
- GNU R
- In English
- Vim
- r-bloggers
---
<p><a href="http://jomuller.fr/wp-content/uploads/2015/02/macvim_r.jpg"><img class="alignleft size-full wp-image-419" src="assets/macvim_r.jpg" alt="macvim_r" width="86" height="238" /></a>Since 4 months I'm using Vim as my main text editor for editing R code. Out the box, Vim and R are not able to communicate together. The <a href="https://github.com/jcfaria/Vim-R-plugin">vim-r-plugin</a> associated with the <a href="https://github.com/jalvesaq/VimCom">VimCom</a> R package provides a highly efficient link between Vim and R.</p>
<p>On Mac OS X, <a href="https://code.google.com/p/macvim/">MacVim</a> is an elegant solution to use Vim with a good integration to the OS. This is also easier than a CLI version for a beginner to discover the power of Vim. Indeed, there is menus for common commands and a lot of things work as user should expect in Mac OS X: pasting to the system clipboard, binding to standard shortcuts, directly open a file from the Finder…</p>
<p>However, to take full advantage of the <em>vim-r-plugin</em> and <em>VimCom</em> in <em>MacVim</em>, one have to install the last development version, that could be awkward.</p>
<p>The aim of this article is to describe this installation.</p>
<p><!--more--></p>
<p><a href="https://github.com/jalvesaq">Jakson Alves de Aquino</a>, the creator of this fantastics plug-in, described in a figure I reproduce above how MacVim and R are communicating using <em>vim-r-plugin</em> and <em>VimCom</em>. It's quite complicated because, as he explained me, the solution needs three different ways of communication to benefit of all the integrations: omnicompletion, object browser, sending code from Vim to R…</p>
<p>[caption id="attachment_424" align="aligncenter" width="300"]<a href="http://jomuller.fr/wp-content/uploads/2015/02/vimrcom.png"><img class="size-medium wp-image-424" src="assets/vimrcom-300x225.png" alt="Communication between MacVim.app and R.app (extract from the vim-r-plugin documentation)" width="300" height="225" /></a> Communication between MacVim.app and R.app (extract from the vim-r-plugin documentation)[/caption]</p>
<h2>Prerequise</h2>
<p>These steps were tested with the following configuration:</p>
<ul>
<li>OS X (Yosemite) 10.10.2</li>
<li>Have the administrator access</li>
</ul>
<h2>Install VIM side : vim-r-plugin</h2>
<h3>Install MacVim</h3>
<p>If not already done, install <a href="https://code.google.com/p/macvim/">MacVim</a></p>
<ol>
<li>Download the lasted snapshot <a href="https://github.com/b4winckler/macvim/releases">https://github.com/b4winckler/macvim/releases</a></li>
<li>Expand the archive and move MacVim to your <code>/Applications/</code> folder</li>
</ol>
<h3>Install Pathogen</h3>
<p><a href="https://github.com/tpope/vim-pathogen">Pathogen</a> is a plug-in manager for <em>Vim</em></p>
<p>To install it, open a terminal an type (or copy-paste):</p>
<p><code>mkdir -p ~/.vim/autoload ~/.vim/bundle &amp;&amp; \<br />
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim</code></p>
<h3>Install Git</h3>
<p><a href="http://www.git-scm.com/">Git</a> is a source control system. We will use it to download last version of the plug-in and the package.</p>
<p>To install Git:</p>
<ol>
<li>Go to <a href="http://www.git-scm.com/download/mac">http://www.git-scm.com/download/mac</a></li>
<li>Download it</li>
<li>Install it</li>
</ol>
<h3>Install vim-r-plugin</h3>
<p>Open a terminal and type:</p>
<p><code>cd ~/.vim/bundle/<br />
git clone https://github.com/jcfaria/Vim-R-plugin.git</code></p>
<p>To enable the documentation, open MacVim and type</p>
<p><code>:Helptags</code></p>
<h2>Install the R side : VimCom package</h2>
<h3>Install R</h3>
<p>Install last version of R with R GUI:</p>
<ol>
<li>Go to <a href="http://cran.r-project.org/bin/macosx/">http://cran.r-project.org/bin/macosx/</a></li>
<li>Download the binary for <em>Maverick</em> or higher (named <em>R-3.1.2-mavericks.pkg</em> today)</li>
<li>Install it</li>
</ol>
<h3>Xquartz</h3>
<p>To build the <em>VimCom</em> package from sources, you needs some dependancies from the X11 system. They can be installed with Xquartz. Reinstall it if necessary.</p>
<ol>
<li>Go to <a href="http://xquartz.macosforge.org/">http://xquartz.macosforge.org/</a></li>
<li>Download the last version (<em>XQuartz-2.7.7.dmg</em> today)</li>
<li>Install it</li>
</ol>
<h3>Build the <em>VimCom</em> package</h3>
<p>Finally we will build and install the <em>VimCom</em> package.</p>
<ol>
<li>Open the terminal</li>
<li>Type (or copy-paste) the following commands:</li>
</ol>
<p><code>cd ~/Downloads/<br />
git clone https://github.com/jalvesaq/VimCom<br />
R --vanilla CMD INSTALL --configure-args=--enable-clientserver VimCom</code></p>
<h3>Load <em>VimCom</em> when R start</h3>
<ol>
<li>Open MacVim</li>
<li>Type <code>:o ~/.Rprofile</code></li>
<li>Add the following lines:</li>
</ol>
<p><code>if (interactive()) {<br />
library(vimcom)<br />
}</code></p>
<h3>Check the installation</h3>
<p>To check the plug-in is properly installed, open <em>R.app</em> and type</p>
<p><code>?vimcom</code></p>
<p>The help dialog should open.</p>
<h2>Try it</h2>
<p>To check the installation, open MacVim, then</p>
<ul>
<li>Set the file type as R: <code>:set ft=R</code></li>
<li>Start R with <kbd>local leader</kbd> + <kbd>r</kbd> + <kbd>f</kbd></li>
<li>In Vim, write <code>1 + 1</code>, go back to normal mode, select the line and press <kbd>space bar</kbd>. The command should be send to R.</li>
<li>Open the object browser with <kbd>local leader</kbd> + <kbd>r</kbd> + <kbd>o</kbd></li>
<li>Try omni-completion: in MacVim, Insert mode, type <code>summ</code> and then <kbd>ctrl</kbd> + <kbd>x</kbd> + <kbd>o</kbd>. It should complete with <code>summary</code>.</li>
<li>Try argument completion: type <code>summary(</code> and then <kbd>ctrl</kbd> + <kbd>x</kbd> + <kbd>a</kbd>. It have to propose the argument <code>object</code>.</li>
</ul>
<h2>Tips</h2>
<ul>
<li>Read the <em>vim-r-plugin</em> documentation. It's well done!</li>
<li>Check you have no old version of the plugin</li>
<li>R.app must be open by the vim-r-plugin. Close R.app before starting MacVim.</li>
</ul>
<h2>Go further</h2>
<p>There is a lot of configuration possible. In another post, I will describe how to install the CLI version, with Tmux and nice colors in the R console.</p>
