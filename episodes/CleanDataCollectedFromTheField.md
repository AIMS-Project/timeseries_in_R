---
title: "Clean data collected from the field"
teaching: 20
exercises: 10
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can you clean time series datasets collected from the field?
  
::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Subset rows and columns
- Identify outliers and interpolate missing data
- Filter outliers and missing data
- Create a new dataframe
  
::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

In this episode, we will clean the field data and prepare the data for analysis and visualization. To do this, we will: 

1. Identify potentially problematic data points (outliers)  
2. Interpolate missing data  
3. Change data from UTC to CT

We will use the files Konza_GW.csv and Konza_SW.csv imported in the introduction to the timeseries lesson and the same R packages. Please remember, that we looked at a summary of the data and noticed some weird values. As a reminder we will need packages ggplot2, tidyr, readr, lubridate, zoo, and dplyr.

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Make sure everyone knows what datasets we are using and to upload now. 

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::: exercise

## Check the data format

 Let's look at the data structure and make sure the data is in the correct format.


```r
str(konza_sw)

#since my timestamp was imported in as a character I will change the timestamp to a datetime format. The data is in the UTC timezone but will need to be changed to the CT timezone.

konza_sw$timestamp<- as.POSIXct(konza_sw$timestamp, format = "%m/%d/%Y %H:%M", tz='UTC')
konza_gw$timestamp<- as.POSIXct(konza_gw$timestamp, format = "%m/%d/%Y %H:%M", tz='UTC')

str(konza_sw)
```

:::::::::::::::::::::::: solution

 ```output
spc_tbl_ [67,804 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ ...1        : num [1:67804] 1 2 3 4 5 6 7 8 9 10 ...
 $ timestamp   : chr [1:67804] "6/9/2021 15:10" "6/9/2021 15:20" "6/9/2021 15:30" "6/9/2021 15:50" ...
 $ SW_Temp_PT_C: num [1:67804] -9999 -9999 -9999 -9999 -9999 ...
 $ yearMonth   : chr [1:67804] "2021-06" "2021-06" "2021-06" "2021-06" ...
 $ SW_Level_ft : num [1:67804] -9999 -9999 -9999 -9999 -9999 ...
 - attr(*, "spec")=
  .. cols(
  ..   ...1 = col_double(),
  ..   timestamp = col_character(),
  ..   SW_Temp_PT_C = col_double(),
  ..   yearMonth = col_character(),
  ..   SW_Level_ft = col_double()
  .. )
 - attr(*, "problems")=<externalptr>

#after changing to a datetime format
spc_tbl_ [67,804 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ ...1        : num [1:67804] 1 2 3 4 5 6 7 8 9 10 ...
 $ timestamp   : POSIXct[1:67804], format: "2021-06-09 15:10:00" "2021-06-09 15:20:00" "2021-06-09 15:30:00" "2021-06-09 15:50:00" ...
 $ SW_Temp_PT_C: num [1:67804] -9999 -9999 -9999 -9999 -9999 ...
 $ yearMonth   : chr [1:67804] "2021-06" "2021-06" "2021-06" "2021-06" ...
 $ SW_Level_ft : num [1:67804] -9999 -9999 -9999 -9999 -9999 ...
 - attr(*, "spec")=
  .. cols(
  ..   ...1 = col_double(),
  ..   timestamp = col_character(),
  ..   SW_Temp_PT_C = col_double(),
  ..   yearMonth = col_character(),
  ..   SW_Level_ft = col_double()
  .. )
 - attr(*, "problems")=<externalptr> 


 ```

::::::::::::::::::::::::
::::::::::::::::::::::::::::::::: exercise

Since the data is in UTC, we can change the data to CT 


```r
#confirm the data is in UTC
head(konza_sw$timestamp)

#Change the timezone to CT
konza_sw$timestamp<- as.POSIXct(konza_sw$timestamp, format="%Y-%m-%d %H:%M", tz='America/Chicago')
konza_gw$timestamp<- as.POSIXct(konza_gw$timestamp, format="%Y-%m-%d %H:%M", tz='America/Chicago')
#check what timezone our data is in now
head(konza_sw$timestamp)
head(konza_gw$timestamp)
```

:::::::::::::::::::::::: solution

