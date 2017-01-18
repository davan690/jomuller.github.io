---
layout: post
title: "My basic reproducible data analysis workflow"
date: 2017-01-18
categories:
    - R
    - data science
    - reproducible analysis
---

The goal of **reproducible data analysis** is, [according to the CRAN task view](https://cran.r-project.org/web/views/ReproducibleResearch.html), *to tie specific instructions to data analysis and experimental data so that scholarship can be recreated, better understood and verified*.

In order to prepare a talk on this subject for [the next Strasbourg's R user group meeting](https://www.meetup.com/fr-FR/StatsRbourg/events/236895990/), I'm starting a serie of blog post. Today I will focus on my personnal workflow.


# Goal of my workflow

Academic use: be able to :

- check my analysis
- share it
- send to paper
- as an epidemiologist, raw data may change -> rerun the whole analysis with new data


# Implementation

## Tools

- Git
- rmarkdown
- knitr
- pandoc
- html
- coding style

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
