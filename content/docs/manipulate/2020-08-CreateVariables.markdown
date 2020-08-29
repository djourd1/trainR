---
title: "Create Variables" 
author: Damien Jourdain
date: '2020-08-02'
slug: createvariables
output: 
  bookdown::pdf_document2:
    fig_caption: yes
    toc: yes
  bookdown::html_document2:
    fig_caption: yes
categories:
  - R
tags: []
type: book
weight: 5
---



Surveys provides raw data. Often you will need to combine information collected with different variables into a new variable. So you will need to add new columns that are functions of existing columns. 

`mutate()` adds new columns at the end of the table. 


```r
fish %>% 
  select(ID, contains("Petal")) %>%
  mutate(ratioPetals = Petal.Width / Petal.Length)
```

```
## # A tibble: 3 x 4
##      ID Petal.Length Petal.Width ratioPetals
##   <int>        <dbl>       <dbl>       <dbl>
## 1     1          1.4         0.2       0.143
## 2     2          1.4         0.2       0.143
## 3     3          1.3         0.2       0.154
```


Arithmetic operators can also be very useful. For example, `x / sum(x)` calculates the proportion of a total, and `y - mean(y)` computes the difference from the mean.


```r
fish %>% 
  select(ID, contains("Petal")) %>%
  mutate(diffMeansPetalWidth = Petal.Length - mean(Petal.Length) )
```

```
## # A tibble: 3 x 4
##      ID Petal.Length Petal.Width diffMeansPetalWidth
##   <int>        <dbl>       <dbl>               <dbl>
## 1     1          1.4         0.2              0.0333
## 2     2          1.4         0.2              0.0333
## 3     3          1.3         0.2             -0.0667
```
