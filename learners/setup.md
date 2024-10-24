---
title: Setup for Timeseries in R
---

::::::::::::::::::::::::::::::::::::: prereqs

To successfully participate in this course, we ask that participants meet the following prerequisites: 

- Have an understanding of how file explorer works - creating folders, how to access and download files and moving them into the approriate folder (uses daily)
- Be able to use and download files from internet (uses daily)
- Be able to open, use basic functions, and edit in excel (uses weekly - monthly)
- Some very basic statistical knowledge (what is a mean, median, boxplot, distribution etc.) (any previous use)
- An overall understanding of why timeseries data is important (any previous use) 
- Basic R: they have opened it, made projects, used basic functions (View, summary, mean, sd, etc.) and can load a dataset and install packages (any previous use) 
- R coding: Base R: understanding of aggregrate() and some experience with tidyverse (has used at least twice before)

::::::::::::::::::::::::::::::::::::::::::::::::

## Data Sets

<!--
FIXME: place any data you want learners to use in `episodes/data` and then use
       a relative link ( [data zip file](data/lesson-data.zip) ) to provide a
       link to it, replacing the example.com link.
-->
Download the [data zip file](https://example.com/FIXME) and unzip it to your Desktop

post them onto HydroShare??

## Software Setup

Install the following packages:

- tidyverse (includes dyplor, ggplot2, and lubridate)

```r

install.packages("tidyverse")

```

Create an .Rproject for this workshop with the following folders:
- data
- data_processed
- scripts

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

