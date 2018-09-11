---
layout: post
title: "A Roll of the Dice"
categories:
  - Blog
top_tags: R
exc: "This was rushed for my eager fans"
tags:
- probability
- simulation
- visualization
- project
---

<br>

### Introduction

<br>

While learning about probability in my master's program, we often refer
to dice and decks of cards to simplify and familiarize sometimes complex
ideas. I decided to bring this often used example into R to see what I
could do with it.

<br>

### Libraries

<br>

    library('tidyverse')

<br>

### Rolling a Single Die

<br>

To simulate the roll of a single die is extremely easy in R. Using the
sample function, define a range of values and your sample size. If
you're looking to roll more than a single die, add in the argument
`replace = T` to command independent results.

    sample(1:6,1)

    ## [1] 4

<br>

### Relative Frequency with a Die

<br>

relative frequency approaches probability as sample size increases.

To illustrate this statement, I'll create two plots. In the first plot,
I roll a single die 50 times, recording the outcome of each roll. Then I
tally the numbers, and divide them by the total 50 rolls, calculating
the relative frequencies of each of the six possible outcomes. The
probability of each of these outcomes is 1/6, assuming a fair die. I
mark the expected probability with a solid red line in each of the
figures. The first relative frequency plot is titled "50 Roll Frequency
Plot".

The second relative frequency plot repeats the same process, but
increases the sample size by 3 magnitudes. The 50,000 roll figure is
titled "50,000 Roll Frequency Plot" is created.

<br>

    # use the sample function to simulate 100 independent rolls of a die
    single_die_1 <- sample(1:6, 50, replace=T)

    # order the values into a dataframe named sd_1_freq
    sd_1_freq <- data_frame(num = 1:6, roll = table(single_die_1)/50)

    # plot the frequency data
    ggplot(sd_1_freq) + 
      geom_bar(aes(num,roll),
               stat = "identity",
               color = "darkslateblue",
               fill = "darkslateblue") + 
      geom_hline(yintercept = (1/6),
                 col = "firebrick",
                 size = 1
                 ) + 
      scale_x_continuous(name = "Result",
                         breaks = sd_1_freq$num, 
                         labels = sd_1_freq$num) + 
      ylab("Relative Frequency") + 
      ggtitle("50 Roll Relative Frequency") + 
      theme_minimal()

![]({{ site.baseurl}}/images/roll_em/roll_em_1.png)

<br>

    # use the sample function to simulate 50,000 independent rolls of a die
    single_die_2 <- sample(1:6, 50000, replace=T)

    # order the values into a dataframe named sd_1_freq
    sd_2_freq <- data_frame(num = 1:6, roll = table(single_die_2)/50000)

    # plot the frequency data
    ggplot(sd_2_freq) + 
      geom_bar(aes(num,roll), 
               stat="identity",
               color = "darkslateblue",
               fill = "darkslateblue"
               ) + 
      geom_hline(
        yintercept = (1/6),
        col = "firebrick",
        size = 1
        ) + 
      scale_x_continuous(name = "Result",
                         breaks = sd_2_freq$num, 
                         labels = sd_2_freq$num
                         ) + 
      ylab("Relative Frequency") + 
      ggtitle("50,000 Roll Relative Frequency") + 
      theme_minimal()

![]({{ site.baseurl}}/images/roll_em/roll_em_2.png)

<br>

### Summing Two Dice

<br>

Have you ever played the casino game Craps? In Craps, you roll two die,
and sum them to determine if you've automatically won (7 or 11), lost
(2, 3, or 12) or some other outcome has occurred. Let's first simulate
summing a single roll of two dice.

<br>

    sum(sample(1:6,1), sample(1:6,1))

    ## [1] 6

<br>

That was easy! So let's do it 10,000 times and plot the relative
frequency of each result! This will give us a solid idea on the
probabilities behind each craps bet.

