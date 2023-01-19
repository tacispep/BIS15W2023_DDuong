---
title: "Lab 3 Homework"
author: "Darren Duong"
date: "2023-01-18"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes

---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
??mammal
```

2. Store these data into a new data frame `sleep`.

```r
sleep <- msleep
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim(sleep)#simple way of showing the dimensions
```

```
## [1] 83 11
```

```r
#different, but equally useful way of finding out dimensions
nrow(sleep)#observations
```

```
## [1] 83
```

```r
ncol(sleep)#variables
```

```
## [1] 11
```

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
anyNA(sleep)
```

```
## [1] TRUE
```

```r
#There are NA's in the sleep.
```

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
herbivores<-sleep[,3] == "herbi" #random variable that stores the type of vore as true, false, or NA based on whether it is a herbivore or not
table(herbivores)["TRUE"] #counts number of True statements, indicating number of herbivores
```

```
## TRUE 
##   32
```

```r
#There are 32 herbivores
```

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small_mammals <- subset(sleep, sleep[,11]<= 1)
large_mammals <- subset(sleep, sleep[,11]>=200)

anyNA(small_mammals)
```

```
## [1] TRUE
```

8. What is the mean weight for both the small and large mammals?

```r
summary(small_mammals[,11])#mean is listed as 0.25967
```

```
##      bodywt       
##  Min.   :0.00500  
##  1st Qu.:0.04475  
##  Median :0.11600  
##  Mean   :0.25967  
##  3rd Qu.:0.38250  
##  Max.   :1.00000
```


```r
summary(large_mammals[,11])#mean is listed as 1747.1
```

```
##      bodywt      
##  Min.   : 207.5  
##  1st Qu.: 560.5  
##  Median : 800.0  
##  Mean   :1747.1  
##  3rd Qu.:1723.5  
##  Max.   :6654.0
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  

```r
summary(small_mammals[,6])
```

```
##   sleep_total   
##  Min.   : 7.00  
##  1st Qu.: 9.75  
##  Median :12.65  
##  Mean   :12.66  
##  3rd Qu.:14.90  
##  Max.   :19.90
```

```r
#Average of 12.66 hours, Small mammals sleep longer at 12.66 hours average
```


```r
summary(large_mammals[,6])
```

```
##   sleep_total  
##  Min.   :1.90  
##  1st Qu.:2.80  
##  Median :3.30  
##  Mean   :3.30  
##  3rd Qu.:3.95  
##  Max.   :4.40
```

```r
#Average of 3.3 hours, Small mammals sleep longer at 12.66 hours average
```

10. Which animal is the sleepiest among the entire dataframe?

```r
max(sleep[,6])
```

```
## [1] 19.9
```

```r
#the sleepiest animal sleeps 19.9 hours total. Time to find which that is
sleep[,6] == 19.9
```

```
##       sleep_total
##  [1,]       FALSE
##  [2,]       FALSE
##  [3,]       FALSE
##  [4,]       FALSE
##  [5,]       FALSE
##  [6,]       FALSE
##  [7,]       FALSE
##  [8,]       FALSE
##  [9,]       FALSE
## [10,]       FALSE
## [11,]       FALSE
## [12,]       FALSE
## [13,]       FALSE
## [14,]       FALSE
## [15,]       FALSE
## [16,]       FALSE
## [17,]       FALSE
## [18,]       FALSE
## [19,]       FALSE
## [20,]       FALSE
## [21,]       FALSE
## [22,]       FALSE
## [23,]       FALSE
## [24,]       FALSE
## [25,]       FALSE
## [26,]       FALSE
## [27,]       FALSE
## [28,]       FALSE
## [29,]       FALSE
## [30,]       FALSE
## [31,]       FALSE
## [32,]       FALSE
## [33,]       FALSE
## [34,]       FALSE
## [35,]       FALSE
## [36,]       FALSE
## [37,]       FALSE
## [38,]       FALSE
## [39,]       FALSE
## [40,]       FALSE
## [41,]       FALSE
## [42,]       FALSE
## [43,]        TRUE
## [44,]       FALSE
## [45,]       FALSE
## [46,]       FALSE
## [47,]       FALSE
## [48,]       FALSE
## [49,]       FALSE
## [50,]       FALSE
## [51,]       FALSE
## [52,]       FALSE
## [53,]       FALSE
## [54,]       FALSE
## [55,]       FALSE
## [56,]       FALSE
## [57,]       FALSE
## [58,]       FALSE
## [59,]       FALSE
## [60,]       FALSE
## [61,]       FALSE
## [62,]       FALSE
## [63,]       FALSE
## [64,]       FALSE
## [65,]       FALSE
## [66,]       FALSE
## [67,]       FALSE
## [68,]       FALSE
## [69,]       FALSE
## [70,]       FALSE
## [71,]       FALSE
## [72,]       FALSE
## [73,]       FALSE
## [74,]       FALSE
## [75,]       FALSE
## [76,]       FALSE
## [77,]       FALSE
## [78,]       FALSE
## [79,]       FALSE
## [80,]       FALSE
## [81,]       FALSE
## [82,]       FALSE
## [83,]       FALSE
```

```r
#the 43rd animal sleeps the most
sleep[43,1]
```

```
## # A tibble: 1 × 1
##   name            
##   <chr>           
## 1 Little brown bat
```

```r
#That animal is the Little Brown Bat
```



## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
