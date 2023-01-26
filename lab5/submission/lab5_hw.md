---
title: "dplyr Superhero"
date: "2023-01-26"
output:
  html_document: 
    theme: spacelab
    toc: yes
    toc_float: yes
    keep_md: yes
---

## Load the tidyverse

```r
library("tidyverse")
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## Rows: 734 Columns: 10
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (8): name, Gender, Eye color, Race, Hair color, Publisher, Skin color, A...
## dbl (2): Height, Weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## Rows: 667 Columns: 168
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr   (1): hero_names
## lgl (167): Agility, Accelerated Healing, Lantern Power Ring, Dimensional Awa...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
names(superhero_info)
```

```
##  [1] "name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```

```r
#just taking a look at what the names currently are
```


```r
superhero_info <- rename(superhero_info, hair_color = "Hair color", gender = "Gender", height="Height", alignment="Alignment", weight ="Weight", publisher="Publisher",race="Race",skin_color="Skin color")
```

```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "Eye color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 × 168
##   hero_…¹ Agility Accel…² Lante…³ Dimen…⁴ Cold …⁵ Durab…⁶ Stealth Energ…⁷ Flight
##   <chr>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl> 
## 1 3-D Man TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE 
## 2 A-Bomb  FALSE   TRUE    FALSE   FALSE   FALSE   TRUE    FALSE   FALSE   FALSE 
## 3 Abe Sa… TRUE    TRUE    FALSE   FALSE   TRUE    TRUE    FALSE   FALSE   FALSE 
## 4 Abin S… FALSE   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE 
## 5 Abomin… FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   FALSE   FALSE 
## 6 Abraxas FALSE   FALSE   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   TRUE  
## # … with 158 more variables: `Danger Sense` <lgl>,
## #   `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons Master` <lgl>,
## #   `Power Augmentation` <lgl>, `Animal Attributes` <lgl>, Longevity <lgl>,
## #   Intelligence <lgl>, `Super Strength` <lgl>, Cryokinesis <lgl>,
## #   Telepathy <lgl>, `Energy Armor` <lgl>, `Energy Blasts` <lgl>,
## #   Duplication <lgl>, `Size Changing` <lgl>, `Density Control` <lgl>,
## #   Stamina <lgl>, `Astral Travel` <lgl>, `Audio Control` <lgl>, …
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```


```r
names(superhero_powers)
```