<br>

    # create empty dataframe of 10,000 values
    roll_vector <- vector("double", 10000)

    # for each element in roll_vector
    for (i in seq_along(roll_vector)) {
    # the element equals the sum of two dice
      roll_vector[i] = sum(sample(1:6,1), sample(1:6,1))
    }

    # turn the resulting vector into a dataframe to plot
    roll_df <- as_data_frame(table(roll_vector)/10000)
    # name the variables roll, and freq
    colnames(roll_df) <- c("roll", "freq")
    # turn the table-created roll variable into a numeric
    roll_df$roll <- as.numeric(roll_df$roll)

    # plot the relative frequencies 
    ggplot(roll_df) + 
      geom_bar(aes(roll, freq),
               stat = "identity",
               color = "mediumpurple4",
               fill = "mediumpurple4"
               ) + 
      scale_x_continuous(name = "Sum of Dice",
                         breaks = roll_df$roll, 
                         labels = roll_df$roll
                         ) + 
      ylab("Relative Frequency") + 
      ggtitle("10,000 Two-Dice Roll Relative Frequency") + 
      theme_minimal()

![]({{ site.baseurl}}/images/roll_em/roll_em_3.png)

<br>

### Generalizing with a Function

<br>

So we've recorded the relative frequency of one and two-die rolls, can
we do it for a 100 die roll? How about a 10,000 Die roll? and What if I
want to roll the die 67 times instead of 50, or 670,000 times instead of
50,000.

To effectively create plots for an infinite number of inputs, we need to
create a function. I'll call the function `roll_em`!

<br>

    # Create the function "roll_em" to sum any number of dice for any number of rolls and produce a relative frequency plot as the output. 

    # naming the function
    roll_em <- function(n_dice, n_roll) {

    # check for input errors, both inputs must be non-zero integers  
      if (n_dice == 0 | is.na(n_dice) | n_dice %% 1 != 0) {
          stop("Enter a non-zero integer for the number of dice to roll")
      } 
        else if (n_roll == 0 | is.na(n_roll) | n_roll %% 1 != 0) {
          stop("Enter a non-zero integer for the number of rolls to perform")
      } 
        else {

    # if no errors are detected, move on to the body of the function
       
    # declare a counter, i, and set it to zero
      i <- 0  
      
    # create roll vector of user-specified length
      roll_vector <- vector("double", n_roll)


    # while the counter doesn't equal the user-specified length of roll_vector, loop through the following commands
      while (i <= length(roll_vector)) {
        
    # create a new dice_vector of user-specified length
        dice_vector <- vector("double", n_dice)

    # for each element j in dice vector, roll a single die and record the value
        for (j in seq_along(dice_vector)) {
          dice_vector[j] = sample(1:6,1)
        }
        
    # sum the values in the dice vector and put that value into the current roll_vector element
        roll_vector[i] <- sum(dice_vector)

    # add one iteration to the roll_vector counter
        i <- i + 1

    # repeat the sequence
      }
      

    # once all elements of the roll_vector are filled, create a roll dataframe
      roll_df <- as_data_frame(table(roll_vector)/n_roll)
      
    # name the columns, or variables, roll and freq (frequency)
      colnames(roll_df) <- c("roll", "freq")

    # ensure the roll numbers are numerics, instead of characters (default in table)
      roll_df$roll <- as.numeric(roll_df$roll)
      
    # plot the output
      ggplot(roll_df) + 
        geom_bar(aes(roll, freq),
               stat="identity", 
               color = "midnightblue",
               fill = "midnightblue"
               ) + 
      scale_x_continuous(name = "Sum of Dice",
                         breaks = roll_df$roll, 
                         labels = roll_df$roll
                         ) + 
      ylab("Relative Frequency") + 
      ggtitle("Roll 'EM! Results") + 
      theme_minimal()
      
    # conclude else statement function
        }
      
    # conclude roll_em function
    }

<br>

That's the inner workings of my new function `roll_em()`! Let's see if
it works!!

<br>

### Testing Roll 'EM!

<br>

A generic entry:

    roll_em(5,200)

![]({{ site.baseurl}}/images/roll_em/roll_em_4.png)

<br>

An entry designed to give an error:

    roll_em(0,1)

    ## Error in roll_em(0, 1): Enter a non-zero integer for the number of dice to roll

<br>

Another error entry:

    roll_em(2,500.1)

    ## Error in roll_em(2, 500.1): Enter a non-zero integer for the number of rolls to perform

<br>

An obscure entry:

    roll_em(502,7)

![]({{ site.baseurl}}/images/roll_em/roll_em_5.png)

<br>

A large number of rolls:

    roll_em(5, 91572)

![]({{ site.baseurl}}/images/roll_em/roll_em_6.png)

<br>

Roll 'EM works!

What a fun little project to practice making functions and better
visualize probabilities.

<br>

Until next time, <br>

- Fisher


