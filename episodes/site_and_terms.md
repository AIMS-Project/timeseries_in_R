---
title: "Define common timeseries terms and familiarize yourself with the field site"
teaching: XX
exercises: XX
---

:::::::::::::::::::::::::::::::::::::: questions 

- What can you expect from this training?
- What is a timeseries and why are they important?
- Where are these data coming from?

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

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

To get familiar with the data, let's first make sure we remember how to create an R Project!

::::::::::::::::::::::::::::::::::::: challenge 

Using file explorer, create a new file and R project for this lesson. 

::::::::::::::::::::::::::::::::::::: discussion 

What files would be good to include?

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: solution 

Typically, we encourage to make files for:
1. data
2. data_processed
3. scripts

:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Exercise #1 

Can you find the Konza Prairie Biological Station on Google Maps? 
What state did this project take place in? What is the closest city?

:::::::::::::::::::::::: solution 

This project took place in Kansas, USA. 
The closest major city is Manhattan, KS. 
 
:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: challenge 

## Exercise #2

How do you upload a .csv file into R?

```r
read.csv("C://", header = TRUE)
```

:::::::::::::::::::::::: solution 

## Output
 
```output
[1] "This new lesson looks good"
```

:::::::::::::::::::::::::::::::::



## Challenge 2: how do you nest solutions within challenge blocks?

:::::::::::::::::::::::: solution 

You can add a line with at least three colons and a `solution` tag.

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
