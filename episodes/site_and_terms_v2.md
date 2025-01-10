---
title: "Define common timeseries terms and familiarize yourself with the field site"
teaching: XX
exercises: XX
---

:::::::::::::::::::::::::::::::::::::: questions 

- What can you expect from this training?
- Where are these data coming from?
- What is a timeseries and why are they important?
  
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Create an R project with folders
- Get familiar with datasets
- Define common terms used to describe timeseries
- Import csv data into R
- Understand differences between base R and tidyverse approaches
  
::::::::::::::::::::::::::::::::::::::::::::::::

## About this Lesson

This lesson is meant to get us started on working with timeseries data. For this course, we wil 
create an R project and make sure we have access to the datasets. During this lesson we will 
familiarize ourselves with the data. This data was collected as a part of an NSF funded project, 
the Aquatic Intermittency effect of Microbiomes in Streams (AIMS; OSF OIA 2019603).  
In this lesson we will review how to 1. make a project in R, 2. familiarize ourselves with the 
data, 3. define common terms used in this lesson, 4. import .csv files, and 5. introuduce differences
between coding in base R and using tidyverse.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

It might be a good idea to check with Learners to see if they were able to get the 
R project created and the data downloaded to assess if running through the 'Getting Started' section 
is needed.

Use sticky notes / zoom reactions to check for completedness

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Getting Started

Before we get familiar with the data, let's first make sure we remember how to create an R Project, 
which will help tell our computers where the data exist. The 'Projects' interface in RStudio creates a 
working directory for you and sets the working directory to the project. 

::::::::::::::::::::::::::::::::::::: challenge 
Using file explorer, create a new folder and R project for this lesson. 
::::::::::::::::::::::::::::::::::::: discussion 
What folders would be good to include?
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: solution 
## Creating a new project:
- under the 'File" menu in RStudio, click on 'New Project', choose 'New Directory', then 'New Project'
- enter a name for this new directory and where it will be located. This creates your working directory (example: C:/XXX/User/Desktop/Timeseries_R)
- click 'Create Project'

## Helpful folders:
Typically, we encourage to make files for:
1. data
2. data_processed
3. scripts
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::

## Creating a timeseries

Start by opening up a new script file in RStudio.

The 'ts()' function convertes a numeric vector into a time series R object: 
ts(vector, start = , end = , frequency = ). 

To create a random vector, use 'sample()'

```r
vector <- sample(x = 1:20, size = 50, replace = TRUE)

# x is a vector of the values to sample from (in this example, any number from 1 to 20)
# size is the length of the vector (in this example, 50 characters long)
# replace = TRUE allows values to be repeated in the vector

```

To make this vector into a time series, use 'ts()'

```r
timeseries <- ts(vector, start = c(2010, 1), end = c(2015, 12), frequency = 12)

# start is the time of first observation (in this example, January 2010)
# end is the time of final observation (in this example, December 2015)
# frequency is the number of observations per unit time (1 = annual, 4 = quartly, 12 = monthly)

```

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise #1

Make a basic plot of this timeseries.
:::::::::::::::::::::::: solution 
```r
plot(timeseries)
``
:::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::

These plots will look differently for everyone, as we took a random sample to create our vector.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise #2

Make a timeseries that has annual samples from March 1992 to March 2024 and plot it.
:::::::::::::::::::::::: solution 
```r
annual_ts <- ts(vector, start = c(1992, 3), end = c(2024, 3), frequency = 1)

plot(annual_ts)
```
:::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::

Another helpful function for time series is 'stl()'. This function will decompose the timeseries 
dataset to identify patterns.

::::::::::::::::::::::::::::::::::::: keypoints 
Decomposition is a process that breaks down a time series dataset into different componsents to 
identify patterns and variations. This helps with predicting (or forecasting) future data points, 
finding trends, and identifying anomalies in the data (outliers).

Trends in time series occur when there is an overall direction of the data. For example, when the 
data is increasing, decreasing, or remains constant over time.
Outliers are single data points that go far outside the average value of a group of statistics. 

In addition to trends, time series data may have other qualities:
Seasonality: A regularly repeating pattern of highs and lows related to regular calendar periods 
such as seasons, quarters, months, or days of the week
Cycles: A series of fluctuations that occur over a long period of time, usually multiple years or decades
Variation: A change or difference in condition, amount, or level. Often separated into short or 
long term variation.
Stationarity: A times series is considered to be stationarity when the statistical properties do not 
depend on a time at which the series is observed (there is no temporal trend in the data)

Some other terms to be aware of when working with time series:
Missing values: Instances where a value for a specific variable is not recorded or available for a 
particular observation in a dataset (often recorded as NA or N/A or blank cells)
:::::::::::::::::::::::::::::::::::::

To look for seasonal decomposition, we can use

```r
fit <- stl(timeseries, s.window = "period")

