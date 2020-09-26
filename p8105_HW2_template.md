Homework 2
================
Wuraola Olawole

``` r
library(tidyverse)
library(readxl)
```

# Problem 1

set a path

``` r
path_2_data = "./Data/Trash-Wheel-Collection-Totals-8-6-19.xlsx"
```

Read dataset, Mr. Trashwheel, set parameters, and modify sports\_ball

``` r
trashwheel_df =

    read_excel(
                path = path_2_data,
                sheet = "Mr. Trash Wheel",
                range = cell_cols("A:N"),
                col_types = NULL,
                skip = 0) %>%
        janitor::clean_names() %>%
  drop_na(dumpster) %>%
    mutate(
    sports_balls = round(sports_balls),
      sports_balls = as.integer(sports_balls)
  )
```

Read data on Precipitation in 2017 and in 2018 and add variable “year”

``` r
Precipi_17_df =
  
  read_excel(
                path = path_2_data,
                sheet = "2017 Precipitation",
                col_types = NULL,
                skip = 1) %>%
        janitor::clean_names() %>%
  drop_na(month) %>%
    mutate(
      year = 2017)
```

``` r
Precipi_18_df =
  
  read_excel(
                path = path_2_data,
                sheet = "2018 Precipitation",
                col_types = NULL,
                skip = 1) %>%
        janitor::clean_names() %>%
  drop_na(month) %>%
    mutate(
      year = 2018)
```
