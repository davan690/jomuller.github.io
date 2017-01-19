---
layout: post
title:  "Chloropleth map of France"
categories: 
    - R 
    - Data science
    - Geographical data
    - Graphical representation 
    - r-bloggers
---



A popular way to represent data on a map is the [cloropleth map](http://en.wikipedia.org/wiki/Choropleth_map). It's possible to produce this kind of map with [_R_](http://www.r-project.org/) but I never took the time to achieve it. I'm actually working on some report system and I have to represent geographical data about France. One constraint is I have to produce a printable and grey tones map, ready to be pusblished. Furthermore, the cloropleth map must use a french specific territorial unit: the [department](http://en.wikipedia.org/wiki/Department_%28country_subdivision%29).

The aim of this post is to create a simple but nice chloropleth map with R of the French population to the administrative department level.

## Gather Data

I will use data from the [French institute of Statistics (INSEE)](http://insee.fr). We will use the [file about the population by deparment](http://insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/estim-pop/estim-pop-dep-sexe-gca-1975-2014.xls). 


```r
# Download the raw XLS file
raw_xls <- file.path(tempdir(), "pop_french.xls")
download.file(
    url = "http://insee.fr/fr/ppp/bases-de-donnees/donnees-detaillees/estim-pop/estim-pop-dep-sexe-gca-1975-2014.xls", 
    destfile = raw_xls, 
    method = "wget"
    )
```



The file is an ugly XLS, then we will use the package _xlsx_ to open it.


```r
library(xlsx)
# Data of 2014 are on sheet one
raw_db <- read.xlsx(
    file = raw_xls, 
    sheetIndex = 2, 
    header = FALSE, 
    colClasses = "character"
    )
dim(raw_db)
```

```
## [1] 110  20
```

```r
head(raw_db[, 1:10])
```

```
##                                                                                      X1
## 1 Estimation de population au 1er janvier, par département, sexe et grande classe d'âge
## 2                                                                            Année 2014
## 3                                                                                  <NA>
## 4                                                                          Départements
## 5                                                                                  <NA>
## 6                                                                                    01
##     X2         X3          X4          X5          X6             X7
## 1 <NA>       <NA>        <NA>        <NA>        <NA>           <NA>
## 2 <NA>       <NA>        <NA>        <NA>        <NA>           <NA>
## 3 <NA>       <NA>        <NA>        <NA>        <NA>           <NA>
## 4 <NA>   Ensemble        <NA>        <NA>        <NA>           <NA>
## 5 <NA> 0 à 19 ans 20 à 39 ans 40 à 59 ans 60 à 74 ans 75 ans et plus
## 6  Ain     165152      148149      176421       88795          48888
##       X8         X9         X10
## 1   <NA>       <NA>        <NA>
## 2   <NA>       <NA>        <NA>
## 3   <NA>       <NA>        <NA>
## 4   <NA>     Hommes        <NA>
## 5  Total 0 à 19 ans 20 à 39 ans
## 6 627405      84519       74901
```

```r
tail(raw_db[, 1:10])
```

```
##                                                                                       X1
## 105                                                                                  973
## 106                                                                                  974
## 107                                                                                  DOM
## 108                                                         France métropolitaine et DOM
## 109 Source : Insee - Estimations de population (résultats provisoires arrêtés fin 2014).
## 110                                                                                 <NA>
##             X2       X3       X4       X5      X6      X7       X8      X9
## 105     Guyane   106360    72397    52584   14811    4225   250377   54368
## 106 La Réunion   267789   219852   235187   88002   34164   844994  136826
## 107       <NA>   579184   450600   529026  220906  100731  1880447  294888
## 108       <NA> 16171587 16001614 17709879 9941571 5976043 65800694 8270176
## 109       <NA>     <NA>     <NA>     <NA>    <NA>    <NA>     <NA>    <NA>
## 110       <NA>     <NA>     <NA>     <NA>    <NA>    <NA>     <NA>    <NA>
##         X10
## 105   35384
## 106  103706
## 107  209578
## 108 7939835
## 109    <NA>
## 110    <NA>
```

It's really an ugly tabular data because header are not on the first line and data after. Then we will need to clean it.

We only need 3 columns :

- _1_: Department's number
- _2_: Department's name
- _8_: Total Population

Data start at line 6 and end at line 106.


```r
clean_data <- raw_db[6:106, c(1,2,8)]
dim(clean_data)
```

```
## [1] 101   3
```

```r
# Check the end of the data
tail(clean_data)
```

```
##                         X1          X2       X8
## 101                     95  Val-d'Oise  1199207
## 102 France métropolitaine         <NA> 63920247
## 103                    971 Guadeloupe    403750
## 104                    972 Martinique    381326
## 105                    973      Guyane   250377
## 106                    974  La Réunion   844994
```

Line 102 is just a sum then delete it.


```r
clean_data <- clean_data[complete.cases(clean_data),]
```

Do a better table


```r
db <- data.frame(
    dept = as.character(clean_data$X1),
    pop = as.integer(as.character(clean_data$X8)),
    row.names = clean_data$X2,
    stringsAsFactors = FALSE
    )
```

Finaly we have a clean data frame


|                        |dept |     pop|
|:-----------------------|:----|-------:|
|Ain                     |01   |  627405|
|Aisne                   |02   |  540409|
|Allier                  |03   |  342593|
|Alpes-de-Haute-Provence |04   |  162438|
|Hautes-Alpes            |05   |  141911|
|Alpes-Maritimes         |06   | 1083268|
|Ardèche                 |07   |  321252|
|Ardennes                |08   |  281987|
|Ariège                  |09   |  152944|
|Aube                    |10   |  306490|
|Aude                    |11   |  367158|
|Aveyron                 |12   |  275063|
|Bouches-du-Rhône        |13   | 1996351|
|Calvados                |14   |  690836|
|Cantal                  |15   |  146504|
|Charente                |16   |  354801|
|Charente-Maritime       |17   |  635191|
|Cher                    |18   |  312052|
|Corrèze                 |19   |  239555|
|Corse-du-Sud            |2A   |  148022|
|Haute-Corse             |2B   |  175070|
|Côte-d'Or               |21   |  528970|
|Côtes-d'Armor           |22   |  599477|
|Creuse                  |23   |  120156|
|Dordogne                |24   |  418566|
|Doubs                   |25   |  534229|
|Drôme                   |26   |  496601|
|Eure                    |27   |  596574|
|Eure-et-Loir            |28   |  435834|
|Finistère               |29   |  904999|
|Gard                    |30   |  740660|
|Haute-Garonne           |31   | 1312022|
|Gers                    |32   |  190943|
|Gironde                 |33   | 1515229|
|Hérault                 |34   | 1107730|
|Ille-et-Vilaine         |35   | 1026962|
|Indre                   |36   |  225993|
|Indre-et-Loire          |37   |  602025|
|Isère                   |38   | 1242280|
|Jura                    |39   |  260280|
|Landes                  |40   |  401458|
|Loir-et-Cher            |41   |  333758|
|Loire                   |42   |  758203|
|Haute-Loire             |43   |  226963|
|Loire-Atlantique        |44   | 1343259|
|Loiret                  |45   |  667812|
|Lot                     |46   |  174810|
|Lot-et-Garonne          |47   |  333182|
|Lozère                  |48   |   76543|
|Maine-et-Loire          |49   |  804860|
|Manche                  |50   |  499860|
|Marne                   |51   |  569789|
|Haute-Marne             |52   |  179856|
|Mayenne                 |53   |  308521|
|Meurthe-et-Moselle      |54   |  734002|
|Meuse                   |55   |  191696|
|Morbihan                |56   |  741905|
|Moselle                 |57   | 1046237|
|Nièvre                  |58   |  214303|
|Nord                    |59   | 2595539|
|Oise                    |60   |  815517|
|Orne                    |61   |  287515|
|Pas-de-Calais           |62   | 1462793|
|Puy-de-Dôme             |63   |  643342|
|Pyrénées-Atlantiques    |64   |  666699|
|Hautes-Pyrénées         |65   |  227926|
|Pyrénées-Orientales     |66   |  465467|
|Bas-Rhin                |67   | 1110416|
|Haut-Rhin               |68   |  758357|
|Rhône                   |69   | 1798511|
|Haute-Saône             |70   |  239828|
|Saône-et-Loire          |71   |  554505|
|Sarthe                  |72   |  570419|
|Savoie                  |73   |  427313|
|Haute-Savoie            |74   |  777356|
|Paris                   |75   | 2241346|
|Seine-Maritime          |76   | 1255335|
|Seine-et-Marne          |77   | 1380030|
|Yvelines                |78   | 1414931|
|Deux-Sèvres             |79   |  374383|
|Somme                   |80   |  571461|
|Tarn                    |81   |  381872|
|Tarn-et-Garonne         |82   |  251573|
|Var                     |83   | 1030489|
|Vaucluse                |84   |  550402|
|Vendée                  |85   |  662406|
|Vienne                  |86   |  432059|
|Haute-Vienne            |87   |  376169|
|Vosges                  |88   |  374357|
|Yonne                   |89   |  340714|
|Territoire de Belfort   |90   |  144600|
|Essonne                 |91   | 1257141|
|Hauts-de-Seine          |92   | 1601583|
|Seine-Saint-Denis       |93   | 1554166|
|Val-de-Marne            |94   | 1356673|
|Val-d'Oise              |95   | 1199207|
|Guadeloupe              |971  |  403750|
|Martinique              |972  |  381326|
|Guyane                  |973  |  250377|
|La Réunion              |974  |  844994|

## Find a map of France

According to this [post](http://web.stanford.edu/~cengel/cgi-bin/anthrospace/download-global-administrative-areas-as-rdata-files), maps can be downloaded from [Global Administrative Areas (GADM)](http://gadm.org).




```r
con <- url("http://biogeo.ucdavis.edu/data/gadm2/R/FRA_adm2.RData")
```

```r
print(load(con))
```

```
## [1] "gadm"
```


```r
close(con)
```

Now we have a new `gadm` object in our environment.

## Try the map

With this, it's possible to plot an empty map just to check if we have the good file.


```r
library(sp)
plot(gadm)
```

<img src="/assets/france_map/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" style="display: block; margin: auto;" />

## Merge map data and pop data

Now we have to put together the data from the map and data about population


```r
str(gadm, max.level = 2)
```

```
## Formal class 'SpatialPolygonsDataFrame' [package "sp"] with 5 slots
##   ..@ data       :'data.frame':	96 obs. of  12 variables:
##   ..@ polygons   :List of 96
##   ..@ plotOrder  : int [1:96] 57 56 13 58 15 34 62 83 36 75 ...
##   ..@ bbox       : num [1:2, 1:2] -5.14 41.33 9.56 51.09
##   .. ..- attr(*, "dimnames")=List of 2
##   ..@ proj4string:Formal class 'CRS' [package "sp"] with 1 slot
```



```r
gadm$regions <- as.factor(gadm@data$NAME_1)
colorss <- rainbow(length(levels(gadm$regions)))
spplot(gadm, "regions", col.regions = colorss)
```

<img src="/assets/france_map/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" style="display: block; margin: auto;" />

Now, do the same but with popultation


```r
dbs <- merge(
    x = gadm@data, 
    y = db, 
    by.x = "ID_2",
    by.y = "dept", 
    all.x = TRUE,
    all.y = FALSE
    )

# Discretize

gadm$pop <- cut(x = dbs$pop, breaks = 6)
color_pop <- heat.colors(6)

spplot(gadm, "pop", col.regions = color_pop)
```

<img src="/assets/france_map/unnamed-chunk-10-1.png" title="plot of chunk unnamed-chunk-10" alt="plot of chunk unnamed-chunk-10" style="display: block; margin: auto;" />

Not bad but there is some department missing. To reasons :


1. The _id_ of the department in the INSEE data are with two digits and represented by _01_, _02_ ... Then we have to convert it in integer
2. Corsica have _id_s in INSEE _2a_ and _2b_ but 96 and 97 in map data.


```r
# Change for Corsica
db$dept[db$dept == "2a"] <- 96 
db$dept[db$dept == "2b"] <- 97 
db$dept <- as.integer(db$dept)

# Redo previous step
gadm$pop <- NULL
dbs <- merge(
    x = gadm@data, 
    y = db, 
    by.x = "ID_2",
    by.y = "dept", 
    all.x = TRUE,
    all.y = FALSE
    )

# Discretize

gadm$pop <- cut(x = dbs$pop, breaks = 6)
color_pop <- rev(heat.colors(6))

spplot(gadm, "pop", col.regions = color_pop)
```

<img src="/assets/france_map/unnamed-chunk-11-1.png" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" style="display: block; margin: auto;" />
