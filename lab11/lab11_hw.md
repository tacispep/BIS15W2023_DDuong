---
title: "Lab 11 Homework"
author: "Please Add Your Name Here"
date: "2023-02-20"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
#install.packages("gapminder")
library("gapminder")
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  


```r
gapminder %>%
  clean_names()
```

```
## # A tibble: 1,704 × 6
##    country     continent  year life_exp      pop gdp_percap
##    <fct>       <fct>     <int>    <dbl>    <int>      <dbl>
##  1 Afghanistan Asia       1952     28.8  8425333       779.
##  2 Afghanistan Asia       1957     30.3  9240934       821.
##  3 Afghanistan Asia       1962     32.0 10267083       853.
##  4 Afghanistan Asia       1967     34.0 11537966       836.
##  5 Afghanistan Asia       1972     36.1 13079460       740.
##  6 Afghanistan Asia       1977     38.4 14880372       786.
##  7 Afghanistan Asia       1982     39.9 12881816       978.
##  8 Afghanistan Asia       1987     40.8 13867957       852.
##  9 Afghanistan Asia       1992     41.7 16317921       649.
## 10 Afghanistan Asia       1997     41.8 22227415       635.
## # … with 1,694 more rows
```

```r
gapminder <- gapminder
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

```r
any_na(gapminder)
```

```
## [1] FALSE
```

```r
#It looks like there aren't any NA's
```


**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**


```r
gapminder %>%
  group_by(year)%>%
  ggplot(aes(lifeExp,x=year))+geom_point()+ scale_fill_gradient(low="red")
```

![](lab11_hw_files/figure-html/unnamed-chunk-4-1.png)<!-- -->
Slowly rose over the years.

**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**


```r
gapminder %>%
  group_by(year)%>%
  ggplot(aes(lifeExp,x=year))+geom_point()+ scale_fill_gradient(low="red")
```

![](lab11_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
Towards 1950, the life expectancy fell much denser near the 40's and under. Later years (2007) had the density of life expectancy fally much higher, towards the 65's.

**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**


```r
gapminder %>%
  group_by(continent)%>%
  summarize(min=min(lifeExp),max=max(lifeExp),mean=mean(lifeExp))
```

```
## # A tibble: 5 × 4
##   continent   min   max  mean
##   <fct>     <dbl> <dbl> <dbl>
## 1 Africa     23.6  76.4  48.9
## 2 Americas   37.6  80.7  64.7
## 3 Asia       28.8  82.6  60.1
## 4 Europe     43.6  81.8  71.9
## 5 Oceania    69.1  81.2  74.3
```


**5. How has life expectancy changed between 1952-2007 for each continent?**


```r
gapminder %>%
  group_by(continent)%>%
  summarize(min=min(lifeExp),max=max(lifeExp),mean=mean(lifeExp))%>%
  ggplot(aes(x=continent,y=mean))+geom_col(color="white",fill="blue",linetype=1)
```

![](lab11_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**


```r
gapminder %>%
  ggplot(aes(x=gdpPercap,y=lifeExp))+geom_line()
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
It looks like there is a relationship between GDP per capita and life expectancy up until the 30,000 GDP per Capita mark. Past that, the relationship does not hold true.

**7. Which countries have had the largest population growth since 1952?**


```r
gapminder_pop_growth <- gapminder %>%
  group_by(country) %>%
  summarise(pop_growth=max(pop)/min(pop))
```


```r
gapminder_top_5_pop <- gapminder_pop_growth %>%
  slice_max(pop_growth, n=5)
```

The top 5 largest populations:

Kuwait

Jordan

Djibouti

Saudi Arabia

Oman

**8. Use your results from the question above to plot population growth for the top five countries since 1952.**


```r
gapminder_top_5_pop %>%
  ggplot(aes(y=country,x=pop_growth))+geom_col(fill="white",color="black",linetype=2)+coord_flip()
```

![](lab11_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


**9. How does per-capita GDP growth compare between these same five countries?**


```r
gapminder %>%
  group_by(country, year) %>%
  filter(country %in% c("Kuwait","Jordan","Oman","Saudi Arabia","Djibouti")) %>%
  summarise(gdpPercap)
```

```
## `summarise()` has grouped output by 'country'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 60 × 3
## # Groups:   country [5]
##    country   year gdpPercap
##    <fct>    <int>     <dbl>
##  1 Djibouti  1952     2670.
##  2 Djibouti  1957     2865.
##  3 Djibouti  1962     3021.
##  4 Djibouti  1967     3020.
##  5 Djibouti  1972     3694.
##  6 Djibouti  1977     3082.
##  7 Djibouti  1982     2879.
##  8 Djibouti  1987     2880.
##  9 Djibouti  1992     2377.
## 10 Djibouti  1997     1895.
## # … with 50 more rows
```


**10. Make one plot of your choice that uses faceting!**


```r
gapminder %>%
  group_by(country, year) %>%
  filter(country %in% c("Kuwait","Jordan","Oman","Saudi Arabia","Djibouti")) %>%
  ggplot(aes(x=country,y=pop,fill=continent))+geom_point()+facet_grid(~continent)+coord_flip()+theme(strip.text.y=element_text(size=32))
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