```output
[1] "2021-06-09 15:10:00 UTC" "2021-06-09 15:20:00 UTC" "2021-06-09 15:30:00 UTC"
[4] "2021-06-09 15:50:00 UTC" "2021-06-09 16:00:00 UTC" "2021-06-09 16:10:00 UTC"

[1] "2021-06-09 10:10:00 CDT" "2021-06-09 10:20:00 CDT" "2021-06-09 10:30:00 CDT" "2021-06-09 10:50:00 CDT"
[5] "2021-06-09 11:00:00 CDT" "2021-06-09 11:10:00 CDT"

[1] "2021-06-09 10:10:00 CDT" "2021-06-09 10:20:00 CDT" "2021-06-09 10:30:00 CDT" "2021-06-09 10:50:00 CDT"
[5] "2021-06-09 11:00:00 CDT" "2021-06-09 11:10:00 CDT"
```

::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::

## Plot the data

 Now, let's plot the surface water level data and the water temperature data and evaluate where there might be erroneous values

::::::::::::::::::::::::::::::::: exercise

```r
library(ggplot2)

ggplot(data= konza_sw)+
 geom_line(aes(x=timestamp, y=SW_Level_ft))+
 xlab("Time")+
 ylab("Surface Water Level (ft)")
```

:::::::::::::::::::::::: solution 
 
```output
![Raw surface water level (ft)](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/sw_level_raw.png)


```
::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::::::::::::::::: exercise

Let's plot the surface water temperature data.
 
```r
ggplot(data= konza_sw)+
 geom_line(aes(x=timestamp, y=SW_Temp_PT_C))+
 xlab("Time")+
 ylab("Temperature (°C)")
```

:::::::::::::::::::::::: solution 

```output
![sw_temp_raw](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/sw_temp_raw.png)



```

:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: challenge 

## Exercise 1: Plot the groundwater dataset and identify outliers.

Fill in the blank: Using code from plotting above to recreate the water level and temperature plots using the groundwater dataset. Visually identify outliers.

```r
ggplot(data= _____)+
 geom_line(aes(x=____, y=____))+
 xlab("Time")+
 ylab("Temperature (degrees symbol C)")

ggplot(data= _____)+
 geom_line(aes(x=____, y=_____))+
 xlab("Time")+
 ylab("Water Level (ft)")
```

:::::::::::::::::::::::: solution 

## Output
 
```output
![Raw groundwater level](https://github.com/AIMS-Project/timeseries_in_R/instructors/gw_level_raw.png)


![Raw groundwater temperature](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/gw_temp_raw.png)

```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::: discussion

Prompt for the learners to discuss.

:::::::::::::::::::::::::::::::::

Looks like we have outliers in our datasets. It is a good idea to zoom in on the outliers and see if there is something weird happening at that time. We can do this by subsetting our data. 

::::::::::::::::::::::::::::::::::::: challenge 

## Exercise 2: Which piece of code would you use to subset the outliers? Which piece of code would you use to subset NA values? Use this code to change outliers to NA.

Test the following code using the surface water dataset followed by the groundwater dataset. Don't forget to change the dataframe when evaluating the groundwater dataset:

```r
a) konza_sw[c(67780:67830),]
b) konza_sw[konza_sw$timestamp > "2022-09-23 11:50" & konza_sw$timestamp< "2022-09-25 12:00",]
c) subset(konza_sw, SW_Level_ft < 0 )
d) konza_sw[is.na(konza_sw$SW_Level_ft),]

# you can also use the tidyverse package to subset data:

e) konza_sw %>% 
  filter(SW_Level_ft < 0)

f) konza_sw %>% 
  filter(!is.na(SW_Level_ft))

```

:::::::::::::::::::::::: solution 

## Output

