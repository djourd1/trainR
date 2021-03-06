---
title: "Sorting rows" 
author: Damien Jourdain
date: '2020-03-02'
slug: sortrows
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
weight: 3
---




You are now familiar with `dplyr` commands and the pipe syntax.

A second type of manipulation is the ordering of your data. For this, we will use the command `arrange()`.

The command `arrange()` changes the order of the rows. Beside the name of the dataset, the argument is a set of column names to order by. 


```r
fish %>% arrange(Species)
```

```
## # A tibble: 150 x 6
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species    ID
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>   <int>
##  1          5.1         3.5          1.4         0.2 setosa      1
##  2          4.9         3            1.4         0.2 setosa      2
##  3          4.7         3.2          1.3         0.2 setosa      3
##  4          4.6         3.1          1.5         0.2 setosa      4
##  5          5           3.6          1.4         0.2 setosa      5
##  6          5.4         3.9          1.7         0.4 setosa      6
##  7          4.6         3.4          1.4         0.3 setosa      7
##  8          5           3.4          1.5         0.2 setosa      8
##  9          4.4         2.9          1.4         0.2 setosa      9
## 10          4.9         3.1          1.5         0.1 setosa     10
## # ... with 140 more rows
```

If you provide more than one column name, the column names are separated with commas.Each additional column will be used to break ties in the values of preceding columns. 


```r
fish %>% arrange(Species, Petal.Width)
```

```
## # A tibble: 150 x 6
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species    ID
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>   <int>
##  1          4.9         3.1          1.5         0.1 setosa     10
##  2          4.8         3            1.4         0.1 setosa     13
##  3          4.3         3            1.1         0.1 setosa     14
##  4          5.2         4.1          1.5         0.1 setosa     33
##  5          4.9         3.6          1.4         0.1 setosa     38
##  6          5.1         3.5          1.4         0.2 setosa      1
##  7          4.9         3            1.4         0.2 setosa      2
##  8          4.7         3.2          1.3         0.2 setosa      3
##  9          4.6         3.1          1.5         0.2 setosa      4
## 10          5           3.6          1.4         0.2 setosa      5
## # ... with 140 more rows
```

Use desc() to re-order by a column in descending order:


```r
fish %>% arrange(desc(Species), Petal.Width)
```

```
## # A tibble: 150 x 6
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species      ID
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>     <int>
##  1          6.1         2.6          5.6         1.4 virginica   135
##  2          6           2.2          5           1.5 virginica   120
##  3          6.3         2.8          5.1         1.5 virginica   134
##  4          7.2         3            5.8         1.6 virginica   130
##  5          4.9         2.5          4.5         1.7 virginica   107
##  6          6.3         2.9          5.6         1.8 virginica   104
##  7          7.3         2.9          6.3         1.8 virginica   108
##  8          6.7         2.5          5.8         1.8 virginica   109
##  9          6.5         3            5.5         1.8 virginica   117
## 10          6.3         2.7          4.9         1.8 virginica   124
## # ... with 140 more rows
```

Just to get familiar with the pipes, you can take a random sample of size 15 of the fish data set and then order it by the Petal.Width. For equal Petal.Width, order it by Species.


```r
fish %>% 
  slice_sample(n=15) %>%
  arrange(Petal.Width, Species)
```

```
## # A tibble: 15 x 6
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species       ID
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>      <int>
##  1          4.8         3            1.4         0.1 setosa        13
##  2          4.9         3.6          1.4         0.1 setosa        38
##  3          5.2         4.1          1.5         0.1 setosa        33
##  4          4.6         3.6          1           0.2 setosa        23
##  5          5.1         3.8          1.6         0.2 setosa        47
##  6          4.8         3.4          1.9         0.2 setosa        25
##  7          5           3.5          1.6         0.6 setosa        44
##  8          5.7         2.6          3.5         1   versicolor    80
##  9          5.5         2.4          3.8         1.1 versicolor    81
## 10          5.9         3            5.1         1.8 virginica    150
## 11          5.8         2.7          5.1         1.9 virginica    143
## 12          7.4         2.8          6.1         1.9 virginica    131
## 13          7.6         3            6.6         2.1 virginica    106
## 14          6.7         3            5.2         2.3 virginica    146
## 15          6.2         3.4          5.4         2.3 virginica    149
```

