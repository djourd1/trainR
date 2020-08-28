---
title: "Subsetting rows" 
author: Damien Jourdain
date: '2020-03-02'
slug: subsetrows
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

## Learning objectives
Often when you want to understand your data or exclude some individuals that are outside the scope of the analysis you want to conduct, you will need to select subsamples of your data set.

In this section, you will learn how to take a subset of a data frame. This will be our first encounter with the package `dplyr` and the use of the pipe for chaining commands. This is an important section, as you will be using this kind of syntax for all the subsequent types of data manipulations.  

At the end of the section, you should be able to select specific rows of a data set that will be selected by their row number, a logical criteria based on the different variables describing the rows, or randomly.  


We will work with the fish data that we just imported from datasets.xlsx file


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



To make things more apparent when we subset rows, I will add a fishID field that will be a unique integer per row.

We can do this using the following command:

```r
fish$ID <- 1:nrow(fish)
```

We have several ways to subset rows.

## Subsetting rows by their indices

A first way to sub-set rows, is to indicate directly the list of rows you want to see. 


```r
# this will select the first five rows
fish[1:5, ]
```

```
## # A tibble: 5 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species    ID
##          <dbl>       <dbl>        <dbl>       <dbl> <chr>   <int>
## 1          5.1         3.5          1.4         0.2 setosa      1
## 2          4.9         3            1.4         0.2 setosa      2
## 3          4.7         3.2          1.3         0.2 setosa      3
## 4          4.6         3.1          1.5         0.2 setosa      4
## 5          5           3.6          1.4         0.2 setosa      5
```

A more interesting application is the creation of a random subsample of cases (i.e., rows).
To do this we can rely on the function `sample` as follows


```r
# first create a sample of row IDs using the function sample()
rows_selected <- sample(1:nrow(fish), 10)

# then create a new data frame containing only the randomly selected rows
fish_sample <- fish[rows_selected, ]
fish_sample
```

```
## # A tibble: 10 x 6
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species       ID
##           <dbl>       <dbl>        <dbl>       <dbl> <chr>      <int>
##  1          5.7         3            4.2         1.2 versicolor    96
##  2          5.4         3            4.5         1.5 versicolor    85
##  3          5.8         2.7          3.9         1.2 versicolor    83
##  4          6.8         2.8          4.8         1.4 versicolor    77
##  5          6.7         2.5          5.8         1.8 virginica    109
##  6          6           2.2          4           1   versicolor    63
##  7          6.9         3.2          5.7         2.3 virginica    121
##  8          4.9         3.6          1.4         0.1 setosa        38
##  9          5.7         2.8          4.5         1.3 versicolor    56
## 10          7.2         3            5.8         1.6 virginica    130
```

## Subsetting rows based on logical criteria

Let say, you want to look only at fish species 'virginica'. To do this we will start to use the `dplyr` syntax. Note that `dplyr` is already loaded if you loaded the package `tidyverse`. 

The command to subset rows is `filter()`


```r
virg <- filter(fish, Species=="virginica")
head(virg)  #the function head selects the first 6 rows of a data frame
```

```
## # A tibble: 6 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species      ID
##          <dbl>       <dbl>        <dbl>       <dbl> <chr>     <int>
## 1          6.3         3.3          6           2.5 virginica   101
## 2          5.8         2.7          5.1         1.9 virginica   102
## 3          7.1         3            5.9         2.1 virginica   103
## 4          6.3         2.9          5.6         1.8 virginica   104
## 5          6.5         3            5.8         2.2 virginica   105
## 6          7.6         3            6.6         2.1 virginica   106
```

Note that all `dplyr` related commands work similarly:

+ The first argument is a data frame.
+ The subsequent arguments describe what to do with the data frame, using the variable names (**without quotes**).
+ The result is a new data frame.

You can create a more complicated criteria, by using the logical operators. Logical operators “&” (and), “|” (or), and “!” (negation) were briefly introduced {{% staticref "docs/data_structures/basic-data-types/#logicals" %}}Basic data types --> Logicals{{% /staticref %}} section. 

In the following example, we are selecting virginica fishes with petal length greater than 6.


```r
virg6 <- filter(fish, Species=="virginica" & Petal.Length >=6)
head(virg6,3)
```

```
## # A tibble: 3 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species      ID
##          <dbl>       <dbl>        <dbl>       <dbl> <chr>     <int>
## 1          6.3         3.3          6           2.5 virginica   101
## 2          7.6         3            6.6         2.1 virginica   106
## 3          7.3         2.9          6.3         1.8 virginica   108
```

