---
title: "Midterm 1"
author: "Jiaying Liu"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library(skimr)
library(janitor)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  
RStudio is an integrated development environment for the R programming language, while GitHub is an online collection of repositories. When we are programming in R, we manage files and repositories in RStudio and we can then share the part of code that is done in local with others. This allows individual code being accessed by anyone in need to either check the progress or to learn the coding method, thus facilitating the work flows. Using RMarkdown files can allow people to access the code directly, making it reproducible for others; RMarkdown can also be exported to various format to fit user's intention.

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**
We discussed data frame, vector and matrix. We are using data frame to package and manipulate data neatly and efficiently; it also provides information about the classes of the data. 

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
str(elephants)
```

```
## tibble [288 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ Age   : num [1:288] 1.4 17.5 12.8 11.2 12.7 ...
##  $ Height: num [1:288] 120 227 235 210 220 ...
##  $ Sex   : chr [1:288] "M" "M" "M" "M" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Age = col_double(),
##   ..   Height = col_double(),
##   ..   Sex = col_character()
##   .. )
```

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- janitor::clean_names(elephants)
view(elephants)
```

```r
elephants$sex <- as.factor(elephants$sex)
```

**5. (2 points) How many male and female elephants are represented in the data?**

```r
elephants %>%
  count(sex == "M")
```

```
## # A tibble: 2 x 2
##   `sex == "M"`     n
##   <lgl>        <int>
## 1 FALSE          150
## 2 TRUE           138
```
From the results above, we know that the number in "True" is the male amount while the number in "False" is the female amount. So there are 138 male and 150 female elephants represented in the data.

**6. (2 points) What is the average age all elephants in the data?**

```r
mean(elephants$age, na.rm = T)
```

```
## [1] 10.97132
```
The average age of all elephants is 10.97132 years.

**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>%
  group_by(sex) %>%
  summarise(age_avg = mean(age), height_avg = mean(height))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 3
##   sex   age_avg height_avg
##   <fct>   <dbl>      <dbl>
## 1 F       12.8        190.
## 2 M        8.95       185.
```

**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

```r
elephants %>%
  group_by(sex) %>%
  filter(age > 25) %>%
  summarise(avg_height = mean(height), min_height = min(height), max_height = max(height), n = n())
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 5
##   sex   avg_height min_height max_height     n
##   <fct>      <dbl>      <dbl>      <dbl> <int>
## 1 F           233.       206.       278.    25
## 2 M           273.       237.       304.     8
```
Male elephants have larger weight in general compared with female elephants that are over 25 years old. This is true for avarage height, min height and max height.

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**

```r
defaunation <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

```r
view(defaunation)
```

```r
str(defaunation)
```

