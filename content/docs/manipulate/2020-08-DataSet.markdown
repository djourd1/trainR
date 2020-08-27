---
title: The data set
author: Damien Jourdain
date: '2020-08-20'
slug: tidydataset
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
weight: 2

---

In this chapter, we will work with the datasets made available by the tidyverse package. 

If you haven't done it yet during the 'Import data' section of Chapter 2, download the file [datasets.xlsx here](/files/datasets.xlsx). Save it on your disk. 

Load the readxl package to import data from excel files.

```r
library(readxl)
```

Then import the first worksheet of the datasets.xlsx file and save it in a tibble that we will name fish.


```r
fish <- read_excel("mypath/datasets.xlsx")
```




If everyting went ok you should obtain this:

```r
fish
```

```
## # A tibble: 150 x 5
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>  
##  1          5.1         3.5          1.4         0.2 setosa 
##  2          4.9         3            1.4         0.2 setosa 
##  3          4.7         3.2          1.3         0.2 setosa 
##  4          4.6         3.1          1.5         0.2 setosa 
##  5          5           3.6          1.4         0.2 setosa 
##  6          5.4         3.9          1.7         0.4 setosa 
##  7          4.6         3.4          1.4         0.3 setosa 
##  8          5           3.4          1.5         0.2 setosa 
##  9          4.4         2.9          1.4         0.2 setosa 
## 10          4.9         3.1          1.5         0.1 setosa 
## # ... with 140 more rows
```

In the next sections we will rely a lot of the set of tidyverse packages. 

If you haven't yet done so, install the package tidyverse. Remember you only have to do this once.

```r
install.packages("tidyverse")
```

Load the tidyverse set of packages. 

```r
library(tidyverse)
```

```
## -- Attaching packages ------------------------------------------------------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.2     v purrr   0.3.4
## v tibble  3.0.3     v dplyr   1.0.1
## v tidyr   1.1.1     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.5.0
```

```
## -- Conflicts ---------------------------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```
You will notice that you have in fact installed a set of packages like tibble. 


We are ready to work on the data.
