---
layout: post
title: "Implementation of a basic reproducible data analysis workflow"
categories:
    - R
    - r-bloggers 
    - data science
    - reproducible analysis
---

[In a previous post]({% post_url 2017-01-19-reproducible_analysis_my_principles %}), I described the principles of my basic reproducible data analysis workflow. Today, let's be more practical and see how to implement it.

Be noted that it is a basic workflow. The goal is to find a good balance between a minimal reproducible analysis and the ease to deploy it on any platform.

This workflow will allow you to run a complete analysis based on multiple files (data files, R script, Rmd files...) just by launching a single R script.

# Summary

There are 3 main components in this workflow:

1. Sofwares: 
    - [R](https://www.r-project.org/) of course.
    - [Rstudio IDE](https://www.rstudio.com/products/rstudio/) optionnaly. It could save you installation time because it's come with both *Pandoc* and *rmarkdown* built-in.
    - If not using *Rstudio IDE*: 
        - [rmarkdown R's package](https://github.com/rstudio/rmarkdown), to convert R markdown files to markdown, html and PDF. Just type in R `install.packages("rmarkdown")`. 
        - a recent version of [Pandoc](https://github.com/rstudio/rmarkdown/blob/master/PANDOC.md) to process markdown files.
    - [Git](https://git-scm.com/) for control version.
2. [Files organisation](#file_orga) (see above).
3. [One R script to rule them all](#run_all) (see above).

# <a name="file_orga"></a>Files organisation

Most important is the organisation of the project. I understand by *project* a folder containing every file necessary to run the analysis: raw data, intermediate data, scripts and other assets (pictures, xml...).

My organisation is:

```
name_of_the_project
|-- assets
|-- functions
    |-- import_html_helper.R
    |-- make_report.R
|-- plot
|-- produced_data
    |-- imported.rds
    |-- model_result.rds
|-- run_all.R     # *** Most important file ***
|-- raw_data
    |-- FinalDataV3.xlsx
    |-- SomeExternalData.csv
    |-- NomenclatureFromWeb.html
|-- reports
    |-- 01-import_data.html
    |-- 02-data_tidying.html
    |-- 03-descriptive.html
    |-- 04-model1.html
    |-- 99-sysinfo.html
|-- rmds
    |-- import_data.Rmd
    |-- data_tidying.Rmd
    |-- descriptive.Rmd
    |-- model1.Rmd
    |-- sysinfo.Rmd
|-- rscripts
    |-- complicated_model.R
|-- name_of_the_project.Rproj # not mandatory but useful
```

As you can observe, there are some principles :

- Directory names are as explicit as possible to be understandable by anyone getting these files.
- Reports and the script or Rmarkdown files are not in the same directory. I don't produce a report for every script or Rmd (there is child Rmd) and this way, it's straightforward where people should look at the results (reports).
- Reports are sorted using 2-digit numbers. Data science is also an art to tell stories in the right order.
- I use *rmarkdown* files to produce my report, to have both the results and my comments. These comments are fundamental, this is an appreciation of the results by the data scientist.
- A rmarkdown file, *sysinfo.Rmd*, will be used to produce a report keeping trace of the name and version of R's package used (with `sessionInfo()`) and some extra information about the OS (`Sys.info()`). In an ideal workflow, these commands have to be called at the end of each report.
- Everything lives in a subfolder except `run_all.R` (detail above).

# <a name="run_all"></a> One R script to run them all

This `run_all.R` script, as it name explicitly tell, it runs everything necessary to produce all the reports. Here an example:

```r
source("functions/make_reports.R")

report("rmds/import_data.Rmd", n_file = "1")
report("rmds/data_tidying.Rmd", "2")
report("rmds/descriptive.Rmd", "3")
report("rmds/model1.Rmd", "4")
````

It's straightforward: one line by rmarkdown files to process.

`make_report.R` is optional. It's  an help function to produce the report, set their name.

```r
# Clean up the environment
rm(list = ls())

# Load the libraries
library(knitr)
library(rmarkdown)

# Set the root dir because my rmds live in rmds/ subfolder
opts_knit$set(root.dir = '../.')

# By default, don't open the report at the end of processing
default_open_file <- FALSE

# Main function
report <- function(file, n_file = "", open_file = default_open_file,  
                   report_dir = "reports") {

  ### Set the name of the report file ###
  base_name <- sub(pattern = ".Rmd", replacement = "", x = basename(file))

  # Make nfiles with always 2 digits
  n_file <- ifelse(as.integer(n_file) < 10, paste0("0", n_file), n_file)

  file_name <- paste0(n_file, "-", base_name, ".html")
  
  ### Render ###
  render(
    input = file,
    output_format = html_document(
      toc = TRUE,
      toc_depth = 1,
      code_folding = "hide"
    ),
    output_file = file_name,
    output_dir = report_dir,
    envir = new.env()
    )

  ### Under macOS, open the report file  ###
  ### in firefox at the end of rendering ###
  if(open_file & Sys.info()[1] == "Darwin") {
    result_path <- file.path(report_dir, file_name)
    system(command = paste("firefox", result_path))
  }

}
```

# Usage

The core of this workflow is the [rmarkdown files](http://rmarkdown.rstudio.com). Most of the time, I write *.Rmd* files, mixing comments about what I'm going to do, code, results and comment about these results.

If there is a heavy computation (*e.g.* models or simulations), I write a script in R and save the results in a *.rds* file. Often I use remote machine for this kind of computation. Then I insert an R script in a Rmd chunk not evaluated and load the results in an evaluated one.

```
 ## ```{r heavy_computation, eval=FALSE}
 ## source("rscript/complicated_model.R")
 ## ```
 ## ```{r load_results}
 ## mod_results <- readRDS("model_result.rds")
 ## ```
```

Because my *.Rmd* don't live in the root directory, I add a setup chunk on all these files. This way, I'm to process my *.Rmd* directly or even use [Rstudio's notebook feature](http://rmarkdown.rstudio.com/r_notebooks.html).

```
 ## ```{r setup, include=FALSE}
 ## knitr::opts_knit$set(root.dir = '../.')
 ## ```
```

When I have to rebuild all the reports (*e.g.* due to some raw data changes), I just run the script `run_all.R`.

# Benefits in real life

This basic workflow works for me because: 

- It's pragmatic: it fulfils all the [constraints and goals I fixed]({% post_url 2017-01-19-reproducible_analysis_my_principles %}) without any bells or whistles.
- It uses mainstream tools. Others can easily use it and these tools are not likely to be deprecated in the next decade.
- It's easy to implement and deploy.
- It's straightforward: peoples who want just to see the reports, know where to see, those who want to reproduce the analysis, just have to use the `run_all.R` file.

# Limits

There are more layers needed for a perfect data analysis reproducibility (as [described in this article](http://journals.plos.org/ploscompbiol/article/file?id=10.1371/journal.pcbi.1003285&type=printable)). The main weakness of my workflow is the lack of back up for the good version of the software I use (R' packages included). I already wasn't able rerun an old data analysis due to change in the package used. (*e.g.* deprecated functions in dplyr or disappearance of a package in the CRAN). To improve this, I already try to add [*packrat*](https://cran.r-project.org/web/packages/packrat/index.html) to my workflow but it's been a long time ago and the package wasn't stable enough for my day to day work. Resolution for 2017: give another try to *packrat*!

Other improvements possible: run the scripts in a [docker container](https://www.docker.com/) or in a virtual machine. But this adds too much overhead in my daily work. Furthermore, it brake the platform agnostic and simplicity principles.

# Conclusion

Currently, I'm happy with this workflow. It covers my needs of my daily data analysis reproducibility with few extra work.

In a later post, I will discuss about these possible extension of this basic workflow.
