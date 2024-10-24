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

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Plot the other datasets (UTC and CT) and identify outliers.

Fill in the blank: Using code from plotting recreate the plot and visually identify outliers. 

```r
ggplot(data= _____)+
 geom_line(aes(x=xxxx, y=yyyy))+
 xlab("Time")+
 ylab("Temperature (degrees symbol C)")
```

:::::::::::::::::::::::: solution 

## Output
 
```output
The plot!
```
:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

::: discussion

Prompt for the learners to discuss.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Challenge 2: Which piece of code would you use to subset the outliers? Which piece of code would you use to subset NA values? Use this code to change outliers to NA.

:::::::::::::::::::::::: solution 

a) df[c(),]
b) df[df$datetime> "date" & df$datetime< "date",]
c) the tydyr method I don't know (subset?)
d) df$field[is.na(df$field),]
e) method I don't know to find NA values

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
