---
layout: post
title: rmarkdown vs markdown R packages
date: 2015-03-16
categories:
- In English
- GNU R
- packages
author: Joris Muller
---

To convert Markdown files to _html_ in [_R_](http://r-project.org/) two packages are currently available: [_markdown_](http://cran.r-project.org/web/packages/markdown/index.html) and [_rmarkdown_](http://cran.r-project.org/web/packages/rmarkdown/index.html) (note there is an extra _r_ to the last one).

Both are very similar but for my actual project I had to choose and master one. Then I compared them in the table above.

# Comparison table

 | _markdown_ | _rmarkdown_
---|:--------:|:---------:
Title | Markdown rendering for R | Dynamic Documents for R
Maintainer | Yihui Xie | JJ Allaire
Creator | JJ Allaire | JJ Allaire
Date of last version | 2014-08-24 | 2015-01-26
Project is active on GitHut | [Not since december 2014](https://github.com/rstudio/markdown/graphs/contributors) | [Yes](https://github.com/rstudio/rmarkdown/graphs/contributors?from=2014-12-25&to=2015-03-16&type=c)
Depends on or imports | mime | tools, utils, knitr, yaml, htmltools, caTools
Require Pandoc installed | No | Yes 
Is imported by Knitr | Yes | No
Convert to HTML | Yes | Yes
Convert to PDF | No | Yes
Convert to Docx | No | Yes
Size without dependencies in MacOS X | 417 Ko | 3184 Ko
Main function | `markdownToHTML` | `render`
YAML | No | Yes
Table of Content | Yes <br/> (with `option = "toc"`) | Yes <br/> (specified in the YAML header of the _.md_)

# Choice

The main obstacle to using _rmarkdown_ is its requirement of Pandoc installed in the system.

I'm working on a reporting tool that have to work in various of platforms, including PC's under Windows XP without admin access. Then we could install R and packages but not easily Pandoc.

Furthermore, this report tool must have the minimum dependencies, and I have to use _knitr_ that include _markdown_ package. 

As a conclusion, _rmarkdown_ is the default choice when you can use Pandoc, and you can have a lot of dependancies. _markdown_ package is a good choice for lightweight applications that have to produce only simple HTML files.
