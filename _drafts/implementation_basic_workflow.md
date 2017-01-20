---
layout: post
title: "Implementation of a basic reproducible data analysis workflow"
categories:
    - R
    - r-bloggers 
    - data science
    - reproducible analysis
---

[In a previous post]({% post_url 2017-01-19-reproducible_analysis_my_principles %}), I described the principles of my basic reproducible data analysis workflow. Today, let's be more pratical and see how to implement it.

Be noted that is a basic workflow. The goal is to find a good balance between a minimal reproducible analysis and the ease to deploy it on any platform.

This workflow will allow you to run a complete analysis based on multiple files (data files, R script, Rmd files...) just by launching a single R script.

I consider you have already base R and git installed. 

# Organisation

Most important is the organisation of the project. I understand by *project* a folder containing every file necessary to run the analysis: raw data, intermediate data, scripts and others assets (pictures, xml...).

My organisation is:

```
name_of_the_project
|-- .git # created by git init
|-- .gitignore
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
    |-- 02-descriptive.html
    |-- 02-model1.html
|-- rmds
    |-- import_data.Rmd
    |-- descriptive.Rmd
    |-- model1.Rmd
|-- rscripts
    |-- complicated_model.R
|-- name_of_the_project.Rproj # not mandatory but useful
```

As you can observe, there some principles :

- Directory names are as explicit as possible to be understandable by anyone getting these files.
- Reports don't belong to the script, because I don't produce a report for every script or Rmd (there is child Rmd) and this way, it's straighforward where people should look the results (reports).
- Reports are sorted using 2 digits numbers. Data science is also an art to tell stories in the right order.
- I use *rmarkdown* files to produce my report, to have both the results and my comments. These comments are fundamental, this is a appreciation of the data scientist.
- Everything live in a subfolder exept `run_all.R` (detail above).

This `run_all.R` script, as it name explicity tell run everything necessary to produce all the reports. Here an example:

```r
source("functions/make_reports.R")

default_open_file <- TRUE

report("rmds/import_data.Rmd", n_file = "1")
report("rmds/descriptive.Rmd", "2")
report("rmds/model1.Rmd", "3")
````

`make_report.R` is optional. It's  an help function to produce the report, set their name.

```r
rm(list = ls())
library(knitr)
library(rmarkdown)
opts_knit$set(root.dir = '../.')

report <- function(file, n_file = "", open_file = default_open_file) {

  base_name <- sub(pattern = ".Rmd", replacement = "", x = basename(file))

  # Make nfiles with always 2 digits
  n_file <- ifelse(as.integer(n_file) < 10, paste0("0", n_file), n_file)

  file_name <- paste0(n_file, "-", base_name, ".html")

  report_dir <- "reports"

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

  if(open_file) {
    result_path <- file.path(report_dir, file_name)
    system(command = paste("firefox", result_path))
  }

}
```

# summary

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

In a next post, I will list the possible extensions of this workflow.