```
##   [1] "hero_names"                   "agility"                     
##   [3] "accelerated_healing"          "lantern_power_ring"          
##   [5] "dimensional_awareness"        "cold_resistance"             
##   [7] "durability"                   "stealth"                     
##   [9] "energy_absorption"            "flight"                      
##  [11] "danger_sense"                 "underwater_breathing"        
##  [13] "marksmanship"                 "weapons_master"              
##  [15] "power_augmentation"           "animal_attributes"           
##  [17] "longevity"                    "intelligence"                
##  [19] "super_strength"               "cryokinesis"                 
##  [21] "telepathy"                    "energy_armor"                
##  [23] "energy_blasts"                "duplication"                 
##  [25] "size_changing"                "density_control"             
##  [27] "stamina"                      "astral_travel"               
##  [29] "audio_control"                "dexterity"                   
##  [31] "omnitrix"                     "super_speed"                 
##  [33] "possession"                   "animal_oriented_powers"      
##  [35] "weapon_based_powers"          "electrokinesis"              
##  [37] "darkforce_manipulation"       "death_touch"                 
##  [39] "teleportation"                "enhanced_senses"             
##  [41] "telekinesis"                  "energy_beams"                
##  [43] "magic"                        "hyperkinesis"                
##  [45] "jump"                         "clairvoyance"                
##  [47] "dimensional_travel"           "power_sense"                 
##  [49] "shapeshifting"                "peak_human_condition"        
##  [51] "immortality"                  "camouflage"                  
##  [53] "element_control"              "phasing"                     
##  [55] "astral_projection"            "electrical_transport"        
##  [57] "fire_control"                 "projection"                  
##  [59] "summoning"                    "enhanced_memory"             
##  [61] "reflexes"                     "invulnerability"             
##  [63] "energy_constructs"            "force_fields"                
##  [65] "self_sustenance"              "anti_gravity"                
##  [67] "empathy"                      "power_nullifier"             
##  [69] "radiation_control"            "psionic_powers"              
##  [71] "elasticity"                   "substance_secretion"         
##  [73] "elemental_transmogrification" "technopath_cyberpath"        
##  [75] "photographic_reflexes"        "seismic_power"               
##  [77] "animation"                    "precognition"                
##  [79] "mind_control"                 "fire_resistance"             
##  [81] "power_absorption"             "enhanced_hearing"            
##  [83] "nova_force"                   "insanity"                    
##  [85] "hypnokinesis"                 "animal_control"              
##  [87] "natural_armor"                "intangibility"               
##  [89] "enhanced_sight"               "molecular_manipulation"      
##  [91] "heat_generation"              "adaptation"                  
##  [93] "gliding"                      "power_suit"                  
##  [95] "mind_blast"                   "probability_manipulation"    
##  [97] "gravity_control"              "regeneration"                
##  [99] "light_control"                "echolocation"                
## [101] "levitation"                   "toxin_and_disease_control"   
## [103] "banish"                       "energy_manipulation"         
## [105] "heat_resistance"              "natural_weapons"             
## [107] "time_travel"                  "enhanced_smell"              
## [109] "illusions"                    "thirstokinesis"              
## [111] "hair_manipulation"            "illumination"                
## [113] "omnipotent"                   "cloaking"                    
## [115] "changing_armor"               "power_cosmic"                
## [117] "biokinesis"                   "water_control"               
## [119] "radiation_immunity"           "vision_telescopic"           
## [121] "toxin_and_disease_resistance" "spatial_awareness"           
## [123] "energy_resistance"            "telepathy_resistance"        
## [125] "molecular_combustion"         "omnilingualism"              
## [127] "portal_creation"              "magnetism"                   
## [129] "mind_control_resistance"      "plant_control"               
## [131] "sonar"                        "sonic_scream"                
## [133] "time_manipulation"            "enhanced_touch"              
## [135] "magic_resistance"             "invisibility"                
## [137] "sub_mariner"                  "radiation_absorption"        
## [139] "intuitive_aptitude"           "vision_microscopic"          
## [141] "melting"                      "wind_control"                
## [143] "super_breath"                 "wallcrawling"                
## [145] "vision_night"                 "vision_infrared"             
## [147] "grim_reaping"                 "matter_absorption"           
## [149] "the_force"                    "resurrection"                
## [151] "terrakinesis"                 "vision_heat"                 
## [153] "vitakinesis"                  "radar_sense"                 
## [155] "qwardian_power_ring"          "weather_control"             
## [157] "vision_x_ray"                 "vision_thermal"              
## [159] "web_creation"                 "reality_warping"             
## [161] "odin_force"                   "symbiote_costume"            
## [163] "speed_force"                  "phoenix_force"               
## [165] "molecular_dissipation"        "vision_cryo"                 
## [167] "omnipresent"                  "omniscient"
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  


```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
neutral_heroes <- filter(superhero_info, superhero_info$alignment=="neutral")
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?



```r
superhero_info_interest <- superhero_info[,c(1, 4, 9)]
```

## Not Human
4. List all of the superheros that are not human.


```r
non_humans = filter(superhero_info_interest, superhero_info_interest$race != "Human")
non_humans
```

