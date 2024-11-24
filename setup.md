---
title: Summary and Setup
---

Timeseries in R

The best way to learn how to program is by doing! In this lesson we will learn some of the basics for working with timeseries in R. We assume that you have some experience with programming in R already for this course. 

::::::::::::::::::::::::::::::::::::: prereqs

To successfully participate in this course, we ask that Learners meet the following prerequisites: 

- Have an understanding of how file explorer works - creating folders, how to access and download files and moving them into the approriate folder (uses daily)
- Be able to use and download files from internet (uses daily)
- Be able to open, use basic functions, and edit in excel (uses weekly to monthly)
- Some very basic statistical knowledge (what is a mean, median, boxplot, distribution etc.) (any previous use)
- An overall understanding of why timeseries data is important (any previous use) 
- Have some familiarity with R: it is downloaded and up to date, Learners have made projects, used basic functions (View, summary, mean, sd, etc.) and can load a dataset and install packages (any previous use) 
- R coding: Base R: understanding of aggregrate() and some experience with tidyverse (have used at least twice before)

::::::::::::::::::::::::::::::::::::::::::::::::

We will study changes in surfacewater and groundwater temperatures and stage (water level) over time. The datasets are uploaded as separate .csv files (comma-separated values), one for surfacewater data and one for groundwater data. Each row holds information for a single time point (in 15 minute intervals) and the columns represent the different variables we will study. 

[show first 5 rows of one dataset?]

## Data Sets

<!--
[FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.]
-->

Make a new folder on your Desktop for this lesson: Timeseries_R
Within your Timeseries_R folder, create an .Rproject for this workshop with the following folders:
- data
- data_processed
- scripts
Download the [ [data zip file](https://example.com/FIXME) ] and move into the 'data' folder
Unzip the .csv files to the 'data' folder. This may create another folder.

We will: 
- load data into the R enviornment
- clean and manipulate the data
- calculate basic metrics
- visulize timeseries data

## Software Setup

This course requires the following packages:

- tidyverse (includes dyplr, ggplot2, and lubridate)

```r

install.packages("tidyverse")

```

## i don't think we need anything else here... maybe a quick note about differences using windows / mac / linux?

::::::::::::::::::::::::::::::::::::::: discussion

### Details

Setup for different systems can be presented in dropdown menus via a `spoiler`
tag. They will join to this discussion block, so you can give a general overview
of the software used in this lesson here and fill out the individual operating
systems (and potentially add more, e.g. online setup) in the solutions blocks.

:::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::: spoiler

### Windows

Use PuTTY

::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

Use Terminal.app

::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

Use Terminal

::::::::::::::::::::::::

