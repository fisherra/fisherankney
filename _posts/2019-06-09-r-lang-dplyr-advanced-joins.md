---
layout: post
title: "Dplyr - Advanced Joins"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<br>

Load Libraries
--------------

    library('tidyverse')
    library('dplyr')

<br>

Prepare Example Data
--------------------

    tib_1 <- tibble(x = 1:2, y = 2:1)
    tib_1

    ## # A tibble: 2 x 2
    ##       x     y
    ##   <int> <int>
    ## 1     1     2
    ## 2     2     1

    tib_2 <- tibble(x = c(1,3), a = 10, b = "a")
    tib_2

    ## # A tibble: 2 x 3
    ##       x     a b    
    ##   <dbl> <dbl> <chr>
    ## 1     1    10 a    
    ## 2     3    10 a

<br>

#### Full Join

The most inclusive join of all, `full_join()` joins every observation
from both tables, as the name suggests.

    full_join(tib_1, tib_2, by = "x")

    ## # A tibble: 3 x 4
    ##       x     y     a b    
    ##   <dbl> <int> <dbl> <chr>
    ## 1     1     2    10 a    
    ## 2     2     1    NA <NA> 
    ## 3     3    NA    10 a

<br>

#### Semi Join

The first of two filter joins, `semi_join()`, keeps all observations in
the `x` argument that have a match in the `y` argument. In this example,
the key is the “x” column of `tib_1`; the only row that matches in
`tib_2` is the observation x = 1. Thus, the resulting semi\_join only
has one observation filtered from `tib_1`.

    semi_join(tib_1, tib_2, by = "x")

    ## # A tibble: 1 x 2
    ##       x     y
    ##   <int> <int>
    ## 1     1     2

<br>

#### Anti Join

`anti_join()` is the antithesis of `semi_join()`, the function only
returns observations that are not present in the `y` dataset.

    anti_join(tib_1, tib_2)

    ## Joining, by = "x"

    ## # A tibble: 1 x 2
    ##       x     y
    ##   <int> <int>
    ## 1     2     1

<br>

