---
layout: post
title: "National Park Biodiversity with ggplot2"
categories:
  - Blog
tags:
- R
- data visualization
- ggplot2
- biostatistics
- ecology
---
This is a simple exploratory data analysis on the U.S. National Park Biodiversity dataset, sourced from [kaggle](https://www.kaggle.com/nationalparkservice/park-biodiversity/data). I've been using R to investigate scientific problems for a while now, but I'm just starting to use the ggplot2 data visualization package. This is also the my first personal data science project with R! I plan on further exploring the biodiversity dataset in the future, experimenting with other graphing packages such as shiny and plotly as I do so. Stay tuned for more exciting biodiversity visualizations! 

This analysis exlores three simple questions about U.S. National Park Biodiversity:

1. Where are the 58 National Parks, and how big are they?
2. Generally, which parks have the most and least plant and animal diversity?
3. Is there any relationship between a park's size, and it's biodiversity?


### Setup 

    library('tidyverse')
    library('RColorBrewer')
    library('maps')

Hadley Wickham's tidyverse includes ggplot2, tidyr, and few other great packages to make data science in R easy. I recommend checking out his book [R for Data Science](http://r4ds.had.co.nz/) to learn more. RColorBrewer provides pleasing color palettes, and maps allows for geospatial plots. 


    # setwd("~/Documents/data_science/np_biodiversity")
    parks <- read_csv("input/parks.csv")
    species <- read_csv("input/species.csv")

Here I'm reading in the biodiversity dataset, again the dataset can be downloaded for free [here](https://www.kaggle.com/nationalparkservice/park-biodiversity/data). 

<br  />
### Park Location and Size

    parks_mapped <- parks %>%
      ggplot(aes(Longitude, Latitude)) +
      borders("state") +
      geom_point(aes(size=Acres)) +
      coord_quickmap()
    parks_mapped

![]({{ site.baseurl }}/images/np_biodiversity/ggplot-1.png)

This is just a quick peak at the data. I create a new variable called parks_mapped, first copying the parks dataframe into it, then (%>%) calling a ggplot function. The mappings for this function are Longitude on the X axis, Latitude on the Y axis. Calling the build in borders("states") allows for a map to be projected around these points; unfortunately the borders function doesn't include Alaska and Hawaii. Next the points are plotted with their size according to how large the parks are in square acres. Coordinates are set to retain the maps ratio, and the parks\_map variable finally plotted.  

Let's cut out Alaska and Hawaii, and spruce up this visualization a little bit.

    lower_48_mapped <- parks %>% 
      filter(State != "AK" & State != "HI") %>% 
      ggplot(aes(Longitude, Latitude)) +
      borders("state") +
      geom_point(
        aes(
          size=Acres, 
          color=Acres
        )
      ) +
      coord_quickmap() +   
      labs(title="Continental U.S. National Parks",               
           subtitle="Size and Location",
           caption="source: Kaggle biodiversity dataset"
      ) + 
      scale_size_continuous(
        range=c(3,9),
        guide=FALSE) + 
      scale_colour_distiller(name = "Square Acres",
                             breaks=c(1e+06, 2e+06, 3e+06, 4e+06),
                             labels=c("1 Million", "2 Million", "3 Million", "4 Million"),
                             palette="Spectral")
    lower_48_mapped

![]({{ site.baseurl}}/images/np_biodiversity/ggplot-2.png)

To create this map, first filtered out the national parks in Alaska and Hawaii. Then use the same visualization recipe as before, but this time I added in that the color of the points should also be determined by the size of the parks. Next add labels and spruce up the legend and color pallete, then plot the new visualization. 

From this visualization it is evident that most national parks are under 1 million square acres in size. The majority of national parks are found west of the Mississippi River, clumping around Utah, California, and Colorado.

Which states have the most national parks?

    # Create a table of number of national parks per state

    parks_by_state <- parks %>%
      group_by(State) %>%
      summarise(num_of_parks = n()) %>% 
      arrange(desc(num_of_parks)) %>%
      ungroup(parks_by_state)

    # Create a graph to show the same: 
    
    ggplot(parks_by_state) + 
      geom_bar(aes(fct_reorder(State, num_of_parks), num_of_parks),
               stat="identity",
               width = 0.8,                      
               color = "black",                   
               fill = "darkgreen",             
               alpha = 0.8                       
               ) +
      labs(title="National Parks",                          
           subtitle="States Ranked by of National Parks", 
           caption="source: Kaggle biodiversity datset", 
           y = "Number of National Parks",
           x = "State"
      ) + 
      coord_flip()

![]({{ site.baseurl}}/images/np_biodiversity/ggplot-3.png)

Alaska earns the title of "state with the most national parks", or does it? If you look closely California appears twice in the graph, once near the top with seven national parks, but again near the bottom combined with Nevada. California has a grand total of 8 national parks, sharing the top spot with Alaska. It appears twice because in the dataset, the variable \`State\` lists all of the states the national park is in. If the park is exclusively found in california, \`State\` returns CA, if the park is found in more than one state, as is the case with Death Valley, it'll return both states: CA, NV. Yellowstone and Great Smokey Mountain national parks both return multiple states as well. 


<br  />

### Plant Biodiversity Within the Parks

<br  />

    # Only include species that are present within the parks
    # Also remove "national park" from each park name, it's redundant

    species_cleaned <- species %>%
      filter(Occurrence == "Present") %>%
      mutate(park_name = str_replace(`Park Name`, " Na.*", ""))


    # Only include species that are algae, fungi, and plants

    plant_biodiv <- species_cleaned %>%
      filter(Category == "Algae" | Category == "Fungi" | Category == "Nonvascular Plant" | Category == "Vascular Plant") %>%
      group_by(park_name) %>%
      mutate(total_div = n())


    # plot the plant biodiversity as a bar chart

    ggplot(plant_biodiv) + 
      geom_bar(
        mapping = aes(x = fct_reorder(park_name, total_div), fill = Category),
        width = 0.8,                       
        color = "black",                 
        alpha = 0.8
      ) + 
      labs(title="Plant Biodiversity Within U.S. National Parks",             
           caption="Source: Kaggle biodiversity dataset", 
           x = "National Park",
           y = "Number of Species"
      ) + 
      coord_flip()


![]({{ site.baseurl}}/images/np_biodiversity/ggplot-4.png)

Wow, there's a lot going on here! First and foremost, the majority of every national park's plant biodiversity is vascular plants; things like flowers, ferns, trees and shrubs. 

<br  />

Animal Biodiversity
===================

<br  />

    animal_biodiv <- species_cleaned %>%
      filter(Category == "Amphibian" | Category == "Reptile" | Category == "Bird" | Category == "Fish" | Category ==  "Mammal") %>%
      group_by(park_name) %>%
      mutate(total_div = n())

    ggplot(animal_biodiv) + 
      geom_bar(
        mapping = aes(x = fct_reorder(park_name, total_div), fill = Category), 
        width = 0.8,                       
        alpha = 0.8
      ) + 
      labs(title="Animal Biodiversity Within U.S. National Parks",                         
           caption="Source: Kaggle biodiversity dataset", 
           x = "National Park",
           y = "Number of Species"
      ) + coord_flip()

![]({{ site.baseurl}}/images/np_biodiversity/unnamed-chunk-6-1.png)


Plant Biodiversity and National Park Size
=========================================

    plant_biodiv_mapped <- parks %>%
      left_join(plant_biodiv, by = "Park Name")

    plant_biodiv_mapped <- plant_biodiv_mapped %>%
      filter(State != "AK" & State != "HI") %>% 
      ggplot(aes(Longitude, Latitude)) +
      borders("state") +
      geom_point(
        aes(
          size=total_div, 
          color=total_div
        )
      ) +
      coord_quickmap() +   
      labs(title="U.S. National Parks",               
           subtitle="Plant Biodiversity",
           caption="source: Kaggle biodiversity dataset"
      ) + 
      scale_size_continuous(
        range=c(3,9),
        guide=FALSE) +
      scale_colour_distiller(name = "Total Plant Species",
                      #       breaks=c(800, 600, 400, 200, 0),
                             palette="Spectral")
    plant_biodiv_mapped

![]({{ site.baseurl}}/images/np_biodiversity/unnamed-chunk-7-1.png)

<br  />

Plant Biodiversity Correlation
==============================

<br  />

    plant_bio_corr <- parks %>%
      left_join(plant_biodiv, by = "Park Name") %>%
      select(park_name, Acres, Latitude, Longitude, total_div)
    plant_bio_corr <- unique(plant_bio_corr)

    plant_bio_corr %>%
      ggplot(
        aes(total_div,
            Acres
            )
      ) + 
      geom_point(
          color="darkgreen",
          size=4,
          alpha = 0.8
      ) + 
      geom_smooth(method="lm", 
                  color = "black",
                  se = FALSE,
                  linetype="dotted"
      ) + 
      labs(                   
        title="Plant Biodiversity & National Park Area", 
        y="Square Acres", 
        x="Total Number of Plant Species", 
        caption = "Source: Kaggle biodiversity dataset"
      )

![]({{ site.baseurl}}/images/np_biodiversity/unnamed-chunk-8-1.png)

    plant_cor_value <- cor.test(plant_bio_corr$Acres, plant_bio_corr$total_div)
    plant_cor_value

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  plant_bio_corr$Acres and plant_bio_corr$total_div
    ## t = -0.31289, df = 54, p-value = 0.7556
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.3020621  0.2228531
    ## sample estimates:
    ##        cor 
    ## -0.0425398

<br  />

Animal Biodiveristy and Park Size
=================================

<br  />

    animal_biodiv_mapped <- parks %>%
      left_join(animal_biodiv, by = "Park Name")

    animal_biodiv_mapped <- animal_biodiv_mapped %>%
      filter(State != "AK" & State != "HI") %>% 
      ggplot(aes(Longitude, Latitude)) +
      borders("state") +
      geom_point(
        aes(
          size=total_div, 
          color=total_div
        )
      ) +
      coord_quickmap() +   
      labs(title="U.S. National Parks",               
           subtitle="Animal Biodiversity",
           caption="source: Kaggle biodiversity dataset"
      ) + 
      scale_size_continuous(
        range=c(3,9),
        guide=FALSE) +
      scale_colour_distiller(name = "Total Animal Species",
                             breaks=c(800, 600, 400, 200, 0),
                             palette="Spectral")
    animal_biodiv_mapped

![]({{ site.baseurl}}/images/np_biodiversity/unnamed-chunk-9-1.png)

<br  />

Animal Biodiversity Correlation
===============================

<br  />

    animal_bio_corr <- parks %>%
      left_join(animal_biodiv, by = "Park Name") %>%
      select(park_name, Acres, Latitude, Longitude, total_div)
    animal_bio_corr <- unique(animal_bio_corr)

    animal_bio_corr %>%
      ggplot(
        aes(total_div,
            Acres
        )
      ) + 
      geom_point(
        color="burlywood4",
        size=4,
        alpha = 0.8
      ) + 
      geom_smooth(method="lm", 
                  color = "black",
                  se = FALSE,
                  linetype="dotted"
      ) + 
      labs(                   
        title="Animal Biodiversity & National Park Area", 
        y="Square Acres", 
        x="Total Number of Animal Species", 
        caption = "source: Kaggle biodiversity dataset"
      )

![]({{ site.baseurl}}/images/np_biodiversity/unnamed-chunk-10-1.png)


    animal_cor_value <- cor.test(animal_bio_corr$Acres, animal_bio_corr$total_div)
    animal_cor_value

    ## 
    ##  Pearson's product-moment correlation
    ## 
    ## data:  animal_bio_corr$Acres and animal_bio_corr$total_div
    ## t = -0.3738, df = 54, p-value = 0.71
    ## alternative hypothesis: true correlation is not equal to 0
    ## 95 percent confidence interval:
    ##  -0.3095684  0.2149692
    ## sample estimates:
    ##         cor 
    ## -0.05080241

Summary:
--------
