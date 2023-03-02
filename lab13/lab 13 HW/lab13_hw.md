---
title: "Lab 13 Homework"
author: "Darren Duong"
date: "2023-03-02"
output:
  html_document:
    theme: spacelab
    toc: no
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries  

```r
library(tidyverse)
library(janitor)
library(here)
library(ggmap)
```


```r
#library(albersusa)
#Albersusa is not loading for me. Not sure why.
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  

2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure.  

```r
grizzy <- readr::read_csv("data/bear-sightings.csv")
```

```
## Rows: 494 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (3): bear.id, longitude, latitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
clean_names(grizzy)
```

```
## # A tibble: 494 × 3
##    bear_id longitude latitude
##      <dbl>     <dbl>    <dbl>
##  1       7     -149.     62.7
##  2      57     -153.     58.4
##  3      69     -145.     62.4
##  4      75     -153.     59.9
##  5     104     -143.     61.1
##  6     108     -150.     62.9
##  7     115     -152.     68.0
##  8     116     -147.     62.6
##  9     125     -157.     60.2
## 10     135     -156.     58.9
## # … with 484 more rows
```

```r
summary(grizzy)
```

```
##     bear.id       longitude         latitude    
##  Min.   :   7   Min.   :-166.2   Min.   :55.02  
##  1st Qu.:2569   1st Qu.:-154.2   1st Qu.:58.13  
##  Median :4822   Median :-151.0   Median :60.97  
##  Mean   :4935   Mean   :-149.1   Mean   :61.41  
##  3rd Qu.:7387   3rd Qu.:-145.6   3rd Qu.:64.13  
##  Max.   :9996   Max.   :-131.3   Max.   :70.37
```

```r
anyNA(grizzy)
```

```
## [1] FALSE
```

```r
#Weird to have no NA's, NA might be listed as something else in the data
```


2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.  

```r
lat <- c(55.02, 70.37)
long <- c(-166.2, -131.3)
g_bbox <- make_bbox(long, lat, f = 0.05)
```


3. Load a map from `stamen` in a terrain style projection and display the map.  

```r
mapbear <- get_map(g_bbox, maptype = "terrain", source = "stamen")
```

```
## ℹ Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```

```r
ggmap::ggmap(mapbear)
```

