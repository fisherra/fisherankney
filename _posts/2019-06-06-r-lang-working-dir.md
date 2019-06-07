---
layout: post
title: "Working Directories and R"
categories:
  - Blog Posts
top_tags: General
tags:
- Data Toolbox
---




<hr>

R Files and Interface
---------------------

It’s essential to understand how directories work. Here’s how you view
your directory directly from R (similar to pwd).

<br>

    getwd()

<br>

Here’s how you set your working directory from R.

    setwd('your/directory/here')


<br>

R Markdown Files
----------------

These can be more tricky. Knitr switches your working directory back
after each chunk is run, unless you set the root.dir using knitr
options. Here’s proof that just using `setwd()` doesn’t work. This is
ran from inside an .rmd file.

<br>

In the first chunk:
```
getwd()
setwd('/Users/fisher/Documents/')
getwd()
```

```
## [1] "/Users/fisher/Documents/data_science/r_workflow/import_export"
## [2] "/Users/fisher/Documents"
```

<br>

In the second chunk:

```
getwd()
```

```   
## [1] "/Users/fisher/Documents/data_science/r_workflow/import_export"
```

<br>

Instead we must use this knitr option:

In the first chunk:

    knitr::opts_knit$set(root.dir = normalizePath("/Users/fisher/")) 
    knitr::opts_knit$get("root.dir")  

    ## [1] "/Users/fisher"

<br> 

In the second chunk:

    getwd()

    ## [1] "/Users/fisher"

<br>
<br>


That’s all for now!

<br> 
<br>

\- Fisher

<br>
<br>
