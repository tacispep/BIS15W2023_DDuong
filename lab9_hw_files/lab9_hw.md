---
title: "Lab 9 Homework"
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
library(here)
library(naniar)
```

For this homework, we will take a departure from biological data and use data about California colleges. These data are a subset of the national college scorecard (https://collegescorecard.ed.gov/data/). Load the `ca_college_data.csv` as a new object called `colleges`.

```r
colleges <- read.csv("data/ca_college_data.csv")
```

The variables are a bit hard to decipher, here is a key:  

INSTNM: Institution name  
CITY: California city  
STABBR: Location state  
ZIP: Zip code  
ADM_RATE: Admission rate  
SAT_AVG: SAT average score  
PCIP26: Percentage of degrees awarded in Biological And Biomedical Sciences  
COSTT4_A: Annual cost of attendance  
C150_4_POOLED: 4-year completion rate  
PFTFTUG1_EF: Percentage of undergraduate students who are first-time, full-time degree/certificate-seeking undergraduate students  

1. Use your preferred function(s) to have a look at the data and get an idea of its structure. Make sure you summarize NA's and determine whether or not the data are tidy. You may also consider dealing with any naming issues.

```r
colleges %>%
  summarize_all(~sum(is.na(.)))
```

```
##   INSTNM CITY STABBR ZIP ADM_RATE SAT_AVG PCIP26 COSTT4_A C150_4_POOLED
## 1      0    0      0   0      240     276     35      124           221
##   PFTFTUG1_EF
## 1          53
```
Lots of NA's, data should be tidy if NA is considered a value.

```r
college_naming <- c("insti_name", "cali_city", "location_state", "zip_code", "admission_rate", "sat_avg_score", "pdegrees_in_biosci", "annual_cost_attend", "four_year_crate", "pstud_seeking_degree")
names(colleges) <- college_naming
```
Naming issues dealt with.

```r
glimpse(colleges)
```

```
## Rows: 341
## Columns: 10
## $ insti_name           <chr> "Grossmont College", "College of the Sequoias", "…
## $ cali_city            <chr> "El Cajon", "Visalia", "San Mateo", "Ventura", "O…
## $ location_state       <chr> "CA", "CA", "CA", "CA", "CA", "CA", "CA", "CA", "…
## $ zip_code             <chr> "92020-1799", "93277-2214", "94402-3784", "93003-…
## $ admission_rate       <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ sat_avg_score        <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
## $ pdegrees_in_biosci   <dbl> 0.0016, 0.0066, 0.0038, 0.0035, 0.0085, 0.0151, 0…
## $ annual_cost_attend   <int> 7956, 8109, 8278, 8407, 8516, 8577, 8580, 9181, 9…
## $ four_year_crate      <dbl> NA, NA, NA, NA, NA, NA, 0.2334, NA, NA, NA, NA, 0…
## $ pstud_seeking_degree <dbl> 0.3546, 0.5413, 0.3567, 0.3824, 0.2753, 0.4286, 0…
```


2. Which cities in California have the highest number of colleges?


```r
colleges_numbers <- colleges %>%
  group_by(cali_city) %>%
  summarize(tot_colleges=length(insti_name)) %>%
  arrange(tot_colleges)
```

```r
colleges_numbers %>%
  slice_max(tot_colleges)
```

```
## # A tibble: 1 × 2
##   cali_city   tot_colleges
##   <chr>              <int>
## 1 Los Angeles           24
```

Los Angeles has the highest number of colleges.



3. Based on your answer to #2, make a plot that shows the number of colleges in the top 10 cities.

```r
college_10 <- top_n(colleges_numbers, 10)
```

```
## Selecting by tot_colleges
```


```r
ggplot(data=college_10, mapping=aes(y=tot_colleges,x=cali_city))+geom_point()
```

![](lab9_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

4. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest average cost? Where is it located?

```r
colleges %>%
  group_by(cali_city) %>%
  summarize(avg_cost=sum(annual_cost_attend,na.rm=TRUE)) %>%
  slice_max(avg_cost)
```

```
## # A tibble: 1 × 2
##   cali_city   avg_cost
##   <chr>          <int>
## 1 Los Angeles   611936
```
Los Angeles has the highest average cost.


5. Based on your answer to #4, make a plot that compares the cost of the individual colleges in the most expensive city. Bonus! Add UC Davis here to see how it compares :>).

```r
colleges_in_LA <- filter(colleges, cali_city == "Los Angeles")
colleges_in_LA %>%
  ggplot(aes(x=insti_name,y=annual_cost_attend))+geom_point(na.rm=T)
