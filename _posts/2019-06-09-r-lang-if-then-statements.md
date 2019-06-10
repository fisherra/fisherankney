---
layout: post
title: "If-Then Statements in R"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<hr>


<br>

if statement
------------

    x <- 5

    if (x > 0) {
      print('X is positive')
    }

    ## [1] "X is positive"

<br>

if else
-------

    if (x < 0) {
      print('X is negative')
    } else {
      print('X is not negative')
    }

    ## [1] "X is not negative"

<br>

if else if
----------

    if (x < 0) {
      print('X is negative')
    } else if (x == 0) {
      print('X is zero')
    } else {
      print('X is positive')
    }

    ## [1] "X is positive"
