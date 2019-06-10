---
layout: post
title: "Custom Functons in R"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<hr>

<br>









<br>

    my_function <- function(arguement_1, arguement_2 = 'default', ...) {
      results = paste(arguement_1, arguement_2)
      print(results)
    }

<br>

    my_function('yes', 'no')

    ## [1] "yes no"

    my_function('yes')

    ## [1] "yes default"

