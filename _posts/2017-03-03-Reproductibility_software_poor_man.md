---
layout: post
title: Which software archive layer for data analysis reproductibility to choose?
categories:
    - r
    - reproductible analysis
---

(image : layers of reproductibility)

In the [journey for reproductible data analysis]({% post_url 2017-01-19-reproducible_analysis_my_principles %}), I identified a blind spot on my workflow [because I don't have a real software archive strategy]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %}#limits). To fix this issue, we will see the aim of archive softwares used for a data analysis, then some specification for an ideal solution and I will describe some solutions. 

For those who are in the hurry, a [table summarize the solutions at the end of this post](#summary).

# Introduction
## Aim

complete

- Aim: let's be clear. The main goal of my reproductible data analysis workflow is for same data with the same analysis be able to produce the same results in a pragmatic way. By extension, I want to be able with reuse again the same analysis pipeline with similar data (e.g.: update of the data for a new year). The initial goal was not to distribute an app to a client. But could be an extension. 
- My current workflow have - Software archive is describe as [important in the ten rules of reproductibility]({% post_url 2017-01-28-ten_rules_reproductible_research %}#software_archive), then I want to find a solution. And in the real life, I was already stuck because some package was missing (package not in CRAN)

## Speficifications

- Able to archive the software on the go (when added during analysis). The idea is to use a piece of software and forget. At a minimum, easy to pack everything when one considered the analysis have to be archived or distributed.
- Able to reuse the analysis years after. Target 3 years after (as titus said: there must be a half life)
- Platform agnostic
- FOSS is mandatory
- Take in account the fact the OS is himself a software and then a dependencie
- Relativly easy to use for a newcomer 
- Quick and easy to learn.
- Nothing to do for someone who want's to deploy the solution.
- Minimal size on disk. Storage is not so cheap, especially when you produce a lot of analysis
- Be able to access seemlessly to each file of the archive (via the standard file browser)
- Reliable

Extra: find an archive solution (online solution ?)

This is the ideal solution. We will see that it don't exists yet.

## Use case

1. Gather your data, use your softwares and other packages, make some analysis, produce reports
2. Pack everything 
2. Archive this in some secure place
3. Some time after (the day after or three year later), ask someone else to rerun everything or to reuse the analysis with similar data

In short `Make analysis` -> `Pack everything` -> `Archive` -> `Re-use`.

# Challengers

5 challengers will be describe here: 

1. Ignoring the threat
2. Manually archive
3. Packrat
4. Virtual machine
5. Docker

# Ignoring the threat: treat softwares archive scornfully

# Manually archive: the poor man's softwares archive

## Recipe

- Use `SessionInfo()` as [I suggested in my workflow]({% post_url 2017-01-21-implementation_basic_reproductible_workflow %})  .
- At the end of an analysis, download ALL the source package (not the binary), including the dependancies listed in ALL the SessionInfo()
- Archive the binaries from your library
- Download the binary version of R + source (just in case)
- Download the other external dependancies (e.g.: curl if there is some webscrapping, Latex if there is some PDF produced)
- Write some documentation to explain how to deploy again everything
- Pack everything and archive it in some safe places
- Some times after, try to rebuild everything, cross your finger and run everything. If a binary file can't be run on your actual platform, try to build the software from source.

Another way: rely on external archive. The CRAN is doing a tremedous job. It's possible to find any version of a package accepted by the CRAN. It's the same for R base.

But sometimes, I rely on packages not in CRAN. Most of these are archived in GitHub. Then, if the owner don't close it's repository, in theory, it must be possible to find again the source.

Pro:

- Easy to understand
- No dependancy to any software that will manage the archive
- Without own software archive (rely on CRAN, Github and other repo) minimal storage footprint

Con:

- Hard to archive manually every package in the good version
- Even harder to get all the dependencies manually (I alredy tried, it's painful)
- Not sure someone will be able to rebuild everything
- Need to write each time an ad-hoc documentation
- Too many manual steps
- No OS management 
- If rely on external software archive, risk that every piece of software in the good version will be not available when necessary


### Conclusion on poor man's softwares archive

To be frank with you, I never tried this solution. I just add some `SessionInfo()` and hope I will be able to rebuild the softwares dependencies from external repositories (CRAN and Github mainly). Just by describing this hypothetical manual local software archive in this post, implementing it seems to be just a loose of time because this solution is just an illusion of reproductibility. The balance reproductibility added over cost to implement is not favorable.  
If you have already this solution with some success, please let me know. For my part, I will not try it and will invest some time (and blog posts) on other solutions: packrat, Docker and virtual machines.

## **Packrat**: pragmatic but incomplete 

## **Virtual machine**: complete but heavy

## **Docker**: premium solution imply premium implementation price

# <a name="summary"></a> Summary

Criteria | Nothing | Manual | packrat | Docker | Virtual Machine |
:--------|:--------:|:-----:|:--------:|:-------:|:----------------:|
Ease to set up | +++ | ++ | +       | -      |- -               |
Ease to archive during data analysis    | +++ | +  | ++      | +      | +               |
Ease to reuse years after | - | +/- | ++ | + | + |
Archive R's packages | - - | + | + | + | + |
Archive R      | - - | +   | -       | ++     | ++ |
Archive external softwares | - - | +/- | - | ++ | ++ |
Archive OS     | - - | -   | -       | +      | ++ |
Time to learn to archive  | 0s | 2 min | 30 min | 2h | 1h |
Time to learn re-run the analysis | 0s to infinite | 5 min | 0 min if Rstudio | 5 min to hours | 10 min |
Ease of documentation to write to share it | ++ | + | + | +/- | + |
Archive softwares | - -| +  | +       | ++      | ++ (and OS)   |
Acquisition cost  | 0$ | 0$ |  0$    | 0$      | 0$ for Virtual Box, but high for good VM |
Size on disk | 0 | Minimal | Fair | High if external dependencies | Extra high |
Run on Linux and macOS  | + | + | + | + | + |
Run on Windows 7 | + | + | + | - | + |
Run without admin credential | + | + | + | - | +/- |

# Conclusion

Currently, I use packrat. It's a good treadoff between reproductibility and use time cost my multi-plaform work environment. For some specific projects, I plan to use Docker.