## Chaining commands and the use of pipes

A very convenient feature of tidyverse is the possibility of chaining different commands. The operator to chain the different command is the pipe `%>%`

To understand how it works, we can reproduce the precedent example using the pipe. 


```r
fish %>% filter(Species=="virginica" & Petal.Length >=6) %>% slice_head(n=3)
```

```
## # A tibble: 3 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species      ID
##          <dbl>       <dbl>        <dbl>       <dbl> <chr>     <int>
## 1          6.3         3.3          6           2.5 virginica   101
## 2          7.6         3            6.6         2.1 virginica   106
## 3          7.3         2.9          6.3         1.8 virginica   108
```

How did that work:

You need to read this chain of command from left to right, and think that the results of the previous command are passed as the first argument of the next command. 

Here the argument fish (the data frame) is passed on as the first argument of the command filter(...). After this step, you have a subset of the initial dataframe, and this subsetted dataframe is passed on to the next commande slice_head(n=3).

You can explore what the `slice_head` command is doing using `? slice_head`. You will see it is part of a "slice" family that helps you select the first n, last n, etc.  rows of a data set. (so this is a dplyr equivalent of the head() function with have been using earlier).

An alternative way to write this without the pipes would be:


```r
fish2 <- filter(fish, Species=="virginica" & Petal.Length >=6)  # create a new dataframe names fish2
slice_head(fish2, n=3)
```

```
## # A tibble: 3 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species      ID
##          <dbl>       <dbl>        <dbl>       <dbl> <chr>     <int>
## 1          6.3         3.3          6           2.5 virginica   101
## 2          7.6         3            6.6         2.1 virginica   106
## 3          7.3         2.9          6.3         1.8 virginica   108
```

However, it requires the creation of an intermediate data set. Note also that in this case, you need to give the first argument to these functions. 

Yet another alternative to obtain the same result could be:


```r
slice_head(filter(fish, Species=="virginica" & Petal.Length >=6),n=3) 
```

```
## # A tibble: 3 x 6
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species      ID
##          <dbl>       <dbl>        <dbl>       <dbl> <chr>     <int>
## 1          6.3         3.3          6           2.5 virginica   101
## 2          7.6         3            6.6         2.1 virginica   106
## 3          7.3         2.9          6.3         1.8 virginica   108
```

However, this second writing becomes rapidly difficult to write and read, as it is difficult to identify the sequence of commands, and arguments of the different functions get easily mixed up.

So the pipe system is now very popular among R data scientists, as it does not require the creation of intermediate objects, and it helps identify quickly the different steps used to transform your data. I encourage you to learn and practice it as much as you can! 


Note that the command has not changed the initial data set `fish`. It will only display the subset.

```r
fish %>% filter(Species=="virginica" & Petal.Length >=6) %>% slice_head(n=3)
```

If you want to save the subset of fishes for later use, you will need to assign it to a new named object, for example:


```r
virg6 <- fish %>% filter(Species=="virginica" & Petal.Length >=6) %>% slice_head(n=3)
```

Finally, remember that R either prints out the results, or saves them to a variable. If you want to do both, you can wrap the assignment in parentheses:

```r
(virg6 <- fish %>% filter(Species=="virginica" & Petal.Length >=6) %>% slice_head(n=3))
```

## This is just the start !

{{% alert note %}}

There are plenty of excellent and very detailed resources and tutorials for the tidyverse and dplyr packages. 
The best place to start is the <a href="https://r4ds.had.co.nz/transform.html" target="_blank">Data transformation chapter in the R for data science online book</a>. It provides a thorough, clear and detailed information about data manipulation. 

Note that dplyr have seen some important updates in the last few years. For systematic and updated references about the `dplyr` package, you can follow the <a href="https://dplyr.tidyverse.org/" target="_blank">package website</a>

Finally, as it might be a bit difficult to remember or understand the syntax, as usual, do not hesitate to browse the internet. Usually the problem you are trying to solve has already been discussed in one of the many developers fora like  <a href="https://stackoverflow.com/questions/tagged/dplyr" target="_blank"> Stackoverflow </a>. 

As a start, when faced with a problem or an error I cannot solve, I usually use Google!

{{% /alert %}}


## Exercise

Now that you understand the basics of the command `filter` and of the `slice_`  family of commands and the `%>%` operator, try to write a code using `filter` and `slice_sample` commands to obtain a random sample of size 10 of the fish data set.
Then write a line of code to obtain a random sample of size 3 of the setosa fishes. 

