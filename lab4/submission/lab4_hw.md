---
title: "Lab 4 Homework"
author: "Darren Duong"
date: "2023-01-23"
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

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- readr::read_csv("C:/Users/darre/OneDrive/Desktop/ECS/BIS015L/BIS15W2023_DDuong/lab4/data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## Rows: 569 Columns: 24
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (16): taxon, common.name, class, order, family, genus, species, primarym...
## dbl  (8): mean.mass.g, log10.mass, mean.hra.m2, log10.hra, dimension, preyma...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
class(homerange)
```

```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```

```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild        dimension        preymass         log10.preymass   
##  Length:569         Min.   :2.000   Min.   :     0.67   Min.   :-0.1739  
##  Class :character   1st Qu.:2.000   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Median :2.000   Median :    53.75   Median : 1.7304  
##                     Mean   :2.218   Mean   :  3989.88   Mean   : 2.0188  
##                     3rd Qu.:2.000   3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                     Max.   :3.000   Max.   :130233.20   Max.   : 5.1147  
##                                     NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```


```r
dim(homerange)
```

```
## [1] 569  24
```

```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

```r
str(homerange)
```

```
## spc_tbl_ [569 × 24] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ taxon                     : chr [1:569] "lake fishes" "river fishes" "river fishes" "river fishes" ...
##  $ common.name               : chr [1:569] "american eel" "blacktail redhorse" "central stoneroller" "rosyside dace" ...
##  $ class                     : chr [1:569] "actinopterygii" "actinopterygii" "actinopterygii" "actinopterygii" ...
##  $ order                     : chr [1:569] "anguilliformes" "cypriniformes" "cypriniformes" "cypriniformes" ...
##  $ family                    : chr [1:569] "anguillidae" "catostomidae" "cyprinidae" "cyprinidae" ...
##  $ genus                     : chr [1:569] "anguilla" "moxostoma" "campostoma" "clinostomus" ...
##  $ species                   : chr [1:569] "rostrata" "poecilura" "anomalum" "funduloides" ...
##  $ primarymethod             : chr [1:569] "telemetry" "mark-recapture" "mark-recapture" "mark-recapture" ...
##  $ N                         : chr [1:569] "16" NA "20" "26" ...
##  $ mean.mass.g               : num [1:569] 887 562 34 4 4 ...
##  $ log10.mass                : num [1:569] 2.948 2.75 1.531 0.602 0.602 ...
##  $ alternative.mass.reference: chr [1:569] NA NA NA NA ...
##  $ mean.hra.m2               : num [1:569] 282750 282.1 116.1 125.5 87.1 ...
##  $ log10.hra                 : num [1:569] 5.45 2.45 2.06 2.1 1.94 ...
##  $ hra.reference             : chr [1:569] "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 "Minns, C. K. 1995. Allometry of home range size in lake and river fishes. Canadian Journal of Fisheries and Aquatic Sciences 52 ...
##  $ realm                     : chr [1:569] "aquatic" "aquatic" "aquatic" "aquatic" ...
##  $ thermoregulation          : chr [1:569] "ectotherm" "ectotherm" "ectotherm" "ectotherm" ...
##  $ locomotion                : chr [1:569] "swimming" "swimming" "swimming" "swimming" ...
##  $ trophic.guild             : chr [1:569] "carnivore" "carnivore" "carnivore" "carnivore" ...
##  $ dimension                 : num [1:569] 3 2 2 2 2 2 2 2 2 2 ...
##  $ preymass                  : num [1:569] NA NA NA NA NA NA 1.39 NA NA NA ...
##  $ log10.preymass            : num [1:569] NA NA NA NA NA ...
##  $ PPMR                      : num [1:569] NA NA NA NA NA NA 530 NA NA NA ...
##  $ prey.size.reference       : chr [1:569] NA NA NA NA ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   taxon = col_character(),
##   ..   common.name = col_character(),
##   ..   class = col_character(),
##   ..   order = col_character(),
##   ..   family = col_character(),
##   ..   genus = col_character(),
##   ..   species = col_character(),
##   ..   primarymethod = col_character(),
##   ..   N = col_character(),
##   ..   mean.mass.g = col_double(),
##   ..   log10.mass = col_double(),
##   ..   alternative.mass.reference = col_character(),
##   ..   mean.hra.m2 = col_double(),
##   ..   log10.hra = col_double(),
##   ..   hra.reference = col_character(),
##   ..   realm = col_character(),
##   ..   thermoregulation = col_character(),
##   ..   locomotion = col_character(),
##   ..   trophic.guild = col_character(),
##   ..   dimension = col_double(),
##   ..   preymass = col_double(),
##   ..   log10.preymass = col_double(),
##   ..   PPMR = col_double(),
##   ..   prey.size.reference = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```


**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  


```r
homerange$taxon <- as.factor(homerange$taxon)
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
homerange$order <- as.factor(homerange$order)
levels(homerange$order)
```

```
##  [1] "accipitriformes"       "afrosoricida"          "anguilliformes"       
##  [4] "anseriformes"          "apterygiformes"        "artiodactyla"         
##  [7] "caprimulgiformes"      "carnivora"             "charadriiformes"      
## [10] "columbidormes"         "columbiformes"         "coraciiformes"        
## [13] "cuculiformes"          "cypriniformes"         "dasyuromorpha"        
## [16] "dasyuromorpia"         "didelphimorphia"       "diprodontia"          
## [19] "diprotodontia"         "erinaceomorpha"        "esociformes"          
## [22] "falconiformes"         "gadiformes"            "galliformes"          
## [25] "gruiformes"            "lagomorpha"            "macroscelidea"        
## [28] "monotrematae"          "passeriformes"         "pelecaniformes"       
## [31] "peramelemorphia"       "perciformes"           "perissodactyla"       
## [34] "piciformes"            "pilosa"                "proboscidea"          
## [37] "psittaciformes"        "rheiformes"            "roden"                
## [40] "rodentia"              "salmoniformes"         "scorpaeniformes"      
## [43] "siluriformes"          "soricomorpha"          "squamata"             
## [46] "strigiformes"          "struthioniformes"      "syngnathiformes"      
## [49] "testudines"            "tinamiformes"          "tetraodontiformes\xa0"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa_names <- c("Taxon", "Common Name", "Class", "Order", "Family", "Genus", "Species")
taxa <- data.frame(homerange$taxon, homerange$common.name, homerange$class, homerange$order, homerange$family, homerange$genus, homerange$species)
colnames(taxa) <- taxa_names
```


**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  


```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```


**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.** 


```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  


```r
carns <- filter(homerange, homerange$trophic.guild == "carnivore")
herbs <- filter(homerange, homerange$trophic.guild == "herbivore")
carn_frame <- data.frame(carns)
herb_frame <- data.frame(herbs)
```


**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  

Herbivores have a larger mean.hra.m2 average

```r
mean(herb_frame$mean.hra.m2, na.rm = TRUE)
```

```
## [1] 34137012
```

```r
mean(carn_frame$mean.hra.m2, na.rm = TRUE)
```

```
## [1] 13039918
```


**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  

The Moose is the largest deer. 


```r
deer_family <- homerange[homerange$family == "cervidae", ]

deer_family[order(deer_family$log10.mass, decreasing = TRUE)]
```

```
## # A tibble: 12 × 12
##    taxon  order alter…¹ N     genus commo…² prima…³ mean.…⁴ family class species
##    <fct>  <fct> <chr>   <chr> <chr> <chr>   <chr>     <dbl> <chr>  <chr> <chr>  
##  1 mamma… arti… <NA>    <NA>  alces moose   teleme… 307227. cervi… mamm… alces  
##  2 mamma… arti… <NA>    <NA>  axis  chital  teleme…  62823. cervi… mamm… axis   
##  3 mamma… arti… <NA>    <NA>  capr… roe de… teleme…  24050. cervi… mamm… capreo…
##  4 mamma… arti… <NA>    <NA>  cerv… red de… teleme… 234758. cervi… mamm… elaphus
##  5 mamma… arti… <NA>    <NA>  cerv… sika d… teleme…  29450. cervi… mamm… nippon 
##  6 mamma… arti… <NA>    <NA>  dama  fallow… teleme…  71450. cervi… mamm… dama   
##  7 mamma… arti… <NA>    <NA>  munt… Reeves… teleme…  13500. cervi… mamm… reevesi
##  8 mamma… arti… <NA>    <NA>  odoc… mule d… teleme…  53864. cervi… mamm… hemion…
##  9 mamma… arti… <NA>    <NA>  odoc… white-… teleme…  87884. cervi… mamm… virgin…
## 10 mamma… arti… <NA>    <NA>  ozot… pampas… teleme…  35000. cervi… mamm… bezoar…
## 11 mamma… arti… <NA>    <NA>  pudu  pudu    teleme…   7500. cervi… mamm… puda   
## 12 mamma… arti… <NA>    <NA>  rang… reinde… teleme… 102059. cervi… mamm… tarand…
## # … with 1 more variable: log10.mass <dbl>, and abbreviated variable names
## #   ¹​alternative.mass.reference, ²​common.name, ³​primarymethod, ⁴​mean.mass.g
```


**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**  

The western worm snake seems to be the smallest. Based on the shape of the western worm snakes head, we can tell that it's actually non-venomous. It's an extremely small, round headed snake that's actually very popular as a pet. Similar to its name, worm snakes are commonly confused with worms.


```r
snake_data <- homerange[homerange$taxon == "snakes", ]
summary(snake_data)
```

```
##            taxon    common.name           class                       order   
##  snakes       :41   Length:41          Length:41          squamata       :41  
##  birds        : 0   Class :character   Class :character   accipitriformes: 0  
##  lake fishes  : 0   Mode  :character   Mode  :character   afrosoricida   : 0  
##  lizards      : 0                                         anguilliformes : 0  
##  mammals      : 0                                         anseriformes   : 0  
##  marine fishes: 0                                         apterygiformes : 0  
##  (Other)      : 0                                         (Other)        : 0  
##     family             genus             species          primarymethod     
##  Length:41          Length:41          Length:41          Length:41         
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass    
##  Length:41          Min.   :   3.46   Min.   :0.5391  
##  Class :character   1st Qu.: 106.70   1st Qu.:2.0282  
##  Mode  :character   Median : 234.10   Median :2.3694  
##                     Mean   : 303.62   Mean   :2.2261  
##                     3rd Qu.: 375.00   3rd Qu.:2.5740  
##                     Max.   :1226.85   Max.   :3.0888  
##                                                       
##  alternative.mass.reference  mean.hra.m2        log10.hra    
##  Length:41                  Min.   :    200   Min.   :2.301  
##  Class :character           1st Qu.:  22900   1st Qu.:4.360  
##  Mode  :character           Median :  77400   Median :4.889  
##                             Mean   : 258416   Mean   :4.715  
##                             3rd Qu.: 240400   3rd Qu.:5.381  
##                             Max.   :2579600   Max.   :6.412  
##                                                              
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:41          Length:41          Length:41          Length:41         
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild        dimension    preymass       log10.preymass  
##  Length:41          Min.   :2   Min.   :   8.97   Min.   :0.9528  
##  Class :character   1st Qu.:2   1st Qu.:  27.39   1st Qu.:1.3783  
##  Mode  :character   Median :2   Median :  51.93   Median :1.7154  
##                     Mean   :2   Mean   : 272.72   Mean   :1.8180  
##                     3rd Qu.:2   3rd Qu.: 129.14   3rd Qu.:2.0978  
##                     Max.   :2   Max.   :2684.21   Max.   :3.4288  
##                                 NA's   :26        NA's   :26      
##       PPMR        prey.size.reference
##  Min.   : 0.380   Length:41          
##  1st Qu.: 3.155   Class :character   
##  Median : 5.740   Mode  :character   
##  Mean   : 8.283                      
##  3rd Qu.:12.460                      
##  Max.   :25.000                      
##  NA's   :26
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
