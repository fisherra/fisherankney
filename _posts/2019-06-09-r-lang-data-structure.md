---
layout: post
title: "Data Structure in R"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<br>

A bit more confusing than the atomic vectors, data structures are built
on top of atomic vectors. can be categorized into homogenous (containing
all of the same atomic vector) and heterogeneous (containing different
types of atomic vectors).

First we’ll look at homogeneous.

<br>

Homogeneous Structures
-----------

<br>

### Matix

Just as you’d expect from linear algebra, matrices.

    matrix_dbl <- matrix(c(1, 1, 1, 0), nrow = 1)
    matrix_int <- matrix(c(1L,2L, 3L, 4L, nrow = 4))
    matrix_chr <- matrix(c('a', 'a', 'a', 'a'), ncol = 2)
    matrix_log <- matrix(c(T, T, T, F), nrow = 2, ncol = 2)

<br>

If you try to make it homogenous, coercion rules apply, see atomic
vector note.

    coerced_matrix <- matrix(c(1, T, 'Seven', 5L), nrow = 1)
    coerced_matrix

    ##      [,1] [,2]   [,3]    [,4]
    ## [1,] "1"  "TRUE" "Seven" "5"

<br>

Attributes are different depending on the function you apply to it.

Is both a matrix and numeric.

    is.matrix(matrix_dbl)

    ## [1] TRUE

    is.numeric(matrix_dbl)

    ## [1] TRUE

<br>

But this type of coercion wont work.

    matrix_dbl == matrix_log

    ## Error in matrix_dbl == matrix_log: non-conformable arrays

<br>

But this will

    sum(matrix_log) == sum(matrix_dbl)

    ## [1] TRUE

<br>

    dim(matrix_chr)

    ## [1] 2 2

<br>

    matrix_int[4,1]

    ## [1] 4

<br>

### Array

<br>

To me, these are the most confusing data structure in R. A multi
dimensional matrix. basically stacked matrices.

<br>

Notices homogeneous

    coerced_array <- array(c(matrix_dbl, matrix_int, matrix_log, matrix_chr))
    is.array(coerced_array)

    ## [1] TRUE

    typeof(coerced_array)

    ## [1] "character"

    coerced_array

    ##  [1] "1"     "1"     "1"     "0"     "1"     "2"     "3"     "4"    
    ##  [9] "4"     "TRUE"  "TRUE"  "TRUE"  "FALSE" "a"     "a"     "a"    
    ## [17] "a"

<br>

Multi-layer

    multi_layer <- array(rep(matrix_dbl, 4), dim = c(1,4,3))
    multi_layer

    ## , , 1
    ## 
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    1    1    0
    ## 
    ## , , 2
    ## 
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    1    1    0
    ## 
    ## , , 3
    ## 
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    1    1    0

<br>

pulling the first row, fourth column, 3rd ‘layer’

    multi_layer[1,4,3]

    ## [1] 0

<br>

as you can imagine, these get complex.

<br>

Hetrogeneous
------------

<br>

### List

<br>

a big of an ugly catch all, retains structures and types

All to a single element in the list

    single_list <- list(c(matrix_dbl, matrix_int, matrix_log, matrix_chr))
    is.list(single_list)

    ## [1] TRUE

    str(single_list)

    ## List of 1
    ##  $ : chr [1:17] "1" "1" "1" "0" ...

    single_list

    ## [[1]]
    ##  [1] "1"     "1"     "1"     "0"     "1"     "2"     "3"     "4"    
    ##  [9] "4"     "TRUE"  "TRUE"  "TRUE"  "FALSE" "a"     "a"     "a"    
    ## [17] "a"

<br>

    multi_list <- list(matrix_dbl, matrix_int, matrix_log, matrix_chr)
    is.list(multi_list)

    ## [1] TRUE

    str(multi_list)

    ## List of 4
    ##  $ : num [1, 1:4] 1 1 1 0
    ##  $ : num [1:5, 1] 1 2 3 4 4
    ##  $ : logi [1:2, 1:2] TRUE TRUE TRUE FALSE
    ##  $ : chr [1:2, 1:2] "a" "a" "a" "a"

    multi_list

    ## [[1]]
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    1    1    0
    ## 
    ## [[2]]
    ##      [,1]
    ## [1,]    1
    ## [2,]    2
    ## [3,]    3
    ## [4,]    4
    ## [5,]    4
    ## 
    ## [[3]]
    ##      [,1]  [,2]
    ## [1,] TRUE  TRUE
    ## [2,] TRUE FALSE
    ## 
    ## [[4]]
    ##      [,1] [,2]
    ## [1,] "a"  "a" 
    ## [2,] "a"  "a"

<br>

### Difference between \[\[\]\] and \[\]

when using a list and other things, you might use \[\[\]\] or \[\].
\[\[\]\] pulls the list itself, \[\] shows you whats inside.

see how \[\[\]\] works with a second set of indicies because now you’re
working with that specific matrix, while \[\] does not

    multi_list[[1]][1,2]

    ## [1] 1

    multi_list[1][1,2]

    ## Error in multi_list[1][1, 2]: incorrect number of dimensions

<br>

### Data Frame

most common data structure.

    df <- data.frame(a = 1:3,
                     b = c('chr_1', 'chr_2', 'chr_3'),
                     c = c(3L, 2L, 1L),
                     d = c(T,F,T))
    df

    ##   a     b c     d
    ## 1 1 chr_1 3  TRUE
    ## 2 2 chr_2 2 FALSE
    ## 3 3 chr_3 1  TRUE

<br>

renaming

    colnames(df) <- c('name_1', 'name_2', 'name_3', 'name_4')
    df

    ##   name_1 name_2 name_3 name_4
    ## 1      1  chr_1      3   TRUE
    ## 2      2  chr_2      2  FALSE
    ## 3      3  chr_3      1   TRUE

not advised to name the rows. use the indices as unique identifiers, if
they have names, make it a variable.

<br>

combining dataframes

column bind

    new_df <- data.frame(name_5 = c(5,5,5))

    df <- cbind(df, new_df)
    df

    ##   name_1 name_2 name_3 name_4 name_5
    ## 1      1  chr_1      3   TRUE      5
    ## 2      2  chr_2      2  FALSE      5
    ## 3      3  chr_3      1   TRUE      5

row bind - have more observations?

    new_row <- data.frame(name_1 = 4, 
                          name_2 = 'chr_4', 
                          name_3 = 0L, 
                          name_4 = F, 
                          name_5 = 5
                          )

    df <- rbind(df, new_row)
    df

    ##   name_1 name_2 name_3 name_4 name_5
    ## 1      1  chr_1      3   TRUE      5
    ## 2      2  chr_2      2  FALSE      5
    ## 3      3  chr_3      1   TRUE      5
    ## 4      4  chr_4      0  FALSE      5

access data frame

    df[1]

    ##   name_1
    ## 1      1
    ## 2      2
    ## 3      3
    ## 4      4

    df[[1]]

    ## [1] 1 2 3 4

    df$name_1

    ## [1] 1 2 3 4

<br>

Alternatively, use tibbles!

That's all for now,

\- Fisher

<br>
<br>
