---
title: "Manipulating a time series data frame"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can we reformat our data for analysis and visualization? 
- How do we manipulate tabular data in R? 
- How and when is it useful to break down a timeseries?
  
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Manipulate a timeseries into different formats for analyses. 
- Pivot data from long to wide formats. 
- Merge datasets with similar time points.
- Create plots of timeseries data at different temporal scales.
  
::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

In this episode, we will manipulate our cleaned field data for analysis and practice some basic visualization. To do this, we will: 

1. Change the format of the dataset from "wide" to "long" and back again.
2. Combine datasets.
3. Create basic plots to observe trends over various time frames. 

We will use the files konza_gw.csv and konza_sw.csv cleaned in the previous lession and the same R packages. 
Please remember, we created new datasets without oultiers or missing datapoints to create a clean dataframe.
As a reminder we will need packages ggplot2, tidyr, readr, lubridate, zoo, and dplyr.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Make sure everyone knows what datasets we are using and to upload now. 

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

Depending on your project and the anlyses you want to do, you may want to change the layout of a data set. 

Currently, our surface and ground water data sets are in the long format:

```r
str(konza_sw)
```

```output
data.frame':	67804 obs. of  7 variables:
 $ X               : int  1 2 3 4 5 6 7 8 9 10 ...
 $ timestamp       : POSIXct, format: "2021-06-09 10:10:00" "2021-06-09 10:20:00" "2021-06-09 10:30:00" "2021-06-09 10:50:00" ...
 $ SW_Temp_PT_C    : num  NA NA NA NA NA ...
 $ yearMonth       : chr  "2021-06" "2021-06" "2021-06" "2021-06" ...
 $ SW_Level_ft     : num  NA NA NA NA NA NA NA 1.17 1.16 1.16 ...
 $ SW_TEMP_PT_C_int: num  NA NA NA NA NA ...
 $ SW_Level_ft_int : num  NA NA NA NA NA NA NA 1.17 1.16 1.16 ...
```

Let's say we wanted to make the surface water data set into a wide format, where each temperature and water level point would be averaged over each yearMonth

```r

```




:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: exercise

## Save the cleaned dataframes

We can use R to create versions of our cleaned dataframes so we won't have to go back and run the code from the previous lesson every 
time we want to work with the data.

To keep our R project neat, save these files in the folder "data_processed". 

Tip - use names that are short, but clear.

```r
write.csv(konza_sw, ".../data_processed/surfacewater_KNZ_clean.csv"
write.csv(konza_gw, ".../data_processed/groundwater_KNZ_clean.csv"
```
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

When working in R, sometimes you can have a lot of files open in your environment, making it difficult to keep track of what you 
are working on. 

It can be helpful to periodically remove files to keep a clean environment. 

You can do this with individual files using the function rm()

```r
rm(konza_sw)
rm(konza_gw)
```
or by cleaning out the full environment all at once

```r
rm(list = ls())
```

BE AWARE! Whenever you remove something from the environment there is no easy "back" or "undo" button in R. Always make sure you have 
the version of the files that you need to continue with your work!

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: exercise

## Upload the cleaned dataframes



```r

```
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