```output
a) # A tibble: 51 × 5
    ...1 timestamp           SW_Temp_PT_C yearMonth SW_Level_ft
   <dbl> <dttm>                     <dbl> <chr>           <dbl>
 1 67780 2022-09-23 14:50:00         19.6 2022-09          0.04
 2 67781 2022-09-23 15:00:00         19.9 2022-09          0.04
 3 67782 2022-09-23 15:10:00         20.4 2022-09      -9999   
 4 67783 2022-09-23 15:20:00      -9999   2022-09      -9999   
 5 67784 2022-09-23 15:30:00      -9999   2022-09      -9999   
 6 67785 2022-09-23 15:40:00      -9999   2022-09      -9999   
 7 67786 2022-09-23 15:50:00      -9999   2022-09      -9999   
 8 67787 2022-09-23 16:00:00      -9999   2022-09      -9999   
 9 67788 2022-09-23 16:10:00      -9999   2022-09      -9999   
10 67789 2022-09-23 16:20:00      -9999   2022-09      -9999   
# ℹ 41 more rows
# ℹ Use `print(n = ...)` to see more rows

b) # A tibble: 30 × 5
    ...1 timestamp           SW_Temp_PT_C yearMonth SW_Level_ft
   <dbl> <dttm>                     <dbl> <chr>           <dbl>
 1 67775 2022-09-23 14:00:00         18.3 2022-09          0.04
 2 67776 2022-09-23 14:10:00         18.6 2022-09          0.04
 3 67777 2022-09-23 14:20:00         18.9 2022-09          0.04
 4 67778 2022-09-23 14:30:00         19.1 2022-09      -9999   
 5 67779 2022-09-23 14:40:00         19.4 2022-09          0.03
 6 67780 2022-09-23 14:50:00         19.6 2022-09          0.04
 7 67781 2022-09-23 15:00:00         19.9 2022-09          0.04
 8 67782 2022-09-23 15:10:00         20.4 2022-09      -9999   
 9 67783 2022-09-23 15:20:00      -9999   2022-09      -9999   
10 67784 2022-09-23 15:30:00      -9999   2022-09      -9999   
# ℹ 20 more rows
# ℹ Use `print(n = ...)` to see more rows

c) # A tibble: 22,000 × 5
    ...1 timestamp           SW_Temp_PT_C yearMonth SW_Level_ft
   <dbl> <dttm>                     <dbl> <chr>           <dbl>
 1     1 2021-06-09 10:10:00      -9999   2021-06         -9999
 2     2 2021-06-09 10:20:00      -9999   2021-06         -9999
 3     3 2021-06-09 10:30:00      -9999   2021-06         -9999
 4     4 2021-06-09 10:50:00      -9999   2021-06         -9999
 5     5 2021-06-09 11:00:00      -9999   2021-06         -9999
 6     6 2021-06-09 11:10:00      -9999   2021-06         -9999
 7     7 2021-06-09 11:20:00      -9999   2021-06         -9999
 8   399 2021-06-12 06:20:00         15.8 2021-06         -9999
 9   442 2021-06-12 13:30:00         18.4 2021-06         -9999
10   443 2021-06-12 13:40:00         18.5 2021-06         -9999
# ℹ 21,990 more rows
# ℹ Use `print(n = ...)` to see more rows


d) # A tibble: 0 × 5
# ℹ 5 variables: ...1 <dbl>, timestamp <dttm>, SW_Temp_PT_C <dbl>, yearMonth <chr>, SW_Level_ft <dbl>

e) 'data.frame':	22000 obs. of  5 variables:
 $ X           : int  1 2 3 4 5 6 7 399 442 443 ...
 $ timestamp   : POSIXct, format: "2021-06-09 10:10:00" "2021-06-09 10:20:00" "2021-06-09 10:30:00" "2021-06-09 10:50:00" ...
 $ SW_Temp_PT_C: num  -9999 -9999 -9999 -9999 -9999 ...
 $ yearMonth   : chr  "2021-06" "2021-06" "2021-06" "2021-06" ...
 $ SW_Level_ft : num  -9999 -9999 -9999 -9999 -9999 ...

f) 'data.frame':	22000 obs. of  5 variables:
 $ X           : int  1 2 3 4 5 6 7 399 442 443 ...
 $ timestamp   : POSIXct, format: "2021-06-09 10:10:00" "2021-06-09 10:20:00" "2021-06-09 10:30:00" "2021-06-09 10:50:00" ...
 $ SW_Temp_PT_C: num  -9999 -9999 -9999 -9999 -9999 ...
 $ yearMonth   : chr  "2021-06" "2021-06" "2021-06" "2021-06" ...
 $ SW_Level_ft : num  -9999 -9999 -9999 -9999 -9999 ...

```

::::::::::::::::::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::: discussion

What did these different methods show? What were the advantages and disadvantages of each method?

::::::::::::::::::::::::::::::::::::::::::::::::

## Replace bad data values

Now that we know there are numerous weird values, we want to remove those from our dataset by setting them to NA. For surface water level we will remove values below 0 but for temperatures, we will set a low threshold where we will assume the value is incorrect. 

::::::::::::::::::::::::::::::::: exercise

