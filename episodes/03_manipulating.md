---
title: "Manipulating a time series data frame"
teaching: XX
exercises: XX
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

We will use the files Konza_GW.csv and Konza_SW.csv cleaned in the previous lession and the same R packages. 
Please remember, we created new datasets without oultiers or missing datapoints to create a clean dataframe.
As a reminder we will need packages ggplot2, tidyr, readr, lubridate, zoo, and dplyr.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Make sure everyone knows what datasets we are using and to upload now. 

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

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
::::::::::::::::::::::::::::::::solution

```output
Fill in the solution...
```

::::::::::::::::::::::::
::::::::::::::::::::::::::::::::: exercise

When working in R, sometimes you can have a lot of files open in your environment, making it difficult to keep track of what you 
are working on. 

It can be helpful to periodically remove files to keep a clean environment. 

You can do this with individual files using the function rm()

```r
rm(konza_sw)
rm(konza_gw)

#or by cleaning out the full environment all at once

rm(list = ls())
```
:::::::::::::::::::::::: solution
```output
BE AWARE! Whenever you remove something from the environment there is no easy "back" or "undo" button in R. Always make sure you have 
the version of the files that you need to continue with your work!

````

::::::::::::::::::::::::


## Upload the cleaned dataframes

::::::::::::::::::::::::::::::::: exercise

```r

```
:::::::::::::::::::::::::::::: solution

```output

```