![](lab13_hw_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.  


```r
ggmap(mapbear) + 
  geom_point(data = grizzy, aes(longitude, latitude),size = 0.35, color = "Green") +
  labs(x= "longitude", y= "latitude", title="Grizzly Bears in Alaska")
```

![](lab13_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


Let's switch to the wolves data. Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

5. Load the data and evaluate its structure.  


```r
wolves <- readr::read_csv("data/wolves_data/wolves_dataset.csv")
```

```
## Rows: 1986 Columns: 23
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): pop, age.cat, sex, color
## dbl (19): year, lat, long, habitat, human, pop.density, pack.size, standard....
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
glimpse(wolves)
```

```
## Rows: 1,986
## Columns: 23
## $ pop                <chr> "AK.PEN", "AK.PEN", "AK.PEN", "AK.PEN", "AK.PEN", "…
## $ year               <dbl> 2006, 2006, 2006, 2006, 2006, 2006, 2006, 2006, 200…
## $ age.cat            <chr> "S", "S", "A", "S", "A", "A", "A", "P", "S", "P", "…
## $ sex                <chr> "F", "M", "F", "M", "M", "M", "F", "M", "F", "M", "…
## $ color              <chr> "G", "G", "G", "B", "B", "G", "G", "G", "G", "G", "…
## $ lat                <dbl> 57.03983, 57.03983, 57.03983, 57.03983, 57.03983, 5…
## $ long               <dbl> -157.8427, -157.8427, -157.8427, -157.8427, -157.84…
## $ habitat            <dbl> 254.08, 254.08, 254.08, 254.08, 254.08, 254.08, 254…
## $ human              <dbl> 10.42, 10.42, 10.42, 10.42, 10.42, 10.42, 10.42, 10…
## $ pop.density        <dbl> 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, 8, …
## $ pack.size          <dbl> 8.78, 8.78, 8.78, 8.78, 8.78, 8.78, 8.78, 8.78, 8.7…
## $ standard.habitat   <dbl> -1.6339, -1.6339, -1.6339, -1.6339, -1.6339, -1.633…
## $ standard.human     <dbl> -0.9784, -0.9784, -0.9784, -0.9784, -0.9784, -0.978…
## $ standard.pop       <dbl> -0.6827, -0.6827, -0.6827, -0.6827, -0.6827, -0.682…
## $ standard.packsize  <dbl> 1.3157, 1.3157, 1.3157, 1.3157, 1.3157, 1.3157, 1.3…
## $ standard.latitude  <dbl> 0.7214, 0.7214, 0.7214, 0.7214, 0.7214, 0.7214, 0.7…
## $ standard.longitude <dbl> -2.1441, -2.1441, -2.1441, -2.1441, -2.1441, -2.144…
## $ cav.binary         <dbl> 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
## $ cdv.binary         <dbl> 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
## $ cpv.binary         <dbl> 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, …
## $ chv.binary         <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, …
## $ neo.binary         <dbl> NA, NA, NA, 0, 0, NA, NA, 1, 0, 1, NA, 0, NA, NA, N…
## $ toxo.binary        <dbl> NA, NA, NA, 1, 0, NA, NA, 1, 0, 0, NA, 0, NA, NA, N…
```

```r
clean_names(wolves)
```

```
## # A tibble: 1,986 × 23
##    pop     year age_cat sex   color   lat  long habitat human pop_dens…¹ pack_…²
##    <chr>  <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>      <dbl>   <dbl>
##  1 AK.PEN  2006 S       F     G      57.0 -158.    254.  10.4          8    8.78
##  2 AK.PEN  2006 S       M     G      57.0 -158.    254.  10.4          8    8.78
##  3 AK.PEN  2006 A       F     G      57.0 -158.    254.  10.4          8    8.78
##  4 AK.PEN  2006 S       M     B      57.0 -158.    254.  10.4          8    8.78
##  5 AK.PEN  2006 A       M     B      57.0 -158.    254.  10.4          8    8.78
##  6 AK.PEN  2006 A       M     G      57.0 -158.    254.  10.4          8    8.78
##  7 AK.PEN  2006 A       F     G      57.0 -158.    254.  10.4          8    8.78
##  8 AK.PEN  2006 P       M     G      57.0 -158.    254.  10.4          8    8.78
##  9 AK.PEN  2006 S       F     G      57.0 -158.    254.  10.4          8    8.78
## 10 AK.PEN  2006 P       M     G      57.0 -158.    254.  10.4          8    8.78
## # … with 1,976 more rows, 12 more variables: standard_habitat <dbl>,
## #   standard_human <dbl>, standard_pop <dbl>, standard_packsize <dbl>,
## #   standard_latitude <dbl>, standard_longitude <dbl>, cav_binary <dbl>,
## #   cdv_binary <dbl>, cpv_binary <dbl>, chv_binary <dbl>, neo_binary <dbl>,
## #   toxo_binary <dbl>, and abbreviated variable names ¹​pop_density, ²​pack_size
```

```r
anyNA(wolves)
```

```
## [1] TRUE
```

```r
#NA's present
```


6. How many distinct wolf populations are included in this study? Make a new object that restricts the data to the wolf populations in the lower 48 US states.  


```r
d_wolves <- wolves %>%
  filter(lat<50)
```


7. Use the range of the latitude and longitude to build an appropriate bounding box for your map.  

```r
max(d_wolves$lat)
```

```
## [1] 47.74968
```

```r
min(d_wolves$lat)
```

```
## [1] 33.88778
```

```r
max(d_wolves$long)
```

```
## [1] -86.81887
```

```r
min(d_wolves$long)
```

```
## [1] -110.9924
```

```r
lat <- c(33.88778, 47.74968)
long <- c(-110.9924, -86.81887)
w_bbox <- make_bbox(long, lat, f = 0.05)
```


8.  Load a map from `stamen` in a `terrain-lines` projection and display the map.  


```r
mapwolves <- get_map(w_bbox, maptype = "terrain-lines", source = "stamen")
```

```
## ℹ Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```

```r
ggmap::ggmap(mapwolves)
```

![](lab13_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


9. Build a final map that overlays the recorded observations of wolves in the lower 48 states.  

```r
ggmap::ggmap(mapwolves)+
  geom_point(data=d_wolves,aes(x=long,y=lat))+
  labs(title="Wolves in the 48 Lower States", x="Longitude", y="Latitude")
```

![](lab13_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


10. Use the map from #9 above, but add some aesthetics. Try to `fill` and `color` by population.  


```r
ggmap::ggmap(mapwolves)+
  geom_point(data=d_wolves,aes(x=long,y=lat,fill = pop, color = pop),size=3.8, alpha=0.65)+
  labs(title="Wolves in the 48 Lower States", x="Longitude", y="Latitude")+
  theme_classic()
```

![](lab13_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
