---
layout: post
title: "A basic reproducible data analysis workflow - principles"
categories:
    - R
    - r-bloggers 
    - data science
    - reproducible analysis
---

The goal of **reproducible data analysis** is, [according to the CRAN task view](https://cran.r-project.org/web/views/ReproducibleResearch.html), *to tie specific instructions to data analysis and experimental data so that scholarship can be recreated, better understood and verified*.

In order to prepare a small talk on this subject for [the next Strasbourg's R user group meeting](https://www.meetup.com/fr-FR/StatsRbourg/events/236895990/), I'm starting a series of blog post. Today I will focus on the goals and constraints of my personal workflow.


# Goal of my workflow

Beside some side projects, I use mainly R in an academic context. For this, I must be able to:

- Check my analysis and track logical errors.
- Share them to be enhanced or curated by colleagues.
- Send my code with papers. In order to struggle with the non-reproducible results crisis, this practice [tends to become a standard](http://dx.doi.org/10.1136/bmj.i2770).
- As an epidemiologist, my raw data may change (*e.g.*: due to some quality data check or because a new year of data is available). Then I need to regenerate my reports with the new data.

# Constraints

Furthermore I have some constraints. 

- My workflow has to be **platform agnostic**. For academic works, I use various Linux distributions on my workstation and some remote machines. My hospital's computer with non-anonymous data run exclusively on Windows and I don't have root access (then limited choice of my software tools). My personal setting is a mix between [macOS](https://en.wikipedia.org/wiki/MacOS) and Linux boxes.
- For the same reason, it must be **easy to deploy**. I don't want to spend hours setting up a virtual or remote machine. Furthermore, peoples who wants to reproduce my analysis don't have either the time or the ressources to install some complicated software suite.
- **Free open source software** ([FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software)) is mandatory. Peoples who needs to run my analysis don't want to pay for software, academics are not rich. Furthermore, open-source software is the only way to control really what the software does. The reproducibility is unattainable with close-source software black boxes.
- Input must be the real **raw data**, whatever the format is (including *csv*, Microsoft Excel and Access files, SAS data or direct connection to [RDBMS](https://en.wikipedia.org/wiki/Relational_database_management_system)).
- Output could be html files (intermediate reports), PDF (for printing) or web app (Shiny).
- It must be **IDE agnostic**. I love Rstudio but I have to work on headless remote machines and I don't want to set up an Rstudio server each time. Script should be launch simply by a `R CMD` and edited in vim (or [nvim](2016-08-22-why neovim)).

# Summary of the implementation

With these goals and constraints, R is a natural choice:

- R is light (± 70 Mo) and easy to install compared to Python with data science modules or SAS with its awkward licence checking system.
- R's package system give me all the tools needed, on every OS.
- R is FOSS.
- R tends to become the *lingua franca* in data science and statistics.
- [Rstudio](https://www.rstudio.com/products/rstudio/) is a tremendous IDE, also easy to deploy

After several years of R practice, I developed a simple workflow based on 

- R with several packages: rmarkdown, knitr
- Pandoc 
- Git
- Coding style

This is basic, doesn't cover all reproductibility problems (*e.g.* no software archive) but is for me a good balance.

I will [describe this workflow in a later post]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}).

If you have your own reproducible data analysis workflow, please feel free to describe it in the commentaries or send me a link!

