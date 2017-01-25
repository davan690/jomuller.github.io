---
layout: post
title:  "Notes on a paper - Ten Simple Rules for Reproducible Computational Research"
categories: 
    - R 
    - Reproductible research
---

Img paper

"[Ten Simple Rules for Reproducible Computational Research](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285)" is a
 freely available paper on [PLOS computational biology](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285). As [I'm currently very interested]({% post_url 2017-01-19-reproducible_analysis_my_principles %}) on the subject of reproductible data analysis,  it caught my attention.

In this posts, I will comment this ten rules, their relative importance and the possible implementation with R, all of this with my point of view of epidemiologist interested by healthcare data-reuse. I will also check if [my workflow]({% post_url 2017-01-21-implementation_basic_reproductible_workflow%}) comply with these rules.

For those who are in the hurry, I [summarize this rules and possible implementation in R in a table](#summary).

# Introduction

The author of this paper, Geir Kjetil Sandve, is [ assistant-professor of Norwegian](http://www.mn.uio.no/ifi/english/people/aca/geirksa/index.html) in team of biomedical informatics. According to his LinkedIn profile, he his skilled in R. I suppose these tens rules could be writen with R in mind althoug he his using others tools in his recent papers. Biomedical informatics is quite far from my practice. For example, they have dedicated integrated framework allowing reproducibility. 

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

## *Archive the Exact Versions of All External Programs Used*

packrat

## *Version Control All Custom Scripts*

git



# <a name="summary"></a>Summary

| |Rule summary | Possible implementation in R| Implementation in my workflow |
|:---:|:-----|:-----------------------------|------:|
|1 | *Keep track* | Keep every R script and track the sequence to run them | [a results = a script]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#report_equal_rmarkdown) and [`run_all.R`]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#run_all) |
|2 | *No manual manipulation* | Just use R! | R and functional programming |
|3 | *Archive software*| VM, docker or packrat |
|4 | *Version control* | Git |
|5 | *Record intermediate results* | *.rds* and *.csv* |
|6 | *Note random seeds* | `set.seed()`` |
|7 | *Store plot's data* | *.rds* |
|8 | *Hierarchical Analysis output* | ? |
|9 | *Comment the results* | R markdown |
|10| *Public access* | Github, Gitlab, paper's repository... |

# Conclusion

All the 10 rules are easely recheable with R.