plot(fit)
``

This function smooths out the seasonal trends in the data, taking the mean and smoothing out remainders 
to get overall trends in the data. 

### Understanding the Data

Make sure the data downloaded from [HydroShare for this lesson]() are moved into the 'data' folder so you 
can easily find it. 

An important first step for any project is making sure you are familiar with the data. 

This lesson uses data collected for the Aquatic Intermittency effects on Microbiomes in Streams 
(AIMS; NSF OIA 2019603). This project seeks to explore the impacts of stream drying on downstream water quality 
across Kansas, Oklahoma, Alabama, Idaho, and Mississippi. AIMS integrates datasets on hydrology, microbiomes,
macroinvertebrates, and biogeochemistry in three regions (Mountain West, Great Plains, and Southeast Forests) 
to test the overarching hypothesis that physical drivers (e.g., climate, hydrology) interact with biological 
drivers (e.g., microbes, biogeochemistry) to control water quality in non-perennial streams. Click here for 
an [overview of the AIMS project](https://youtu.be/HDKIBNEnwdM). 

::::::::::::::::::::::::::::::::::::: keypoints 
For our purposes, Non-perennial streams are rivers and streams that cease to flow at some point in time 
or space and are also commonly referred to as intermittent rivers and ephemeral streams (IRES). 
:::::::::::::::::::::::::::::::::::::

This dataset was collected from an [EXO2 Multiparameter Sonde](https://www.ysi.com/exo2) that can measure 
mutliple water chemistry parameters, including: conductivity, temperature, dissolved oxygen, pH, and others. 

One of these sensors was deployed in King's Creek and the Konza Biological Prairie.

::::::::::::::::::::::::::::::::::::: challenge 
## Exercise #3
Can you find the Konza Prairie Biological Station on Google Maps? 
What state did this project take place in? What is the closest city?
:::::::::::::::::::::::: solution 
This project took place in Kansas, USA. 
The exact coordinates for this site are: 39°05'32.2"N 96°35'13.9"W

The closest major city is Manhattan, KS. 
::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::

In this lesson, we will explore the data collected by the sensor at the outlet of the King's Creek 
watershed. We will compare groundwater data to surfacewater data to see when the stream dries.

::::::::::::::::::::::::::::::::::::: keypoints 
A watershed is an area of land that separates water flowing to different rivers, basins, lakes, or the sea.
Groundwater is freshwater that is stored in and orginiates from the ground between soil and rocks.
Surfacewater is freshwater found on top fo land in a lake, river, stream, or pond. 
:::::::::::::::::::::::::::::::::::::

Before we load the .csv files into R, try opening them using Excel. Since this will be your first 
time using the data, it is always a good idea to get familiar with the dataset.

:::::::::::::::::::::::: discussion 
Breakout rooms / At your table, answer the following questions:

Can you see how many time points there are?
 - When was the first time point?
 - When was the last time point?
What is the time interval of data collection?
Do you have guesses as to what the columns mean?
:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::: instructor
This data was collected between XXXX 2021 and XXXX 2024
The EXO2 collects data every 15 minutes.
Column names: XXXXX 
::::::::::::::::::::::::

Now that we have a better understanding of the data that we will be working with, 
let's get started working in R!

::::::::::::::::::::::::::::::::::::: challenge
## Exercise #4
How do you upload a .csv file into R?
:::::::::::::::::::::::: solution
```r
konza <- read.csv("C://[location of your data]", header = TRUE)
```
::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::

Once your data is uploaded to R, it can be a lot easier to understand the data.

To get a data summary, use
```r
summary(konza)
```
Which will tell you the min, 1st quartile, median, mean, 3rd quartile, and maximum values 
for numeric columns in the dataset.

Many of these numbers will be used to make boxplots of the data:

:::::::::::::::::::::::: glossary 
Boxplot: A method for demonstrating the spread and skewness of the data. Uses min, 
1st quartile, median, 3rd quartile, and maximum values
Quartiles: The Q1 (1st Qu.) is the 25% of the data below that point, Q2 is the end of the 
second quartile and 50th percentile (median), Q3 is the third quartile and is the 75th percentile 
and upper 25% of the data (3rd. Qu.)
Median: The middle number in an organized list of numbers
Minimum: The lowest number in a list of numbers
Maximum: The highest number in a list of numbers

Another common statistic used is the mean:
Mean: The average of a set of data
::::::::::::::::::::::::

Now that we better understand the data, let's get started on working on cleaning and manipulating the data!
