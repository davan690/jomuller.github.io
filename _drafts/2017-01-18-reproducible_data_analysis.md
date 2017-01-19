---
layout: post
title: "My basic reproducible data analysis workflow"
categories:
    - R
    - data science
    - reproducible analysis
---

The goal of **reproducible data analysis** is, [according to the CRAN task view](https://cran.r-project.org/web/views/ReproducibleResearch.html), *to tie specific instructions to data analysis and experimental data so that scholarship can be recreated, better understood and verified*.

In order to prepare a small talk on this subject for [the next Strasbourg's R user group meeting](https://www.meetup.com/fr-FR/StatsRbourg/events/236895990/), I'm starting a serie of blog post. Today I will focus on the goals and constraints of my personnal workflow.


# Goal of my workflow

Beside some side projects, I use mainly R in an academic context. For this, I must be able to :

- check my analysis and track logical errors.
- share them to be enhanced or curated by colleagues.
- send my code with papers. In order to struggle with the non-reproductible results crisis, this practice [tends to become a standard](http://dx.doi.org/10.1136/bmj.i2770).
- as an epidemiologist, my raw data may change (*e.g.*: due to some quality data check or because a new year of data is available). Then I need to regenerate my reports with th new data

# Constraints

Furthermore I have some constraints. 

- My workflow have to be **OS agnostic**. For academic works, I use varions Linux distributions on my workstation and some remotes machines. My hospital's computer with non-anonymous data run exclusivly on Windows and I don't have root access (then limited choice of my software tools). My personal setting is a mix between [macOS](https://en.wikipedia.org/wiki/MacOS) and Linux boxes.
- For the same reason, it must be **easy to deploy**. I don't want to spend hours to set up a virtual or remote machine. Furthermore, all my colleagues don't have high computer skills (non their job, *e.g.* clinicians) but want's to reproduce the analysis.
- **Free open source softwares** ([FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software)) are mandatory. Peoples who needs to run my analysis don't want to pay for the softwares, academic is not rich. Furthermore, open source software are the only way to control really what the software do. Reproducibility is unattainable with close sources software black boxes.
- Input must be the real **raw data**, whatever the format is (including *csv*, Microsoft's Excel and Access files, SAS data or direct connection to [RDBMS](https://en.wikipedia.org/wiki/Relational_database_management_system)).
- Output could be html files (intermediate reports), PDF (for printing) or webapp (Shiny).

# Summary of the implementation

With these goals and constraints, R is a natural choice:

- R is really easy to install (contrary to Python with data science extensions)
- R's package system give me all the tools needed, on every OS.
- R is FOSS
- R tends to become the *lingua franca* in data-science and statitics
- Rstudio is a tremedous IDE, also easy to deploy

After several years of R practice, I manage to implement a simple workflow based on 

- R with several packages: rmarkdown, knitr
- Pandoc 
- Git
- Coding style

I will describe this workflow in a next post.

If you have your own reproducible data analysis workflow, please feel free to describe it in the commentaries or send me a link!

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
