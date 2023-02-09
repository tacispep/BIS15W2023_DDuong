---
title: "Lab 8 Homework"
author: "Darren Duong"
date: "2023-02-09"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
```

## Install `here`
The package `here` is a nice option for keeping directories clear when loading files. I will demonstrate below and let you decide if it is something you want to use.  

```r
#install.packages("here")
```

## Data
For this homework, we will use a data set compiled by the Office of Environment and Heritage in New South Whales, Australia. It contains the enterococci counts in water samples obtained from Sydney beaches as part of the Beachwatch Water Quality Program. Enterococci are bacteria common in the intestines of mammals; they are rarely present in clean water. So, enterococci values are a measurement of pollution. `cfu` stands for colony forming units and measures the number of viable bacteria in a sample [cfu](https://en.wikipedia.org/wiki/Colony-forming_unit).   

This homework loosely follows the tutorial of [R Ladies Sydney](https://rladiessydney.org/). If you get stuck, check it out!  

1. Start by loading the data `sydneybeaches`. Do some exploratory analysis to get an idea of the data structure.

```r
sydneybeaches <- read.csv("C:/Users/darre/OneDrive/Desktop/ECS/BIS015L/BIS15W2023_DDuong/lab8/data/sydneybeaches.csv")
```

If you want to try `here`, first notice the output when you load the `here` library. It gives you information on the current working directory. You can then use it to easily and intuitively load files.

```r
library(here)
```

```
## here() starts at C:/Users/darre/OneDrive/Desktop/ECS/BIS015L/BIS15W2023_DDuong
```

The quotes show the folder structure from the root directory.

```r
sydneybeaches <-read_csv(here("lab8", "data", "sydneybeaches.csv")) %>% janitor::clean_names()
```

```
## Rows: 3690 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (4): Region, Council, Site, Date
## dbl (4): BeachId, Longitude, Latitude, Enterococci (cfu/100ml)
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

2. Are these data "tidy" per the definitions of the tidyverse? How do you know? Are they in wide or long format?

Yes, they are tidy by tidyverse definition. Every column is a variable, every row is an observation of that variable, and every cell created by column and row intersection is a single value. It is in long format as it is much longer.

3. We are only interested in the variables site, date, and enterococci_cfu_100ml. Make a new object focused on these variables only. Name the object `sydneybeaches_long`


```r
sydneybeaches_long <- data.frame(sydneybeaches$site, sydneybeaches$date, sydneybeaches$enterococci_cfu_100ml)
```


4. Pivot the data such that the dates are column names and each beach only appears once. Name the object `sydneybeaches_wide`


```r
sydneybeaches_wide <- sydneybeaches_long %>%
  pivot_wider(
  names_from = sydneybeaches.date,
  values_from = sydneybeaches.site
)
```

```
## Warning: Values from `sydneybeaches.site` are not uniquely identified; output will contain list-cols.
## * Use `values_fn = list` to suppress this warning.
## * Use `values_fn = {summary_fun}` to summarise duplicates.
## * Use the following dplyr code to identify duplicates.
##   {data} %>%
##     dplyr::group_by(sydneybeaches.enterococci_cfu_100ml, sydneybeaches.date) %>%
##     dplyr::summarise(n = dplyr::n(), .groups = "drop") %>%
##     dplyr::filter(n > 1L)
```


5. Pivot the data back so that the dates are data and not column names.
#struggling on this, will ask in class later.

```r
#sydneybeaches_wide <- sydneybeaches_long %>%
  #pivot_longer(names_to = sydneybeaches.site, values_to = #sydneybeaches.date, 2:50)
```


6. We haven't dealt much with dates yet, but separate the date into columns day, month, and year. Do this on the `sydneybeaches_long` data.


```r
sydneybeaches_long <- sydneybeaches_long%>%
  
  separate(sydneybeaches.date, into = c("day", "month", "year"), sep = "/")
```


