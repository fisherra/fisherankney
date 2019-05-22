---
layout: post
title: "Reading and writing delimited files"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---

<hr> 

Perhaps the most versatile file type, .csv files are ubiquitous. Reading
and writing comma-separated value files is often the first and last step
of an analysis in R.


It is advantageous to use the readr library (a part of the tidyverse) to
read and write .csv files for several reasons. Readr functions use more
consistent naming schemes, offer the user more intuitive parsing, and
are much faster than base R. Readr also automatically loads your data as
a tibble. If you're looking for more speed, it may be useful to check out the
data.table package instead.

<br>

    library('readr')

<br>

Reading Files
-------------
<hr>

#### reading a .csv file

<br>

    my_data <- read_csv('mydata.csv')

<br>

In its most simple form, this may get the job done. It can be helpful to
use R Studio's Import Dataset GUI to quickly access the secondary
arguments of `read_csv`.

<br>

#### reading custom delimiters

<br>

    read_delim(
      "mydata.csv",               # file path and name always comes first
      delim = ",",                # single character field separator
      quote = "\"",               # single character to quote strings
      comment = "#",              # single character to signal comments
      col_names = c('a','b'),     # custom name columns on import
      na = ".",                   # string to signify missing values
      skip = 0,                   # number of lines to skip before read
      progress = show_progress()  # display a progress bar
      )

<br>

To read tab or semicolon delimited files, use these arguments with the
`read_delim` function. Again, it's probably best to use the R Studio GUI
if you have access to it. This can be useful when you're working with
tab separated files, or European data (semicolons).

<br>

#### reading zipped files

<br>

    my_data <- read_csv('mydata.csv.zip')

<br>

Compressed data files are treated just like uncompressed files.

<br>

#### reading files outside of your directory

<br>

    my_data <- read_csv('~/local/path/to/my/file/mydata.csv')
    my_data <- read_csv('https://github.com/tidyverse/readr/raw/master/inst/extdata/mtcars.csv')

<br>

This is useful for projects that have an input folder that is not apart
of your analysis workspace.

<br>

Writing Files
-------------

<hr> 

<br>

#### writing a .csv

<br>

    write_csv(
      data_name, 
      'path/to/filename.csv'
    )

<br>

Remember that you must type the name of the object in your R environment
before you give the argument to the path and file name you're saving.

<br>

#### writing custom delimiters

<br>

    write_delim(
      data_name,                
      'path/to/filename.tsv'    
      delim = '\t'              
    )

<br>

The function `write_delim` can also be used to write a .csv files if you
set the delim arguement to ','. In this case, the tibble will be saved as
a tab separated file.

<br>

#### appending to an existing .csv

<br>

    write_csv(
      your_object,
      'path/to/existing/filename.csv',
      append = T
    )

<br>

Be careful not to append the column names as an observation.

<br> <br>
