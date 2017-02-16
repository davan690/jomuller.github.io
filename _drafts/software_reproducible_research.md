---
layout: post
title: How to archive software for reproducible data analysis?
categories: 
    - R 
    - Reproductible research
---

# Challengers

## Nothing

## Manual archive

## packrat

## Docker

## Virtual machine

# Biblio

- Titus post
- Docker on r-blogger
- packrat on r-blogger

# Table summary

Criteria | nothing | .zip | packrat | Docker | Virtual Machine |
--------:|:--------|:-----|:--------|:-------|:----------------|
Ease to set up | +++ | ++ | +       | -      |- -               |
Ease to use    | +++ | +  | ++      | +      | +               |
Ease to reuse years after | +/- | + | ++ (if archiving packrat) | + | + |
Archive R      | - - | +   | -       | ++     | ++ |
Archive OS     | - - | -   | -       | +      | ++ |
Archive non-R dependancies | - - | +/- | - | ++ | ++ |
Time to learn to use it | 0s | 2 min | 30 min | 2h | 1h |
Ease of documentation to write to share it | ++ | + | + | +/- | + |
Archive softwares | - -| +  | +       | ++      | ++ (and OS)   |
Cost              | 0$ | 0$ |  0$    | 0$      | 0$ for Virtual Box, but high for good VM |
Size on disk | 0 | Minimal | 
Platform agnostic | +++ | ++ | ++ | +/- | + (depend of the VM)|

