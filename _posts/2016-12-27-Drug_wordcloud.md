---
layout: post
title:  "Start with wordcloud"
date: 2016-12-27 
categories:
    - Data-Science
    - R
    - r-bloggers
    - Pet project
    - Datavisualisation
---


<script src="/assets/2016-12-27-Drug_wordcloud_files/htmlwidgets-0.7/htmlwidgets.js"></script>
<link href="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-0.0.1/wordcloud.css" rel="stylesheet">
<script src="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-0.0.1/wordcloud2-all.js"></script>
<script src="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-0.0.1/hover.js"></script>
<script src="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-binding-0.2.0/wordcloud2.js"></script>

I followed my good resolutions on practising data analysis in [my previous post](http://blog.jom.link/data_science_pet_project.html) and started to play with the [French drug database](https://github.com/jomuller/explore_drug_database).

After importing the data, I started classically with data visualisation. In this database, there is a lot of text data. To visualise this, some wordcloud is always welcome. They are maybe not accurate at all but are from my point of view a very good illustration of a text-based dataset.

To my knowledge there are two main wordcloud packages in R :

- [*wordcloud*](https://cran.rstudio.com/web/packages/wordcloud/index.html): produce plot
- [*wordcloud2*](https://cran.rstudio.com/web/packages/wordcloud2/index.html): produce html widget

Let's play with this.

# Prepare the data


```r
# Read the previously imported data
db <- readRDS("raw_rds/bdpm.rds")
```

For example, there is the **pharmaceutical form** column.


```r
head(db$forme)
```

```
## [1] "pommade"                                                 
## [2] "capsule molle"                                           
## [3] "solution injectable"                                     
## [4] "solution injectable"                                     
## [5] "suspension à diluer pour perfusion"                      
## [6] "poudre et pommade et comprimé et granules et solution(s)"
```

There is a lot of different forms


```r
uforme <- unique(db$forme)
length(uforme)
```

```
## [1] 405
```

405 various form. But there is multiple form in one line sometimes, separated by "et". Try to find the real different form.


```r
forms <- db$forme %>%
  strsplit(split = " et ") %>%
  unlist() 
  
length(unique(forms))  
```

```
## [1] 393
```

```r
head(forms)
```

```
## [1] "pommade"                           
## [2] "capsule molle"                     
## [3] "solution injectable"               
## [4] "solution injectable"               
## [5] "suspension à diluer pour perfusion"
## [6] "poudre"
```

Select the 100 most frequent


```r
head(as.data.frame(table(forms)))
```

```
##                                   forms Freq
## 1                              comprimé   17
## 2                       comprimé enrobé   19
## 3                    comprimé pelliculé   36
## 4  comprimé pelliculé buvable pelliculé    1
## 5          comprimé pelliculé pelliculé    1
## 6                                 crème   17
```

```r
cent <- forms %>%
  table() %>%
  as.data.frame() %>%
  arrange(desc(Freq)) %>%
  head(100)

kable(head(cent))
```

name                    Freq
--------------------  -----
comprimé pelliculé     2289
comprimé               1782
gélule                 1019
solution injectable     934
poudre                  886
comprimé sécable        852


# Wordcloud

Make a wordcloud with *wordcloud*


```r
library(wordcloud)
wordcloud(cent$., freq = cent$Freq)
```

![](/assets/2016-12-27-Drug_wordcloud_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

Not bad. Try something funkier.


```r
wordcloud(
  words = cent$., 
  freq = cent$Freq, 
  random.color = T, 
  random.order = F, 
  colors = brewer.pal(8,"Dark2")
)
```

![](/assets/2016-12-27-Drug_wordcloud_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

I find this very informative. Intituively it's possible to see what's the most frequent forms are. And is far more attractive than a table or an unreadable barplot.


```r
library(ggplot2)

ggplot(cent) +
  aes(x = ., y = Freq) +
  geom_bar(stat = "identity") +
  coord_flip()
```

![](/assets/2016-12-27-Drug_wordcloud_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

OK, I would be possible to make a better plot but I think you see the point.

# Wordcloud 2

Wordcloud 2 produce html widget


```r
library(wordcloud2)
wordcloud2(cent)
```

<!--html_preserve--><div id="htmlwidget-ddabb23c8052f6c567e9" style="width:672px;height:480px;" class="wordcloud2 html-widget"></div>
<script type="application/json" data-for="htmlwidget-ddabb23c8052f6c567e9">{"x":{"word":["comprimé pelliculé","comprimé","gélule","solution injectable","poudre","comprimé sécable","comprimé pelliculé sécable","granules","pommade","solution(s)","solution buvable","solution pour perfusion","comprimé enrobé"," solvant pour solution injectable","gélule à libération prolongée","comprimé orodispersible","solution à diluer pour perfusion","crème","poudre pour solution injectable","gélule gastro-résistant(e)","poudre pour suspension buvable","gel","comprimé gastro-résistant(e)","collyre en solution","dispositif","comprimé à libération prolongée","sirop","solution pour application","comprimé pelliculé à libération prolongée","solution","comprimé effervescent(e)"," solution en gouttes en gouttes","poudre pour solution pour perfusion","poudre pour solution buvable","suspension injectable","suppositoire","capsule molle","suspension buvable","collyre","solution injectable pour perfusion","comprimé à croquer","comprimé dispersible","solution pour pulvérisation","solution buvable en gouttes","comprimé quadrisécable","comprimé à sucer","gaz pour inhalation","comprimé effervescent(e) sécable","lyophilisat","granulés pour solution buvable","poudre pour solution à diluer pour perfusion"," poudre","pastille"," comprimé pelliculé","poudre pour solution injectable ou pour perfusion","solution pour inhalation par nébuliseur","poudre pour inhalation","granulés","granulés pour suspension buvable","gel pour application","solution injectable pour usage dentaire","solution pour bain de bouche","solution pour inhalation","vernis à ongles médicamenteux(se)","comprimé à libération modifiée"," solution pour perfusion","comprimé pelliculé sécable à libération prolongée","solution buvable gouttes","solution injectable ou pour perfusion","solution pour dialyse péritonéale"," solution pour usage parentéral"," solvant pour suspension injectable à libération prolongée","comprimé dispersible sécable","comprimé enrobé à libération prolongée","suspension pour inhalation par nébuliseur"," comprimé enrobé","poudre pour application","suspension pour pulvérisation","comprimé dispersible ou à croquer","comprimé pelliculé à libération modifiée","gomme à mâcher médicamenteux(se)","poudre pour inhalation en gélule"," comprimé"," crème"," solution pour dialyse péritonéale"," solvant pour suspension injectable","émulsion pour perfusion","comprimé enrobé gastro-résistant(e)","comprimé sécable à libération prolongée"," solution","capsule","collutoire","solution pour usage dentaire"," solvant pour solution pour perfusion","lyophilisat pour usage parentéral","poudre pour solution injectable pour perfusion"," solvant pour solution injectable ou pour perfusion","émulsion pour application","ovule à libération prolongée","ovule"],"freq":[2289,1782,1019,934,886,852,764,654,586,506,302,277,257,235,216,197,191,185,185,171,167,160,153,148,147,142,133,123,116,116,112,104,103,95,87,85,84,77,75,67,62,60,51,48,47,44,44,41,40,39,38,37,37,36,36,35,33,32,32,31,30,29,27,27,26,25,22,22,22,22,21,21,21,21,20,19,19,19,18,18,18,18,17,17,17,17,17,16,16,15,15,15,15,14,14,14,13,13,13,12],"fontFamily":"Segoe UI","fontWeight":"bold","color":"random-dark","minSize":0,"weightFactor":0.0786369593709043,"backgroundColor":"white","gridSize":0,"minRotation":-0.785398163397448,"maxRotation":0.785398163397448,"shuffle":true,"rotateRatio":0.4,"shape":"circle","ellipticity":0.65,"figBase64":null,"hover":null},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

It's easier and more fun! Try it, it's interactive.

Note : if you want to add this widget to a page, you need to link the proper javascript files. In my case I put this in my markdown file:

```
<script src="/assets/2016-12-27-Drug_wordcloud_files/htmlwidgets-0.7/htmlwidgets.js"></script>
<link href="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-0.0.1/wordcloud.css" rel="stylesheet">
<script src="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-0.0.1/wordcloud2-all.js"></script>
<script src="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-0.0.1/hover.js"></script>
<script src="/assets/2016-12-27-Drug_wordcloud_files/wordcloud2-binding-0.2.0/wordcloud2.js"></script>
```

Enough with wordcloud. We understood that's the "comprimé" (tablet) pharmaceutical form is the most frequent, followed by the "gélule" (capsule), "poudre" (powder) and "granule" (small pill). We can also see that's some text cleaning would be necessary to make a proper analysis.

