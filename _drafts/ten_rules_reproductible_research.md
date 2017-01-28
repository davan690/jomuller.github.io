---
layout: post
title:  Use the "Ten Simple Rules for Reproducible Computational Research" in R
categories: 
    - R 
    - Reproductible research
---

<a href="http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285"><img src="/assets/article_ten_rules_cover.jpg" alt="article_cover" style="float:left;width:200px"></a>

"[Ten Simple Rules for Reproducible Computational Research](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285)" is a
 freely available paper on [PLOS computational biology](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285). As [I'm currently very interested]({% post_url 2017-01-19-reproducible_analysis_my_principles %}) on the subject of reproductible data analysis,  it caught my attention.

In this posts, I will comment this ten rules, their relative importance and the possible implementation with R, all of this with my point of view of epidemiologist interested by healthcare data-reuse. I will also check if [my workflow]({% post_url 2017-01-21-implementation_basic_reproductible_workflow%}) comply with these rules.

For those who are in the hurry, I [summarize this rules and possible implementation in R in a table](#summary).

# Introduction

The author of this paper, Geir Kjetil Sandve, is [ assistant-professor of Norwegian](http://www.mn.uio.no/ifi/english/people/aca/geirksa/index.html) in team of biomedical informatics. I suppose these tens rules could be writen with R in mind because he cite it in rule 7. However, biomedical informatics is quite far from my practice and he seems to use other tools. For example, they have dedicated integrated framework allowing reproducibility. 

Two successive main goals for reproducible data analysis are pointed out:

1. Be able to **reproduce the results yourself**. It seems to be obvious. However in my experience, without a good framework implemented in the start of the analysis, you can faile to rerun an analysis shortly after haven produce the first version of a report. I experienced this all the time before switching on R ([see "Avoid Manual Data Manipulation Steps"](#manual_steps)).
2. Ensure **others  are able to reproduce the results** (*e.g.*: peers reviewers). This is a higher level of complexity because it suppose that's the way to rerun the analysis must be straightforwards (*e.g.*: steps writen somewhere). 

The author highlight the needs of a pragmatic setting that needs a trade-off between the ideals of reproductibility and simplicity. I claimed the same in [a previous post]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#benefits).


# The ten rules

The title below are the exact reproduction of the rules proposed in the Sandve's paper.

## 1 - *For Every Result, Keep Track of How It Was Produced*

In other words one report = one script. But there is a extra step because the ability to reproduce a result may rely on previous steps. Then Sandve advice is to note the exact sequence of steps or even better, to allow the direct execution of everything. I implemented this with the "[One report file = one R markdown file]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#report_equal_rmarkdown)" rule and the "[one R script to rule them all]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#run_all)". Another intersting proposition is to use Make files to fit all together. I already use this to produce my MPH dissertation in a well formated PDF and I found this perfect in this case. Yet, there is a major drawback: makefiles are not platform-agnostic.


## <a name="manual_steps"></a> 2 - *Avoid Manual Data Manipulation Steps*

Before mastering R, I used WYSIWYG softwares like Microsoft Excel, Libreoffice Calc or CDC's EpiInfo. They are easy to use but once the report produced, it was impossible to check the steps or rerun them with new data. Furthermore, human more error-prone than machines. But if you are a R user, the solution is builtin : just use R for every step (including datamanagement)! Furthermore, one have to check his coding style and avoid copy-pasting of code. In my workflow, I embrace [functional programing](http://adv-r.had.co.nz/Functional-programming.html) as much as I can (with a [dedicated folder]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#folders)).

## 3 - *Archive the Exact Versions of All External Programs Used*

Softwares changes, sometines at a very high pace like some R packages. Just try to run again a 4 year old R script that use ggplot. There is good chance you will at the minimum have some warnings and even have your script crash. This threat is even greater when the data format change (*e.g.* Microsoft Access and Excel before and after 2007).

In this case, Sandve point out the need to, at minimum note the version number of all the softwares use. With R, to reach the minimum is easy: just store the results of `sessionInfo()`.  A better practice is to archive all the softwares and their dependancy. Still easy to do in R, by making a copy of your library folders (use `.libPath() to locate them) when archiving the project. A more elegant way is to use the dedicated package [*packrat*](https://cran.r-project.org/web/packages/packrat/index.html) in your workflow.

Yet it could be not enough. The software we use rely on dependancies that relies may rely also on some operating system component. In this case storing the image of the [virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) with it hypervisor (*e.g* [Virtual Box](https://en.wikipedia.org/wiki/VirtualBox)) seem same with some drawbacks: it take a lot of space (OS + software + R project), running a complex analysis in a virtual machine could be slow. Another elegant way is to use [softwares containers](https://en.wikipedia.org/wiki/Operating-system-level_virtualization) like [Dockers](https://en.wikipedia.org/wiki/Docker_(software)). I have little with the later then I don't use it in my workflow but I plan to give Docker another try.

Even with this, there is still a layer missing: the hardware. Recently I tried to open a very old file produced on one of my first computer (a [Macintosh LC4](https://en.wikipedia.org/wiki/Macintosh_LC) under [Mac OS 7](https://en.wikipedia.org/wiki/System_7)). I'm still trying a way to open this binary file... The ultimate reproducible archive is maybe to store the hardware in some bunker.

<a href="https://commons.wikimedia.org/wiki/File:Kudrna-lc605b.jpg"><img src="/assets/lc_stack.jpg" title="A Macintosh LC stack" style="display: block; margin: auto;" /></a>

## 4 - *Version Control All Custom Scripts*

Did you ever make a change in your script that break everything ? Sometines, a simple "undo" command is enough to fix it. But the working version of the script could be an quite old one. In this case, you need a system to go back in time.

A common practice is to use manually named files ("script_v1.R", "script_v2.R", "script_final.R", "script_finalev2.R"...). It's better than nothing but hard to maintain in the long term.

<a href="http://www.phdcomics.com/comics/archive.php?comicid=1531"><img src="/assets/phd101212s.gif" title="Final doc" style="display: block; margin: auto;" /></a>

Use a version control system. Using R an Rstudio, git is the natural choice because it's well integrated and allow to interact with GitHub that tends to be (for good or bad) an unofficial package repository. The learning curve of these software is not steep compared to R.


## <a name="rule5"></a>5 - *Record All Intermediate Results, When Possible in Standardized Formats*

In theory, because we use reproductible data analysis, intermediate results are not mandatory. However a long process could be necessary to produce one results: *e.g.* data importation, cleaning, databases merging and additional variable creation are the basic first parts in my analysis. If I have to re-run all this stack each time I want to make any analysis I loose a lot of time. 

![Example of intermediate results](/assets/produced_data.jpg)

In my workflow, intermediate results are mandatory because one R markdown file = one question. The only way to chain all my R markdown files is to use intermediate data files. For example, my first file is almost always "import.Rmd". This import the raw files (csv, xlsx, mdb, web scrapping...). At the end of this R markdown file, I record the results. This way, the data imported are directly available for the next part of the analysis (most of the time data cleaning). 

My prefered format is RDS as [suggested some others](http://www.fromthebottomoftheheap.net/2012/04/01/saving-and-loading-r-objects/) with `saveRDS`. Other option is RData. And when it is a really important results, I save it also in .csv, just in case someone else wants to check the data with another software.

## 6 - *For Analyses That Include Randomness, Note Underlying Random Seeds*

Randomness is common: for example when using bootstrap method to estimate confidence intervals. Consequence: each time you run the analysis you have slighly differents results. The solution is easy to implement in R: just use `set.seed()`.

![Dice](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Dice_%28PSF%29.png/298px-Dice_%28PSF%29.png)

## 7 - *Always Store Raw Data behind Plots* 

Plots are important to communicate results. Producing an informative plot takes time and it's very common necessary to make cosmetical changes in a plot to fit in a final report or in a paper. As many R users, I use mainly ggplot2 to produce high-quality plots. Before starting a plot with ggplot, I produce the exact dataframe needed and simply save it with `saveRDS()`. This rules is [almost the same than No 5](#rule5).

![A plot I don't want to make again from scratch](/assets/hba1c-pentes.jpg)

## 8 - *Generate Hierarchical Analysis Output, Allowing Layers of Increasing Detail to Be Inspected*

The basic idea is that a summarized output (plot, table...) have to be linked with detailed value. This allow to debug this results.

In my workflow, I simply print the intermediate results (*e.g.* for a model with `summary(model_name)`) in the R Markdown file that will generate the final output. It's verbose but the people just have to go the end of the reports to find the summarized output. And because one R markdown file = one question, in theory there is only one summarised output in a R markdown file.

## 9 - *Connect Textual Statements to Underlying Results*

A data scientist could greatly increase the value of a results by adding clarification about how to read the results and how this results was produce. To implement this R markdown or Sweave are easy-to-use solutions. 

## 10 - *Provide Public Access to Scripts, Runs, and Rsults*

In my case, my "Public" are the people who ask me to analyze their data. Then I just send to them the whole project folder and explain them quickly were to find the reports (in the */reports/* subdirectory) and plots for their papers (in the *plots/* directory). It's straighforward and don't add any extra step.

When possible (for my personal projects), I share my projects with Git, but most of the time it's just a copy to an USB key or a zipped archive send by email.


# <a name="summary"></a>Summary

| |Rule summary | Possible implementation in R| Implementation in my workflow |
|:---:|:-----|:-----------------------------|------:|
|1 | *Keep track* | Keep every R script and track the sequence to run them | [a results = a script]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#report_equal_rmarkdown) and [`run_all.R`]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#run_all) |
|2 | *No manual manipulation* | Just use R! | R and functional programming |
|3 | *Archive software*| VM, docker or packrat | [*packrat*](https://cran.r-project.org/web/packages/packrat/index.html) (work in progress) + `sessionInfo()` + `Sys.info()` |
|4 | *Version control* | Git, SVN, mercury | Git |
|5 | *Record intermediate results* | *.Rdata*, *.rds*, *.csv*, *.txt*, *.xlsx*... | `saveRDS()` + `write.csv()`|
|6 | *Note random seeds* | `set.seed()` | `set.seed()` |
|7 | *Store plot's data* | *.rds* | `saveRDS()` before ggplot |
|8 | *Hierarchical Analysis output* | Print intermediate results | rmarkdown chunks |
|9 | *Comment the results* | Link results to text file, Sweave or Knitr  | R markdown files in *rmds/* subdirectory |
|10| *Public access* | Github, Gitlab, paper's repository... | Send the whole project's directory |

# Conclusion

All the 10 rules are easely recheable with R. Just by using R itself, the *rmarkdown* workflow and some organisationnal rules cover most of these rules.

Be welcome to expose in the comments your own way to follow these rules!