7. What is the average `enterococci_cfu_100ml` by year for each beach. Think about which data you will use- long or wide.

Probably use wide


```r
sydneybeaches_long_average <- sydneybeaches_long %>%
  pivot_wider(names_from=year,values_from=sydneybeaches.enterococci_cfu_100ml)
```


```r
sydneybeaches_long_average$"2013" <- as.integer(sydneybeaches_long_average$"2013")
mean(sydneybeaches_long_average$"2013", na.rm = TRUE)
```

```
## [1] 50.64618
```

```r
sydneybeaches_long_average$"2014" <- as.integer(sydneybeaches_long_average$"2014")
mean(sydneybeaches_long_average$"2014", na.rm = TRUE)
```

```
## [1] 26.32131
```


```r
sydneybeaches_long_average$"2015" <- as.integer(sydneybeaches_long_average$"2015")
mean(sydneybeaches_long_average$"2015", na.rm = TRUE)
```

```
## [1] 31.16294
```


```r
sydneybeaches_long_average$"2016" <- as.integer(sydneybeaches_long_average$"2016")
mean(sydneybeaches_long_average$"2016", na.rm = TRUE)
```

```
## [1] 42.1542
```


```r
sydneybeaches_long_average$"2017" <- as.integer(sydneybeaches_long_average$"2017")
mean(sydneybeaches_long_average$"2017", na.rm = TRUE)
```

```
## [1] 20.72065
```


```r
sydneybeaches_long_average$"2018" <- as.integer(sydneybeaches_long_average$"2018")
mean(sydneybeaches_long_average$"2018", na.rm = TRUE)
```

```
## [1] 33.13002
```




8. Make the output from question 7 easier to read by pivoting it to wide format.

It's already in wide format.

9. What was the most polluted beach in 2018?


```r
sydneybeaches_long_average %>%
  
  summarize(max(sydneybeaches_long_average$"2018", na.rm=TRUE))
```

```
## # A tibble: 1 × 1
##   `max(sydneybeaches_long_average$"2018", na.rm = TRUE)`
##                                                    <int>
## 1                                                   2100
```

```r
sydneybeaches_long_average %>%
  group_by(sydneybeaches_long_average$"2018" == 2100)
```

```
## # A tibble: 2,656 × 10
## # Groups:   sydneybeaches_long_average$"2018" == 2100 [3]
##    sydneybeaches…¹ day   month `2013` `2014` `2015` `2016` `2017` `2018` sydne…²
##    <chr>           <chr> <chr>  <int>  <int>  <int>  <int>  <int>  <int> <lgl>  
##  1 Clovelly Beach  02    01        19     NA     NA     NA     NA     NA NA     
##  2 Clovelly Beach  06    01         3     NA     NA     NA     NA     NA NA     
##  3 Clovelly Beach  12    01         2     NA     NA     NA     NA     NA NA     
##  4 Clovelly Beach  18    01        13     NA     NA     NA     NA     NA NA     
##  5 Clovelly Beach  30    01         8     NA     NA     NA      8     NA NA     
##  6 Clovelly Beach  05    02         7     NA     NA     NA     NA     NA NA     
##  7 Clovelly Beach  11    02        11     NA     NA     NA     NA     NA NA     
##  8 Clovelly Beach  23    02        97     NA     NA     NA     NA     NA NA     
##  9 Clovelly Beach  07    03         3     NA     NA     NA     NA     NA NA     
## 10 Clovelly Beach  25    03         0     NA     11     NA     NA     NA NA     
## # … with 2,646 more rows, and abbreviated variable names ¹​sydneybeaches.site,
## #   ²​`sydneybeaches_long_average$"2018" == 2100`
```

Little Bay Beach was the most polluted beach in 2018

10. Please complete the class project survey at: [BIS 15L Group Project](https://forms.gle/H2j69Z3ZtbLH3efW6)


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