```

![](lab9_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

6. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What do you think this means?

```r
ggplot(colleges, aes(x=admission_rate,y=four_year_crate))+geom_point(na.rm=T)
```

![](lab9_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

Lower admission rate colleges (colleges that are "harder" to get into) have higher 4-year completion rates on average. That makes sense. Nitpicky colleges will only take those that they know can graduate

7. Is there a relationship between cost and four-year completion rate? (You don't need to do the stats, just produce a plot). What do you think this means?

```r
ggplot(colleges, aes(x=four_year_crate,y=annual_cost_attend))+geom_point(na.rm=T)
```

![](lab9_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

It looks like there is a relationship. Higher costs are going to gradually lead to higher four year rates, but with a higher standard deviation. The relationship is not strong.

8. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Make a new data frame that is restricted to UC institutions. You can remove `Hastings College of Law` and `UC San Francisco` as we are only interested in undergraduate institutions.

```r
library(stringr)
```


```r
UCS <- filter(colleges, str_detect(insti_name, "University of California"))
```

Remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```r
univ_calif_final <- UCS[-c(9:10),]
```

Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```r
univ_calif_final %>%
  separate(insti_name, into = c("UNIV", "CAMPUS"), sep="-")
```

```
##                       UNIV        CAMPUS     cali_city location_state
## 1 University of California     San Diego      La Jolla             CA
## 2 University of California        Irvine        Irvine             CA
## 3 University of California     Riverside     Riverside             CA
## 4 University of California   Los Angeles   Los Angeles             CA
## 5 University of California         Davis         Davis             CA
## 6 University of California    Santa Cruz    Santa Cruz             CA
## 7 University of California      Berkeley      Berkeley             CA
## 8 University of California Santa Barbara Santa Barbara             CA
##     zip_code admission_rate sat_avg_score pdegrees_in_biosci annual_cost_attend
## 1      92093         0.3566          1324             0.2165              31043
## 2      92697         0.4065          1206             0.1073              31198
## 3      92521         0.6634          1078             0.1491              31494
## 4 90095-1405         0.1799          1334             0.1548              33078
## 5 95616-8678         0.4228          1218             0.1975              33904
## 6 95064-1011         0.5785          1201             0.1927              34608
## 7      94720         0.1693          1422             0.1053              34924
## 8      93106         0.3577          1281             0.1075              34998
##   four_year_crate pstud_seeking_degree
## 1          0.8724               0.6622
## 2          0.8764               0.7254
## 3          0.7300               0.8111
## 4          0.9112               0.6607
## 5          0.8502               0.6049
## 6          0.7764               0.7856
## 7          0.9165               0.7087
## 8          0.8157               0.7077
```

9. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>%
  slice_max(admission_rate)
```

```
##                           insti_name cali_city location_state zip_code
## 1 University of California-Riverside Riverside             CA    92521
##   admission_rate sat_avg_score pdegrees_in_biosci annual_cost_attend
## 1         0.6634          1078             0.1491              31494
##   four_year_crate pstud_seeking_degree
## 1            0.73               0.8111
```

```r
univ_calif_final %>%
  slice_min(admission_rate)
```

```
##                          insti_name cali_city location_state zip_code
## 1 University of California-Berkeley  Berkeley             CA    94720
##   admission_rate sat_avg_score pdegrees_in_biosci annual_cost_attend
## 1         0.1693          1422             0.1053              34924
##   four_year_crate pstud_seeking_degree
## 1          0.9165               0.7087
```


```r
ggplot(univ_calif_final, aes(insti_name, admission_rate))+geom_point()
```

![](lab9_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

10. If you wanted to get a degree in biological or biomedical sciences, which campus confers the majority of these degrees? Produce a numerical summary and an appropriate plot.

```r
univ_calif_final %>%
  slice_max(pdegrees_in_biosci)
```

```
##                           insti_name cali_city location_state zip_code
## 1 University of California-San Diego  La Jolla             CA    92093
##   admission_rate sat_avg_score pdegrees_in_biosci annual_cost_attend
## 1         0.3566          1324             0.2165              31043
##   four_year_crate pstud_seeking_degree
## 1          0.8724               0.6622
```

```r
univ_calif_final %>%
  slice_min(pdegrees_in_biosci)
```

```
##                          insti_name cali_city location_state zip_code
## 1 University of California-Berkeley  Berkeley             CA    94720
##   admission_rate sat_avg_score pdegrees_in_biosci annual_cost_attend
## 1         0.1693          1422             0.1053              34924
##   four_year_crate pstud_seeking_degree
## 1          0.9165               0.7087
```


```r
univ_calif_final %>%
  ggplot(aes(insti_name,pdegrees_in_biosci))+geom_point()
```

![](lab9_hw_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)
