---
layout: post
title: "Implementation of my reproducible data analysis workflow"
categories:
    - R
    - data science
    - reproducible analysis
---

## Organisation

- Folders
- Master R file
- Backup
- R session

# Benefits in the real life

Pragmatic
Easy to implement and deploy
Constraints: use of various workstations
Mainstream tools

# Limits

 my workflow don't allow to reproduce really everything. It's just

As I will discuss in a next post, there much more layers needed for reproducibility. The main weakness of my workflow is the lack of backup of the good version of the softwares I use (R's packages included). I already wasn't able rerun a old data analysis due to change in package used. (*e.g.* deprecated functions in dplyr or disappearance of a package in the CRAN). To improve this, I already try to add [*packrat*](https://cran.r-project.org/web/packages/packrat/index.html) to my workflow but it's was a long time ago and the package wasn't stable enough for my day to day work. Resolution for 2017: give another to packrat!