```r
# base R
konza_sw$SW_Level_ft[konza_sw$SW_Level_ft< 0]<- NA
konza_sw$SW_Temp_PT_C[konza_sw$SW_Temp_PT_C< -100]<- NA

konza_gw$GW_Level_ft[konza_gw$GW_Level_ft< 0]<- NA
konza_gw$GW_Temp_PT_C[konza_gw$GW_Temp_PT_C< -100]<- NA

# tidyR example
konza_sw <- konza_sw %>%
mutate(SW_Level_ft = if_else(SW_Level_ft < 0, 0, SW_Level_ft))

#subset and plot to see if our changes worked
subset(konza_sw, SW_Level_ft < 0 )

#plot each dataset and see if it looks ok
ggplot(data= konza_sw)+
  geom_line(aes(x=timestamp, y=SW_Level_ft))+
  xlab("Time")+
  ylab("Surface Water Level (ft)")

ggplot(data= konza_sw)+
  geom_line(aes(x=timestamp, y=SW_Temp_PT_C))+
  xlab("Time")+
  ylab("Temperature (°C)")


ggplot(data= konza_gw)+
  geom_line(aes(x=timestamp, y=GW_Level_ft))+
  xlab("Time")+
  ylab("Surface Water Level (ft)")


ggplot(data= konza_gw)+
  geom_line(aes(x=timestamp, y=GW_Temp_PT_C))+
  xlab("Time")+
  ylab("Temperature (°C)")

```

:::::::::::::::::::::::: solution 

```output

# A tibble: 0 × 5
# ℹ 5 variables: ...1 <dbl>, timestamp <dttm>, SW_Temp_PT_C <dbl>, yearMonth <chr>, SW_Level_ft <dbl>

![Surface water level](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/sw_level_edit.png)


![Surface water temperature](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/sw_temp_edit.png)


![Groundwater level](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/gw_level_edit.png)


![Groundwater Temperature](https://github.com/AIMS-Project/timeseries_in_R/blob/main/instructors/gw_temp_edit.png)


```

:::::::::::::::::::::::::::::::::


Now that we removed bad data values we can count how many NA values are in our dataset. This information may be useful if you need to report your results. 

::::::::::::::::::::::::::::::::: exercise

```r
#count NA values
sum(is.na(konza_sw$SW_Temp_PT_C))
sum(is.na(konza_sw$SW_Level_ft))
sum(is.na(konza_gw$GW_Temp_PT_C))
sum(is.na(konza_gw$GW_Level_ft))

#divide the number of NA values by the number of rows to see what fraction of our data is NA
sum(is.na(konza_sw$SW_Temp_PT_C)) / nrow(konza_sw)
sum(is.na(konza_sw$SW_Level_ft)) / nrow(konza_sw)
sum(is.na(konza_gw$GW_Temp_PT_C)) / nrow(konza_gw)
sum(is.na(konza_gw$GW_Level_ft)) /nrow(konza_gw)


```

:::::::::::::::::::::::: solution 

```output
[1] 29
[1] 22000
[1] 30
[1] 1884

#divide the number of NA values by the number of rows to see what fraction of our data is NA
[1] 0.0004277034
[1] 0.3244646
[1] 0.0004424518
[1] 0.02778597

```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

Some of these NA values were short intervals that we can interpolate. Let's interpolate missing values in sections that have less than 12 consecutive NA values and save the interpolated data as a separate column.

::::::::::::::::::::::::::::::::: exercise
```R
#interpolate missing values
konza_sw$SW_TEMP_PT_C_int <- na.approx(konza_sw$SW_Temp_PT_C, maxgap = 12, na.rm=FALSE)
konza_sw$SW_Level_ft_int <- na.approx(konza_sw$SW_Level_ft , maxgap = 12, na.rm=FALSE)
konza_gw$GW_TEMP_PT_C_int <- na.approx(konza_gw$GW_Temp_PT_C, maxgap = 12, na.rm=FALSE)
konza_gw$GW_Level_ft_int <- na.approx(konza_gw$GW_Level_ft, maxgap = 12, na.rm=FALSE)

```

:::::::::::::::::::::::: solution 

```output

```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: challenge 

## Exercise 3: Find the new fraction of NA values in the datasets

Adjust the following code for each column and dataset to find the new fraction of NA values: 

```r
#SW temperature 
sum(is.na(konza_sw$SW_TEMP_PT_C_int)) / nrow(konza_sw)

#SW level

#GW temperature

#GW level


```

:::::::::::::::::::::::: solution 

## Output

```output
sum(is.na(konza_sw$SW_TEMP_PT_C_int)) / nrow(konza_sw)
[1] 0.0004277034

sum(is.na(konza_sw$SW_Level_ft_int)) / nrow(konza_sw)
[1] 0.3133296

sum(is.na(konza_gw$GW_TEMP_PT_C_int)) / nrow(konza_gw)
[1] 0.004277034

sum(is.na(konza_gw$GW_Level_ft)) /nrow(konza_gw)
[1] 0.02778597

```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::

