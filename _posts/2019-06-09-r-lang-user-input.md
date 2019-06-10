---
layout: post
title: "User Input in R"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<hr>

<br>

### readlines function

sometimes you want your function / script to take user input, use
readline() function

    readline(prompt="Enter something to print: ")

In the console -

    Enter something to print: this is my input, print it
    [1] "this is my input, print it"

<br>

### using readlines in a function

    paste_function <- function(arg_1) {
      arg_2 <- readline(prompt= "Enter what you want pasted: ")
      return <- paste(arg_1, arg_2)
      print(return)
    }

    paste_function('hello')

In the console -

    Enter what you want pasted: , how are you?
    [1] "hello, how are you?"

<br>

### interactive file choice

    file.choose()

brings up your finder to browse through and select, often data, to as
input. great to use in a script you want to make easily accessable for
others. be aware that as you create more complex scripts based on this
data, often your restricting the input formatting, describe this in the
script.

This script would return the first 5 rows of a dataset of your choice if
ran.

    data <- file.choose()
    ?head
    head(data, n = 5)

<br>

### source

you can run an R script without even opening it using source. do this
from within R, often helpful if you open R in the terminal, or want to
source a file as apart of another program.

    source('path/to/my/script.R')

The script is ran

In the console -

    [1] output of your script here

and objects are saved into your environment that were created by this
script.

