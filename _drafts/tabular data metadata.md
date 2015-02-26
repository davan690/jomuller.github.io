---
layout: post
title:  "Tabular data metadata"
date:  2015-02-20 
categories: R data_science
---

## Introduction

Importing raw data to any software is always painfull. If you import a .CSV file, you have only the data, i.e. some column names and a value for each observation. You have no idea about the _type_ of each variable: quantitative, nominal, date... and when you are able to guess it, most of the time you don't have a mainingful title for each variable. For example, the variable _age_ mean what? Age of the child (in days, months, years?), or the age of his mother?

Example of a non explicit table:

age | study | childs
----|-------|-------
1   | 3     | 0
4   | 2     | 2
3   |       | 9

It tried to find a solution with the package _vartors_. It works but the theorical part is weak. The main leak I think is the vocabulary. I will discuss here about the concept of __metadata__

## Metadata

When one data scientist want to send some data to another one, he have to send two files: the data (mostly in .CSV) and something to explain what are these data. This is this second file I want to discuss here.

There is, to my knowledge, no normalized way to send this information. Each time one have to write a _variable definition_ file where he describes variable by variable, the real name, the meaning, the type, the labels of each modality. Then the other data scientist have to read each of it definition and write a script in his software to gather all this information. It's _janitor work_. It's painfull and cost a lot of time. It's error prone but essential.

## Specifications

- Describe each variable
- Describe general information of the database : encoding, decimal point, separator, format, how NA are written, a general description of the table.
- Text format (durability)

## A solution

YAML metadata file

## Glossary

- _Metadata_: information that describes data. Should describe a whole table or only a variable
- _Tabular data_: Data presented in a table with one observation by line, one column by variable.