```
## # A tibble: 222 × 3
##    name         race              alignment
##    <chr>        <chr>             <chr>    
##  1 Abe Sapien   Icthyo Sapien     good     
##  2 Abin Sur     Ungaran           good     
##  3 Abomination  Human / Radiation bad      
##  4 Abraxas      Cosmic Entity     bad      
##  5 Ajax         Cyborg            bad      
##  6 Alien        Xenomorph XX121   bad      
##  7 Amazo        Android           bad      
##  8 Angel        Vampire           good     
##  9 Angel Dust   Mutant            good     
## 10 Anti-Monitor God / Eternal     bad      
## # … with 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".



```r
good_superhero <- filter(superhero_info, superhero_info$alignment == "good")
```


```r
bad_superhero <- filter(superhero_info, superhero_info$alignment == "bad")
```

6. For the good guys, use the `tabyl` function to summarize their "race".



```r
tabyl(good_superhero, race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
filter(good_superhero, good_superhero$race == "Asgardian")
```

```
## # A tibble: 3 × 10
##   name      gender Eye col…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##   <chr>     <chr>  <chr>     <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Sif       Female blue      Asga… Black      188 Marvel… <NA>    good       191
## 2 Thor      Male   blue      Asga… Blond      198 Marvel… <NA>    good       288
## 3 Thor Girl Female blue      Asga… Blond      175 Marvel… <NA>    good       143
## # … with abbreviated variable names ¹​`Eye color`, ²​hair_color, ³​publisher,
## #   ⁴​skin_color, ⁵​alignment
```

8. Among the bad guys, who are the male humans over 200 inches in height?



```r
filter(bad_superhero, bad_superhero$height>200)
```

```
## # A tibble: 25 × 10
##    name       gender Eye c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 Abominati… Male   green   Huma… No Hair    203 Marvel… <NA>    bad        441
##  2 Alien      Male   <NA>    Xeno… No Hair    244 Dark H… black   bad        169
##  3 Amazo      Male   red     Andr… <NA>       257 DC Com… <NA>    bad        173
##  4 Apocalypse Male   red     Muta… Black      213 Marvel… grey    bad        135
##  5 Bane       Male   <NA>    Human <NA>       203 DC Com… <NA>    bad        180
##  6 Bloodaxe   Female blue    Human Brown      218 Marvel… <NA>    bad        495
##  7 Darkseid   Male   red     New … No Hair    267 DC Com… grey    bad        817
##  8 Doctor Do… Male   brown   Human Brown      201 Marvel… <NA>    bad        187
##  9 Doctor Do… Male   brown   <NA>  Brown      201 Marvel… <NA>    bad        132
## 10 Doomsday   Male   red     Alien White      244 DC Com… <NA>    bad        412
## # … with 15 more rows, and abbreviated variable names ¹​`Eye color`,
## #   ²​hair_color, ³​publisher, ⁴​skin_color, ⁵​alignment
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

They have the same amount!

```r
length(filter(bad_superhero, bad_superhero$hair_color == "No Hair"))
```

```
## [1] 10
```

```r
length(filter(good_superhero, good_superhero$hair_color == "No Hair"))
```

```
## [1] 10
```

10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight greater than or equal to 450?

```r
filter(superhero_info, superhero_info$height > 200| superhero_info$weight>=450)
```

```
## # A tibble: 60 × 10
##    name       gender Eye c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##    <chr>      <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
##  1 A-Bomb     Male   yellow  Human No Hair    203 Marvel… <NA>    good       441
##  2 Abominati… Male   green   Huma… No Hair    203 Marvel… <NA>    bad        441
##  3 Alien      Male   <NA>    Xeno… No Hair    244 Dark H… black   bad        169
##  4 Amazo      Male   red     Andr… <NA>       257 DC Com… <NA>    bad        173
##  5 Ant-Man    Male   blue    Human Blond      211 Marvel… <NA>    good       122
##  6 Anti-Venom Male   blue    Symb… Blond      229 Marvel… <NA>    <NA>       358
##  7 Apocalypse Male   red     Muta… Black      213 Marvel… grey    bad        135
##  8 Bane       Male   <NA>    Human <NA>       203 DC Com… <NA>    bad        180
##  9 Beta Ray … Male   <NA>    <NA>  No Hair    201 Marvel… <NA>    good       216
## 10 Bloodaxe   Female blue    Human Brown      218 Marvel… <NA>    bad        495
## # … with 50 more rows, and abbreviated variable names ¹​`Eye color`,
## #   ²​hair_color, ³​publisher, ⁴​skin_color, ⁵​alignment
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
filter(superhero_info, superhero_info$height>300)
```

```
## # A tibble: 8 × 10
##   name        gender Eye c…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##   <chr>       <chr>  <chr>   <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Fin Fang F… Male   red     Kaka… No Hair   975  Marvel… green   good        18
## 2 Galactus    Male   black   Cosm… Black     876  Marvel… <NA>    neutral     16
## 3 Groot       Male   yellow  Flor… <NA>      701  Marvel… <NA>    good         4
## 4 MODOK       Male   white   Cybo… Brownn    366  Marvel… <NA>    bad        338
## 5 Onslaught   Male   red     Muta… No Hair   305  Marvel… <NA>    bad        405
## 6 Sasquatch   Male   red     <NA>  Orange    305  Marvel… <NA>    good       900
## 7 Wolfsbane   Female green   <NA>  Auburn    366  Marvel… <NA>    good       473
## 8 Ymir        Male   white   Fros… No Hair   305. Marvel… white   good        NA
## # … with abbreviated variable names ¹​`Eye color`, ²​hair_color, ³​publisher,
## #   ⁴​skin_color, ⁵​alignment
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

It's not 16 because the | operator indicates an OR. It combines the number of superheroes that satisfy those conditions, not just superheroes that satisfy both conditions, leading to a much higher total.

```r
filter(superhero_info, superhero_info$weight>450)
```

```
## # A tibble: 8 × 10
##   name       gender Eye co…¹ race  hair_…² height publi…³ skin_…⁴ align…⁵ weight
##   <chr>      <chr>  <chr>    <chr> <chr>    <dbl> <chr>   <chr>   <chr>    <dbl>
## 1 Bloodaxe   Female blue     Human Brown    218   Marvel… <NA>    bad        495
## 2 Darkseid   Male   red      New … No Hair  267   DC Com… grey    bad        817
## 3 Giganta    Female green    <NA>  Red       62.5 DC Com… <NA>    bad        630
## 4 Hulk       Male   green    Huma… Green    244   Marvel… green   good       630
## 5 Juggernaut Male   blue     Human Red      287   Marvel… <NA>    neutral    855
## 6 Red Hulk   Male   yellow   Huma… Black    213   Marvel… red     neutral    630
## 7 Sasquatch  Male   red      <NA>  Orange   305   Marvel… <NA>    good       900
## 8 Wolfsbane  Female green    <NA>  Auburn   366   Marvel… <NA>    good       473
## # … with abbreviated variable names ¹​`Eye color`, ²​hair_color, ³​publisher,
## #   ⁴​skin_color, ⁵​alignment
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

This might be a weird way to do it but... here goes

```r
ratio_hw = superhero_info$height/superhero_info$weight
max(ratio_hw, na.rm = TRUE)
```

```
## [1] 175.25
```

```r
#175.25 is the highest ratio, now I just need to find WHO has that ratio
ratio_hw[ratio_hw==175.25]
```

```
##   [1]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [11]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [21]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [31]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [41]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [51]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [61]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [71]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [81]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
##  [91]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [101]     NA     NA     NA     NA     NA     NA 175.25     NA     NA     NA
## [111]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [121]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [131]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [141]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [151]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [161]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [171]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [181]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [191]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [201]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [211]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [221]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [231]     NA     NA     NA     NA     NA     NA     NA     NA     NA     NA
## [241]     NA     NA     NA     NA     NA
```



```r
#It's in the 107th position, time to match that to superhero_info
superhero_info$name[107]
```

```
## [1] "Black Widow"
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Abin …
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, F…
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE, FA…
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, …
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, T…
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE, F…
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE, FA…
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, TRUE…
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, FAL…
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE, FA…
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE, F…
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, …
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE, T…
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALSE, …
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, …
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRUE, …
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALSE, …
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, …
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
length(filter(superhero_powers, superhero_powers$accelerated_healing == "TRUE", superhero_powers$durability == "TRUE", superhero_powers$super_strength == "TRUE"))
```

```
## [1] 168
```

## Your Favorite
15. Pick your favorite superhero and let's see their powers!

```r
filter(superhero_powers, superhero_powers$hero_names == "Flash"|superhero_powers$hero_names == "Spider-Man")
```

```
## # A tibble: 2 × 168
##   hero_…¹ agility accel…² lante…³ dimen…⁴ cold_…⁵ durab…⁶ stealth energ…⁷ flight
##   <chr>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl>   <lgl> 
## 1 Flash   FALSE   TRUE    FALSE   FALSE   FALSE   FALSE   FALSE   TRUE    TRUE  
## 2 Spider… TRUE    TRUE    FALSE   FALSE   FALSE   TRUE    TRUE    FALSE   FALSE 
## # … with 158 more variables: danger_sense <lgl>, underwater_breathing <lgl>,
## #   marksmanship <lgl>, weapons_master <lgl>, power_augmentation <lgl>,
## #   animal_attributes <lgl>, longevity <lgl>, intelligence <lgl>,
## #   super_strength <lgl>, cryokinesis <lgl>, telepathy <lgl>,
## #   energy_armor <lgl>, energy_blasts <lgl>, duplication <lgl>,
## #   size_changing <lgl>, density_control <lgl>, stamina <lgl>,
## #   astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>, …
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
