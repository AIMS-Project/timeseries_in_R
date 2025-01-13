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
We will need packages ggplot2, data.table, tidyr, readr, lubridate, zoo, and dplyr.

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

Let's say we wanted to make the surface water data set into monthly averages, then into long format for plotting in ggplot facets. 
- Using the yearMonth column we will average the temperature and water level data by each year-month.
- We will melt the data into a long column
- plot using ggplot

```r
#first, lets aggregate the data to yearMonth. We will use our interpolated data columns from the  clean data lesson. 
konza_sw_yrmnth<- aggregate(konza_sw[,c(6,7)], list(konza_sw$yearMonth), FUN=mean, na.rm=T) #aggregate the data columns temperature and water level by yearMonth

#melt the dataframe so that there are three columns; timestamp, value, and variable columns. Note that this uses the data.table melt function.
konza_sw_yrmnth_long<- melt(konza_sw_yrmnth, id.var="Group.1")
konza_sw_yrmnth_long$Group.1<- ym(konza_sw_yrmnth_long$Group.1) #change the character year-month format to a date format. This will add the first day of the month, but we need a day value to format the date value and plot
head(konza_sw_yrmnth_long)

#plot the data using ggplot facets
ggplot(data=konza_sw_yrmnth_long)+ 
  geom_line(aes(x=Group.1, y=value))+
  geom_point(aes(x=Group.1, y=value))+
  facet_grid(variable~., scales='free', 
             labeller=as_labeller(c('SW_TEMP_PT_C_int'='Surface Water Temperature (°C)','SW_Level_ft_int'= "Surface Water Level (ft)")))+
  theme_bw()

```

```output
#The aggregated konza sw data should look like the dataframe below
Group.1 SW_TEMP_PT_C_int SW_Level_ft_int
1  2021-06               NA              NA
2  2021-07      17.18803987       0.8641611
3  2021-08      24.95998656              NA
4  2021-09      20.27976820              NA
5  2021-10      14.24646306              NA
6  2021-11       9.85321991              NA
7  2021-12       5.54276295              NA
8  2022-01      -1.83456541              NA
9  2022-02      -0.01608879              NA
10 2022-03       6.81216816              NA
11 2022-04      11.85371296       1.1746100
12 2022-05      14.92028002       1.2461929
13 2022-06      17.90387561       1.3941239
14 2022-07      17.15921991       1.3757745
15 2022-08      21.30550236              NA
16 2022-09               NA              NA

      Group.1         variable    value
1 2021-06-01 SW_TEMP_PT_C_int 15.50090
2 2021-07-01 SW_TEMP_PT_C_int 17.18804
3 2021-08-01 SW_TEMP_PT_C_int 24.95999
4 2021-09-01 SW_TEMP_PT_C_int 20.27977
5 2021-10-01 SW_TEMP_PT_C_int 14.24646
6 2021-11-01 SW_TEMP_PT_C_int  9.85322


```
Let's say we wanted to make the surface water data set into an annual timeframe. To do this we will:
- add a column that only includes the year
- aggregate our data by the year column

```r
#add the year column
konza_sw$yr<- year(konza_sw$timestamp)
head(konza_sw)

#aggregate by year
konza_sw_annual<- aggregate(konza_sw[,c(6,7)], list(konza_sw$yr), FUN=mean, na.rm=T)
head(konza_sw_annual)

```

```output
# A tibble: 6 × 8
   ...1 timestamp           SW_Temp_PT_C yearMonth SW_Level_ft SW_TEMP_PT_C_int SW_Level_ft_int    yr
  <dbl> <dttm>                     <dbl> <chr>           <dbl>            <dbl>           <dbl> <dbl>
1     1 2021-06-09 10:10:00           NA 2021-06            NA               NA              NA  2021
2     2 2021-06-09 10:20:00           NA 2021-06            NA               NA              NA  2021
3     3 2021-06-09 10:30:00           NA 2021-06            NA               NA              NA  2021
4     4 2021-06-09 10:50:00           NA 2021-06            NA               NA              NA  2021
5     5 2021-06-09 11:00:00           NA 2021-06            NA               NA              NA  2021
6     6 2021-06-09 11:10:00           NA 2021-06            NA               NA              NA  2021


Group.1 SW_TEMP_PT_C_int SW_Level_ft_int
1    2021         15.34314       0.5591678
2    2022         12.01738       1.0039842

```


:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: exercise
## Change the groundwater data frame into a yearMonth dataframe and an annual dataframe


::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

What if we want to compare the surface water and groundwater monthly and annual values? We can merge the two dataframes and plot both together. 


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
