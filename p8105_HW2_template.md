Homework 2
================
Wuraola Olawole

``` r
library(tidyverse)
library(readxl)
```

# Problem 1

Read dataset, Mr. Trashwheel

``` r
trashwheel_df =

    read_xlsx(
    
    "./Data/Trash-Wheel-Collection-Totals-8-6-19.xlsx",
    sheet = "Mr. Trash Wheel",
      range = cell_cols("A:N")) %>%
        janitor::clean_names() %>%
drop_na(dumpster) %>%
  mutate(
    sports_balls = round(sports_balls),
      sports_balls = as.integer(sports_balls)
  )
```
