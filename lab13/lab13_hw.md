---
title: "Lab 13 Homework"
author: "Jiaying Liu"
date: "2021-03-02"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse')
```

```
## Loading required package: tidyverse
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.0.4     ✓ dplyr   1.0.2
## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.0
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
library(janitor)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv") %>%
  clean_names()
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  


```r
str(UC_admit)
```

```
## tibble [2,160 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ campus           : chr [1:2160] "Davis" "Davis" "Davis" "Davis" ...
##  $ academic_yr      : num [1:2160] 2019 2019 2019 2019 2019 ...
##  $ category         : chr [1:2160] "Applicants" "Applicants" "Applicants" "Applicants" ...
##  $ ethnicity        : chr [1:2160] "International" "Unknown" "White" "Asian" ...
##  $ perc_fr          : chr [1:2160] "21.16%" "2.51%" "18.39%" "30.76%" ...
##  $ filtered_count_fr: num [1:2160] 16522 1959 14360 24024 17526 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   Campus = col_character(),
##   ..   Academic_Yr = col_double(),
##   ..   Category = col_character(),
##   ..   Ethnicity = col_character(),
##   ..   `Perc FR` = col_character(),
##   ..   FilteredCountFR = col_double()
##   .. )
```
The overall structure and variable classes are shown above, and we can see that the dimensions are 2160 * 6.


```r
names(UC_admit)
```

```
## [1] "campus"            "academic_yr"       "category"         
## [4] "ethnicity"         "perc_fr"           "filtered_count_fr"
```


```r
anyNA(UC_admit)
```

```
## [1] TRUE
```


```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable          n_miss pct_miss
##   <chr>              <int>    <dbl>
## 1 perc_fr                1   0.0463
## 2 filtered_count_fr      1   0.0463
## 3 campus                 0   0     
## 4 academic_yr            0   0     
## 5 category               0   0     
## 6 ethnicity              0   0
```
The NA's in the dataset are represented as "NA" and they are located in variables "perc_fr" and "filtered_count_fr".

**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Ethnic Diversity"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Select Admission Detail", choices = c("campus", "academic_yr", "category"), selected = "campus"),
  hr(),
      helpText("UC Information Center Reference: (https://www.universityofcalifornia.edu/infocenter)"),
  ), 
  box(title= "UC Campus Ethnicity Distribution", width = 5,
  plotOutput("plot", width = "500px", height = "500px")
  )
  )
  ) 
) 

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
    UC_admit %>% 
    ggplot(aes_string(x = input$x, y = "filtered_count_fr", fill="ethnicity"))+
  geom_col(position = "dodge", alpha=0.9, size=4)+
  theme_grey(base_size = 18)+ labs(x=NULL, y="Number of Individuals", fill="Fill Variable")+
  theme(axis.text.x = element_text(angle = 65, hjust = 1))
  })
  
  session$onSessionEnded(stopApp)
  }
shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}

**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Enrollment Diversity"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(title = "Plot Options", width = 3,
  selectInput("x", "Student/Applicant Details", choices = c("ethnicity", "category", "campus"), 
              selected = "campus"),
      hr(),
      helpText("UC Information Center Reference: (https://www.universityofcalifornia.edu/infocenter)"),
  ), 
  box(title = "Enroment Data of UC Campus", width = 6,
  plotOutput("plot", width = "500px", height = "500px")
  ) 
  ) 
  ) 
)

server <- function(input, output, session) { 
  
  output$plot <- renderPlot({
  UC_admit %>% 
  ggplot(aes_string(x = "academic_yr", y="filtered_count_fr",fill = input$x)) +
geom_col(position = "dodge", alpha=0.9, size=4)+
  theme_grey(base_size = 18)+ labs(x="Academic Year", y="Number of Individuals", fill="Fill Variable")+
  theme(axis.text.x = element_text(angle = 65, hjust = 1))
  })
  
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
