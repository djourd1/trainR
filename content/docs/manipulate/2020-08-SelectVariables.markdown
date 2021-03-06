---
title: "Select Variables" 
author: Damien Jourdain
date: '2020-08-02'
slug: selectvariables
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
weight: 4
---




When you have a large survey with many variables, it will often be useful to extract the specific variables useful for a particular type of analysis. The function `select()` will allow you do create a subset of variables based on their names.

## Selecting columns

A first approach is to list all the names of the variables you want to select

```r
fish %>% select(Species, Petal.Length, Petal.Width)
```

```
## # A tibble: 3 x 3
##   Species Petal.Length Petal.Width
##   <chr>          <dbl>       <dbl>
## 1 setosa           1.4         0.2
## 2 setosa           1.4         0.2
## 3 setosa           1.3         0.2
```

However, it can become rapidly cumbersome to have to write a long list of variable names. Hopefully, you can use a few tricks.

### Select by position
First, you can provide the column positions instead 


```r
fish %>% select(1, 3, 2) 
```

```
## # A tibble: 3 x 3
##   Sepal.Length Petal.Length Sepal.Width
##          <dbl>        <dbl>       <dbl>
## 1          5.1          1.4         3.5
## 2          4.9          1.4         3  
## 3          4.7          1.3         3.2
```

### Select ranges
Second, you can provide a range of variables or a range of column positions


```r
# This will select of the variables between the column Sepal.Length and Petal.Width
fish %>% select(Sepal.Length:Petal.Width)
```

```
## # A tibble: 3 x 4
##   Sepal.Length Sepal.Width Petal.Length Petal.Width
##          <dbl>       <dbl>        <dbl>       <dbl>
## 1          5.1         3.5          1.4         0.2
## 2          4.9         3            1.4         0.2
## 3          4.7         3.2          1.3         0.2
```

### Exclude instead of selecting
Third, if you need to select most variables and exclude only a few variables you can use the sign `-` to signal the variables you want to exclude:


```r
# This will exclude the variable Petal.Width
fish %>% select(-Petal.Width)
```

```
## # A tibble: 3 x 5
##   Sepal.Length Sepal.Width Petal.Length Species    ID
##          <dbl>       <dbl>        <dbl> <chr>   <int>
## 1          5.1         3.5          1.4 setosa      1
## 2          4.9         3            1.4 setosa      2
## 3          4.7         3.2          1.3 setosa      3
```

### Select by type

Fourth, you can select by type of variables or any other characteristic of the variable using `where()`. where() applies a function to all variables and selects those for which the function returns TRUE.


```r
# will select only the numeric variables
fish %>% select(where(is.numeric))
```

```
## # A tibble: 3 x 5
##   Sepal.Length Sepal.Width Petal.Length Petal.Width    ID
##          <dbl>       <dbl>        <dbl>       <dbl> <int>
## 1          5.1         3.5          1.4         0.2     1
## 2          4.9         3            1.4         0.2     2
## 3          4.7         3.2          1.3         0.2     3
```


### Use additional helpers on variable names
To make your life easier, you can also use a number of helper function within select

```r
# Select of the variables with names starting with P, and the variable ID
fish %>% select(starts_with("P"), ID)
```

```
## # A tibble: 3 x 3
##   Petal.Length Petal.Width    ID
##          <dbl>       <dbl> <int>
## 1          1.4         0.2     1
## 2          1.4         0.2     2
## 3          1.3         0.2     3
```


```r
# Select of the variables with names containing Width, and the variable ID
fish %>% select(contains("Width"), ID)
```

```
## # A tibble: 3 x 3
##   Sepal.Width Petal.Width    ID
##         <dbl>       <dbl> <int>
## 1         3.5         0.2     1
## 2         3           0.2     2
## 3         3.2         0.2     3
```


```r
# Presented for reference only, we do not have variables x1, x2, x3!
# Select of variables x1, x2 and x3
fish %>% select(num_range("x", 1:3))

# Select of x3, x4, x1 in that order
fish %>% select(num_range("x", c(3,4,1)))
```


## Reordering columns

Sometime, you just want to re-order the variables. Indeed you can do this with select, but it can still be cumbersome if you need to move only a few among many colmuns.

In such case you can use the function `relocate()`. 

The default behaviour corresponds to the case where you want to move some variables to the front:


```r
# this will make the ID and Species variable as the first colum
fish %>% relocate(ID, Species)
```

```
## # A tibble: 3 x 6
##      ID Species Sepal.Length Sepal.Width Petal.Length Petal.Width
##   <int> <chr>          <dbl>       <dbl>        <dbl>       <dbl>
## 1     1 setosa           5.1         3.5          1.4         0.2
## 2     2 setosa           4.9         3            1.4         0.2
## 3     3 setosa           4.7         3.2          1.3         0.2
```

Use the function helper if you need more sophisticated changes.
