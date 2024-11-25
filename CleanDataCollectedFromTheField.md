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

We will use the files Konza_GW.csv and Konza_SW.csv imported in the introduction to the timeseries lesson and the same R packages. Please remember, that we looked at a summary of the data and noticed some weird values. 

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Make sure everyone knows what datasets we are using and to upload now. 

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::: exercise

## Check the data format

 Let's look at the data structure and make sure the data is in the correct format.
 
```r
str(konza_sw)
```

:::::::::::::::::::::::: solution

 ```output
 spc_tbl_ [99,105 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ ...1        : num [1:99105] 1 2 3 4 5 6 7 8 9 10 ...
 $ timestamp   : POSIXct[1:99105], format: "2021-06-09 15:10:00" "2021-06-09 15:20:00" "2021-06-09 15:30:00" ...
 $ SW_Temp_PT_C: num [1:99105] -1e+05 -1e+05 -1e+05 -1e+05 -1e+05 ...
 $ yearMonth   : chr [1:99105] "2021-06" "2021-06" "2021-06" "2021-06" ...
 $ SW_Level_ft : num [1:99105] -1.00e+05 -1.00e+05 -1.00e+05 -1.00e+05 -1.00e+05 -1.00e+05 -1.00e+05 1.17 1.16 1.16 ...
 - attr(*, "spec")=
  .. cols(
  ..   ...1 = col_double(),
  ..   timestamp = col_datetime(format = ""),
  ..   SW_Temp_PT_C = col_double(),
  ..   yearMonth = col_character(),
  ..   SW_Level_ft = col_double()
  .. )
 - attr(*, "problems")=<externalptr> 
 ```
::::::::::::::::::::::::
::::::::::::::::::::::::::::::::: exercise

What timezone is our data in? Since this dataset is from Kansas, we want our data to be plotted in Central time.

```r
head(konza_sw$timestamp)
```

:::::::::::::::::::::::: solution

```output
[1] "2021-06-09 15:10:00 UTC" "2021-06-09 15:20:00 UTC" "2021-06-09 15:30:00 UTC"
[4] "2021-06-09 15:50:00 UTC" "2021-06-09 16:00:00 UTC" "2021-06-09 16:10:00 UTC"
```

::::::::::::::::::::::::
::::::::::::::::::::::::::::::::: exercise

Since the data is in UTC, we can change the data to CT 


```r
konza_sw$timestamp<- as.POSIXct(konza_sw$timestamp, format="%Y-%m-%d", tz='America/Chicago')
konza_gw$timestamp<- as.POSIXct(konza_gw$timestamp, format="%Y-%m-%d", tz='America/Chicago')
#check what timezone our data is in now
head(konza_gw)
head(konza_sw)
```

:::::::::::::::::::::::: solution

```output
[1] "2021-06-09 10:10:00 CDT" "2021-06-09 10:20:00 CDT" "2021-06-09 10:30:00 CDT"
[4] "2021-06-09 10:50:00 CDT" "2021-06-09 11:00:00 CDT" "2021-06-09 11:10:00 CDT
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

## Plot the data

 Now, let's plot the surface water level data.

 ::::::::::::::::::::::::::::::::: exercise
 
```r
ggplot(data= konza_sw)+
 geom_line(aes(x=timestamp, y=SW_level_ft))+
 xlab("Time")+
 ylab("Surface Water Level (ft)")
```

:::::::::::::::::::::::: solution 
 
```output
The plot!
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

Let's plot the surface water temperature data.

 ::::::::::::::::::::::::::::::::: exercise
 
```r
ggplot(data= konza_sw)+
 geom_line(aes(x=timestamp, y=SW_Temp_PT_C))+
 xlab("Time")+
 ylab("Surface Water Level (ft)")
```

:::::::::::::::::::::::: solution 

```output
The plot!
```

:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::: challenge 

## Exercise 1: Plot the groundwater dataset and identify outliers.

Fill in the blank: Using code from plotting above to recreate the water level and temperature plots using the groundwater dataset. Visually identify outliers.

```r
ggplot(data= _____)+
 geom_line(aes(x=xxxx, y=yyyy))+
 xlab("Time")+
 ylab("Temperature (degrees symbol C)")

ggplot(data= _____)+
 geom_line(aes(x=xxxx, y=yyyy))+
 xlab("Time")+
 ylab("Water Level (ft)")
```

:::::::::::::::::::::::: solution 

## Output
 
```output
The plots!
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::: discussion

Prompt for the learners to discuss.

:::::::::::::::::::::::::::::::::
:::::::::::::::::::::::::::::::::

Looks like we have outliers in our datasets. It is a good idea to zoom in on the outliers and see if there is something weird happening at that time. We can do this by subsetting our data. 

::::::::::::::::::::::::::::::::::::: challenge 

## Exercise 2: Which piece of code would you use to subset the outliers? Which piece of code would you use to subset NA values? Use this code to change outliers to NA.

Test the following code using the surface water dataset followed by the groundwater dataset. Don't forget to change the dataframe when evaluating the groundwater dataset:

```r
a) konza_sw[c(67742:67830),]
b) konza_swe[konza_sw$datetime > "2022-09-23" & konza_sw$datetime< "2022-09-25",]
c) the tydyr method I don't know (subset?)
d) konza_sw$SW_level_ft[is.na(konza_sw$SW_level_ft),]
```

:::::::::::::::::::::::: solution 

## Output

```output
What worked and what didn't work? What did these methods show?
```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Replace bad data values

Now that we know there are numerous weird values, we want to remove those from our dataset by setting them to NA. For surface water level we will remove values below 0 but for temperatures, we will set a low threshold where we will assume the value is incorrect. 

::::::::::::::::::::::::::::::::: exercise

```r
konza_sw$SW_Level_ft[konza_sw$SW_Level_ft< 0]<- NA
konza_sw$SW_Temp_PT_C[konza_sw$SW_Temp_PT_C< -100]<- NA

konza_sw$GW_Level_ft[konza_sw$GW_Level_ft< 0]<- NA
konza_sw$GW_Temp_PT_C[konza_sw$GW_Temp_PT_C< -100]<- NA

```

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Figures

You can also include figures generated from R Markdown:

```{r pyramid, fig.alt = "pie chart illusion of a pyramid", fig.cap = "Sun arise each and every morning"}
pie(
  c(Sky = 78, "Sunny side of pyramid" = 17, "Shady side of pyramid" = 5), 
  init.angle = 315, 
  col = c("deepskyblue", "yellow", "yellow3"), 
  border = FALSE
)
```

Or you can use standard markdown for static figures with the following syntax:

`![optional caption that appears below the figure](figure url){alt='alt text for
accessibility purposes'}`

![You belong in The Carpentries!](https://raw.githubusercontent.com/carpentries/logo/master/Badge_Carpentries.svg){alt='Blue Carpentries hex person logo with no text.'}

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides": 
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::


## Math

One of our episodes contains $\LaTeX$ equations when describing how to create
dynamic reports with {knitr}, so we now use mathjax to describe this:

`$\alpha = \dfrac{1}{(1 - \beta)^2}$` becomes: $\alpha = \dfrac{1}{(1 - \beta)^2}$

Cool, right?

::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
