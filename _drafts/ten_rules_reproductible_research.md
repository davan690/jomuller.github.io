---
layout: post
title:  "Notes on a paper - Ten Simple Rules for Reproducible Computational Research"
categories: 
    - R 
    - Reproductible research
---

Img paper

"Ten Simple Rules for Reproducible Computational Research" is the title of a
 freely available paper on [PLOS computational biology](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285). As [I'm currently very interested]({% post_url 2017-01-19-reproducible_analysis_my_principles %}) on the subject of reproductible data analysis,  caught my attention.

In this posts, I will comment this ten rules, their relative importance and the possible implementation with R, all of this with my point of view of epidemiologist interested by healthcare data-reuse.

For those who are in the hurry, I [summarize this rules and possible implementation in R in a table](#summary).

# Introduction

The author of this paper, Geir Kjetil Sandve, is [ assistant-professor of Norwegian](http://www.mn.uio.no/ifi/english/people/aca/geirksa/index.html) in team of biomedical informatics. According to his LinkedIn profile, he his skilled in R then this tens rules could be writen with R in mind althoug he his using others tools in his recent papers. Biomedical informatics is quite far from my practice. For example, they have dedicated integrated framework allowing reproducibility. 

Two successive main goals for reproducible data analysis are pointed out:

1. Be able to **reproduce the results yourself**. It seems to be obvious. However in my experience, without a good framework implemented in the start of the analysis, you can faile to rerun an analysis shortly after haven produce the first version of a report. I experienced this all the time before switching on R ([see "Avoid Manual Data Manipulation Steps"](#manual_steps)).
2. Ensure **others  are able to reproduce the results** (*e.g.*: peers reviewers). This is a higher level of complexity because it suppose that's the way to rerun the analysis must be straightforwards (*e.g.*: steps writen somewhere). 

The author highlight the needs of a pragmatic setting that needs a trade-off between the ideals of reproductibility and simplicity. I claimed the same in [a previous post]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#benefits).


# The ten rules

1. "*For Every Result, Keep Track of How It Was Produced*".
2. <a name="manual_steps"></a>"*Avoid Manual Data Manipulation Steps*".

# <a name="summary"></a>Summary

| |Rule summary | Possible implementation in R|
|:---:|:-----|:-----------------------------|
|1 | *Keep track* | Keep every R script |
|2 | *No manual manipulation* | Just use R! |
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