```
## tibble [24 × 26] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ TransectID             : num [1:24] 1 2 2 3 4 5 6 7 8 9 ...
##  $ Distance               : num [1:24] 7.14 17.31 18.32 20.85 15.95 ...
##  $ HuntCat                : chr [1:24] "Moderate" "None" "None" "None" ...
##  $ NumHouseholds          : num [1:24] 54 54 29 29 29 29 29 54 25 73 ...
##  $ LandUse                : chr [1:24] "Park" "Park" "Park" "Logging" ...
##  $ Veg_Rich               : num [1:24] 16.7 15.8 16.9 12.4 17.1 ...
##  $ Veg_Stems              : num [1:24] 31.2 37.4 32.3 29.4 36 ...
##  $ Veg_liana              : num [1:24] 5.78 13.25 4.75 9.78 13.25 ...
##  $ Veg_DBH                : num [1:24] 49.6 34.6 42.8 36.6 41.5 ...
##  $ Veg_Canopy             : num [1:24] 3.78 3.75 3.43 3.75 3.88 2.5 4 4 3 3.25 ...
##  $ Veg_Understory         : num [1:24] 2.89 3.88 3 2.75 3.25 3 2.38 2.71 3.25 3.13 ...
##  $ RA_Apes                : num [1:24] 1.87 0 4.49 12.93 0 ...
##  $ RA_Birds               : num [1:24] 52.7 52.2 37.4 59.3 52.6 ...
##  $ RA_Elephant            : num [1:24] 0 0.86 1.33 0.56 1 0 1.11 0.43 2.2 0 ...
##  $ RA_Monkeys             : num [1:24] 38.6 28.5 41.8 19.9 41.3 ...
##  $ RA_Rodent              : num [1:24] 4.22 6.04 1.06 3.66 2.52 1.83 3.1 1.26 4.37 6.31 ...
##  $ RA_Ungulate            : num [1:24] 2.66 12.41 13.86 3.71 2.53 ...
##  $ Rich_AllSpecies        : num [1:24] 22 20 22 19 20 22 23 19 19 19 ...
##  $ Evenness_AllSpecies    : num [1:24] 0.793 0.773 0.74 0.681 0.811 0.786 0.818 0.757 0.773 0.668 ...
##  $ Diversity_AllSpecies   : num [1:24] 2.45 2.31 2.29 2.01 2.43 ...
##  $ Rich_BirdSpecies       : num [1:24] 11 10 11 8 8 10 11 11 11 9 ...
##  $ Evenness_BirdSpecies   : num [1:24] 0.732 0.704 0.688 0.559 0.799 0.771 0.801 0.687 0.784 0.573 ...
##  $ Diversity_BirdSpecies  : num [1:24] 1.76 1.62 1.65 1.16 1.66 ...
##  $ Rich_MammalSpecies     : num [1:24] 11 10 11 11 12 12 12 8 8 10 ...
##  $ Evenness_MammalSpecies : num [1:24] 0.736 0.705 0.65 0.619 0.736 0.694 0.776 0.79 0.821 0.783 ...
##  $ Diversity_MammalSpecies: num [1:24] 1.76 1.62 1.56 1.48 1.83 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   TransectID = col_double(),
##   ..   Distance = col_double(),
##   ..   HuntCat = col_character(),
##   ..   NumHouseholds = col_double(),
##   ..   LandUse = col_character(),
##   ..   Veg_Rich = col_double(),
##   ..   Veg_Stems = col_double(),
##   ..   Veg_liana = col_double(),
##   ..   Veg_DBH = col_double(),
##   ..   Veg_Canopy = col_double(),
##   ..   Veg_Understory = col_double(),
##   ..   RA_Apes = col_double(),
##   ..   RA_Birds = col_double(),
##   ..   RA_Elephant = col_double(),
##   ..   RA_Monkeys = col_double(),
##   ..   RA_Rodent = col_double(),
##   ..   RA_Ungulate = col_double(),
##   ..   Rich_AllSpecies = col_double(),
##   ..   Evenness_AllSpecies = col_double(),
##   ..   Diversity_AllSpecies = col_double(),
##   ..   Rich_BirdSpecies = col_double(),
##   ..   Evenness_BirdSpecies = col_double(),
##   ..   Diversity_BirdSpecies = col_double(),
##   ..   Rich_MammalSpecies = col_double(),
##   ..   Evenness_MammalSpecies = col_double(),
##   ..   Diversity_MammalSpecies = col_double()
##   .. )
```

```r
defaunation$HuntCat <- as.factor(defaunation$HuntCat)
defaunation$LandUse <- as.factor(defaunation$LandUse)
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

```r
defaunation %>%
  group_by(HuntCat) %>%
  filter(HuntCat == "High" | HuntCat == "Moderate") %>%
  summarise(birds_div_avg = mean(Diversity_BirdSpecies), mammals_div_avg = mean(Diversity_MammalSpecies))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 2 x 3
##   HuntCat  birds_div_avg mammals_div_avg
##   <fct>            <dbl>           <dbl>
## 1 High              1.66            1.74
## 2 Moderate          1.62            1.68
```

**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
defaunation %>%
  filter(Distance > 20 | Distance < 5) %>%
  group_by(Distance > 20, Distance < 5) %>%
  summarise(across(contains("RA_"), mean, na.rm = T))
```

```
## `summarise()` regrouping output by 'Distance > 20' (override with `.groups` argument)
```

```
## # A tibble: 2 x 8
## # Groups:   Distance > 20 [2]
##   `Distance > 20` `Distance < 5` RA_Apes RA_Birds RA_Elephant RA_Monkeys
##   <lgl>           <lgl>            <dbl>    <dbl>       <dbl>      <dbl>
## 1 FALSE           TRUE              0.08     70.4      0.0967       24.1
## 2 TRUE            FALSE             7.21     44.5      0.557        40.1
## # … with 2 more variables: RA_Rodent <dbl>, RA_Ungulate <dbl>
```
The relative abundance of apes, elephants, monkeys and ungulates drops off the closer you get to a villge; while the relative abundance of birds and elephants rises the closer you get to a village.

**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**
The relative abundance of animals compared by land use.

```r
defaunation %>%
  group_by(LandUse) %>%
  summarise(across(contains("RA_"), mean, na.rm = T))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```
## # A tibble: 3 x 7
##   LandUse RA_Apes RA_Birds RA_Elephant RA_Monkeys RA_Rodent RA_Ungulate
##   <fct>     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1 Logging    2.10     62.7       0.425       28.4      3.22        3.10
## 2 Neither    1.05     71.0       0.815       22.1      4.18        0.81
## 3 Park       2.50     44.0       0.614       42.0      2.87        8.06
```

