---
layout: post
title:  The "Ten Simple Rules for Reproducible Computational Research" are easy to reach for R users
categories: 
    - R 
    - Reproductible research
    - r-bloggers
---

<a href="http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285"><img src="/assets/article_ten_rules_cover.jpg" alt="article_cover" style="float:left;width:150px"></a>

"[Ten Simple Rules for Reproducible Computational Research](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285)" is a
freely available paper on [PLOS computational biology](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285).

As [I'm currently very interested]({% post_url 2017-01-19-reproducible_analysis_my_principles %}) on the subject of reproducible data analysis, I will describe the possible implementation of these ten rules in R with my point of view of epidemiologist interested in healthcare data reuse. I will also check if [my workflow]({% post_url 2017-01-21-implementation_basic_reproductible_workflow%}) comply with these rules.

For those who are in a hurry, I [summarised these rules and possible implementation in R in a table at the end of this post](#summary).

# Introduction

The author of this paper, Geir Kjetil Sandve, is [assistant-professor](http://www.mn.uio.no/ifi/english/people/aca/geirksa/index.html) in a Norwegian biomedical informatics lab. It is possible he wrote these rules with R in mind because he cites R in rule 7. However, biomedical informatics is quite far from my practice and he describes other integrated framework allowing reproducibility. 

Two successive main goals for reproducible data analysis are pointed out:

1. Be able to **reproduce the results yourself**. It seems to be obvious. However in my experience, without a good framework implemented at the start of the analysis, you can fail to rerun an analysis shortly after haven produce the first version of a report. I experienced this all the time before switching on R ([see "Avoid Manual Data Manipulation Steps"](#manual_steps)).
2. Ensure **others  are able to reproduce the results** (*e.g.*: peer reviewers). This is a higher level of complexity because it supposes that's the way to rerun the analysis is straightforward (*e.g.*: steps written somewhere). 

The author highlight the needs of a pragmatic setting that needs a trade-off between the ideals of reproducibility and simplicity. I agree strongly with this point of view [as I wrote in a previous post]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#benefits).


# The ten rules

The titles below are the exact reproduction of the rules proposed in the Sandve's paper.

## 1 - *For Every Result, Keep Track of How It Was Produced*

In other words, one report = one script. But there is an extra step because the ability to reproduce a result may rely on previous steps. Sandve advice to note the exact sequence of steps or even better, to allow the direct execution of everything. I implemented this with the "[One report file = one R markdown file]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#report_equal_rmarkdown)" rule and the "[one R script to rule them all]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#run_all)". Another interesting proposition is to use Make files in order to link each part, especially when relying on different software. I already use this to produce my MPH dissertation PDF and I found this perfect in this case. Yet, there two drawback: makefiles are not platform-agnostic and it tends to become a truly labyrinthine system.


## <a name="manual_steps"></a> 2 - *Avoid Manual Data Manipulation Steps*

Before mastering R, I used WYSIWYG softwares like Microsoft Excel, LibreOffice Calc or CDC's EpiInfo. They are easy to use but once the report produced, it was impossible to check the steps or rerun them with new data. Furthermore, human more error-prone than machines. But if you are an R user, the solution is built in : just use R for every step (including data management)! Furthermore, one have to check his coding style and avoid copy-pasting of the code. In my workflow, I embrace [functional programing](http://adv-r.had.co.nz/Functional-programming.html) as much as I can (with a [dedicated folder]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#folders)).

## 3 - *Archive the Exact Versions of All External Programs Used*

Software changes, sometimes at a very high pace like some R packages. Just try to run again a 4-year-old R script that uses ggplot. There is a good chance you will at the minimum have some warnings and even have your script crash. This threat is even greater when the data format change (*e.g.* Microsoft Access and Excel before and after 2007).

In this case, Sandve points out the need to, at a minimum, note the version number of all the software use. With R, to reach the minimum is easy: just store the results of `sessionInfo()`.  A better practice is to archive all the software and their dependency. Still easy to do in R, by making a copy of your library folders (use `.libPath()` to locate them) when archiving the project. A more elegant way is to use the dedicated package [*packrat*](https://cran.r-project.org/web/packages/packrat/index.html) in your workflow.

Yet it could be not enough. The software we use relies on dependencies that may also rely on some operating system component. In this case storing the image of the [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) with it hypervisor (*e.g* [Virtual Box](https://en.wikipedia.org/wiki/VirtualBox)) could be a solution. Some drawbacks of VM: it take a lot of space (OS + software + R project) and running a complex analysis in a virtual machine could be slow. Another elegant way is to use [software containers](https://en.wikipedia.org/wiki/Operating-system-level_virtualization) like [Dockers](https://en.wikipedia.org/wiki/Docker_(software)). I have little with the later then I don't use it in my workflow but I plan to give Docker another try.

Even with this, there is still a layer missing: the hardware. Recently I tried to open a very old file produced on one of my first computer (a [Macintosh LC4](https://en.wikipedia.org/wiki/Macintosh_LC) under [Mac OS 7](https://en.wikipedia.org/wiki/System_7)). I'm still trying a way to open this binary file... The ultimate reproducible archive is maybe to store the hardware in some bunker.

<a href="https://commons.wikimedia.org/wiki/File:Kudrna-lc605b.jpg"><img src="/assets/lc_stack.jpg" title="A Macintosh LC stack" style="display: block; margin: auto;" /></a>

## 4 - *Version Control All Custom Scripts*

Did you ever make a change in your script that breaks everything ? Sometimes, a simple "undo" command is enough to fix it. But the working version of the script could be a quite old one. In this case, you need a system to go back in time.

A common practice is to use manually named files ("script_v1.R", "script_v2.R", "script_final.R", "script_finalev2.R"...). It's better than nothing but hard to maintain in the long term.

<a href="http://www.phdcomics.com/comics/archive.php?comicid=1531"><img src="/assets/phd101212s.gif" title="Final doc PhD comics" style="display: block; margin: auto;" /></a>

Just use a [version control](https://en.wikipedia.org/wiki/Version_control) system. If you are using R with Rstudio and don't already have one, [git](https://git-scm.com/) is the natural choice. It is well integrated and allow to interact with GitHub that tends to be ([for good or bad](https://www.wired.com/2015/06/problem-putting-worlds-code-github/)) an unofficial package repository. The learning curve of this software is not steep compared to R.


## <a name="rule5"></a>5 - *Record All Intermediate Results, When Possible in Standardized Formats*

In theory, because we use reproducible data analysis, intermediate results are not mandatory. However a long process could be necessary to produce one results: *e.g.* data importation, cleaning, databases merging and additional variable creation are the first basic parts in my analysis. If I have to re-run all this stack each time I want to make any analysis, I lose a lot of time. 

![Example of intermediate results](/assets/produced_data.jpg)

In my workflow, intermediate results are mandatory because one R markdown file = one question. The only way to chain all my R markdown files is to use intermediate data files. For example, my first file is almost always "import.Rmd". This import the raw files (csv, xlsx, mdb, web scrapping...). At the end of this R markdown file, I record the results. This way, the data imported are directly available for the next part of the analysis (most of the time data cleaning). 

My preferred format is RDS as [suggested some others](http://www.fromthebottomoftheheap.net/2012/04/01/saving-and-loading-r-objects/) with `saveRDS`. Another option is RData. And when it is a really important results, I save it also in .csv, just in case someone else wants to check the data with another software.

## 6 - *For Analyses That Include Randomness, Note Underlying Random Seeds*

Randomness is common: for example when using [bootstrap method](http://statmethods.net/advstats/bootstrapping.html) to estimate confidence intervals. Consequence: each time you run the analysis you have slightly different results. The solution is easy to implement in R: just use `set.seed()`.

![Dice](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Dice_%28PSF%29.png/298px-Dice_%28PSF%29.png)

## 7 - *Always Store Raw Data behind Plots* 

Plots are important to communicate results. Producing an informative plot takes time and it's very common necessary to make cosmetic changes in a plot to fit in a final report or in a paper. As many R users, I use mainly [ggplot2](http://ggplot2.org/) to produce high-quality plots. Before starting a plot with ggplot, I produce the exact data frame needed and simply save it with `saveRDS()`. This rule is [almost the same as No.5](#rule5).

![A plot I don't want to make again from scratch](/assets/hba1c-pentes.jpg)

## 8 - *Generate Hierarchical Analysis Output, Allowing Layers of Increasing Detail to Be Inspected*

The basic idea is that a summarised output (plot, table...) has to be linked with detailed value. This allows to understand and debug this result.

In my workflow, I simply print the intermediate results (*e.g.* for a model with `summary(model_name)`) in the R Markdown file that will generate the final output. It's verbose but the people just have to go to the end of the reports to find the summarised output. And because one R markdown file = one question, in theory there is only one summarised output in a report.

## 9 - *Connect Textual Statements to Underlying Results*

A data scientist could greatly increase the value of the result by adding clarification about how to read the results and how these results were produced. To implement this R markdown or Sweave are easy-to-use solutions. The recently introduced [R notebooks](http://rmarkdown.rstudio.com/r_notebooks.html) add the possibility to see directly code, results and comment when writing the R markdown file.

## 10 - *Provide Public Access to Scripts, Runs, and Results*

In my case, my "Public" are the people who ask me to analyse their data. Then I just send to them the whole project folder and explain them quickly were to find the reports (in the */reports/* directory) and plots for their papers (in the *plots/* directory). It's straightforward and doesn't add any extra step.

When possible (for my personal projects), I share my projects with Git, but most of the time it's just a copy to an USB key or a zipped archive send by email.

This is not exactly the spirit of Sandve advice. He advocate for data and scripts sharing to everyone for the sake of science transparency. The project I'm working on are not always for papers (*e.g.* analysis for my healthcare facility) and the aim is not to share it to the world. But [more and more medical paper](http://annals.org/aim/pages/authors#data-sharing-and-reproducible-research) asks for this material and I will be happy to provide it if I have the opportunity to submit a paper.

# <a name="summary"></a>Summary

| |Rule summary | Possible implementation in R| Implementation in my workflow |
|:---:|:-----|:-----------------------------|------:|
|1 | *Keep track* | Keep every R script and track the sequence to run them | [a results = a script]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#report_equal_rmarkdown) and [`run_all.R`]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#run_all) |
|2 | *No manual manipulation* | Just use R! | R and functional programming |
|3 | *Archive software*| VM, docker or packrat | [*packrat*](https://cran.r-project.org/web/packages/packrat/index.html) + `sessionInfo()` + `Sys.info()` |
|4 | *Version control* | Git, SVN, mercury | Git |
|5 | *Record intermediate results* | *.Rdata*, *.rds*, *.csv*, *.txt*, *.xlsx*... | `saveRDS()` + `write.csv()`|
|6 | *Note random seeds* | `set.seed()` | `set.seed()` |
|7 | *Store plot's data* | *.rds* | `saveRDS()` before ggplot |
|8 | *Hierarchical Analysis output* | Print intermediate results | rmarkdown chunks |
|9 | *Comment the results* | Link results to text file. Use Sweave or Knitr.  | R markdown files in *rmds/* subdirectory |
|10| *Public access* | Github, Gitlab, paper's repository... | Send the whole project's directory |

# Conclusion

All the 10 rules proposed in [the Sandve paper](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285) are reachable for a R user. Just by using R itself, the *rmarkdown* workflow and some organisational rules cover most of these rules. [My basic reproductible workflow meet almost all the criterias]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}) with the notable exceptions of the software archive (but it's work in progress with packrat) and the lack of public access (but I can't share everything).

Be welcome to expose in the comments your own way to follow these rules!

# Image copyrigth

All the images of this post are from [Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page) or are my own with the notable exeption of the PHD comic strip from [phdcomics.com](http://phdcomics.com/).
