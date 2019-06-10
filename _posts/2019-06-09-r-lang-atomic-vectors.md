---
layout: post
title: "Atomic Vectors"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---


<br>

### Double

    double <- c(1, 0.1, 1.0, 1.01)
    is.double(double)

    ## [1] TRUE

<br>

### Integer

    integer <- c(1L, 500L, 0L)
    is.integer(integer)

    ## [1] TRUE

<br>

### logical

    logical <- c(T, TRUE, F, FALSE)
    is.logical(logical)

    ## [1] TRUE

<br>

### Character

    character <- c('string_1', "string 2", '345')
    is.character(character)

    ## [1] TRUE

<br>

Coercion Rules
--------------

<br>

coercion based on which retains the most information.

<br>

### logical to numeric (double or integer)

    mixed <- c(T, 1L)
    typeof(mixed)

    ## [1] "integer"

<br>

### integer to double

    mixed <- c(1, 2L)
    typeof(mixed)

    ## [1] "double"

<br>

### Everything to character

    mixed_1 <- c('a', 1)
    mixed_2 <- c(T, 'a')
    mixed_3 <- c('a', 1L)

    typeof(mixed_1)

    ## [1] "character"

    typeof(mixed_2)

    ## [1] "character"

    typeof(mixed_3)

    ## [1] "character"

<br>

Forcing Coersion
----------------

some functions will force coersion on there own, helpful for counting

    example_log <- c(T, T, F)
    example_num <- as.numeric(example_log)

    example_num

    ## [1] 1 1 0

    sum(example_log)

    ## [1] 2

    mean(example_log)

    ## [1] 0.6666667

<br>


