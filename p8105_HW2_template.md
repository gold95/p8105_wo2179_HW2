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

Read dataset, Mr. Trashwheel, set parameters, and modify sports\_balls

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
                year = 2017) %>%
  relocate(year)
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
              year = 2018) %>%
  relocate(year)
```

Combine precipitation dataset and Change month to Character variable

``` r
month_df =
    tibble(
            month = 1:12,
            month_name = month.name
    )
```

``` r
Precipi_df = 
            bind_rows(Precipi_17_df, Precipi_18_df)

Precipi_df =
            left_join(Precipi_df, month_df, by = "month")
```

# problem 2

Read the NYC\~ csv data, and only include select variables

``` r
path_3_data = "./Data/NYC_Transit_Subway_Entrance_And_Exit_Data.csv"
  
NYC_df = 
    
  read_csv(
            path_3_data) %>% 
   janitor::clean_names() %>% 
    select(2:18, 20, 23)
```

Convert ‘entry’ variable from character to logical: created a variable
enter\_df and modified it

``` r
 enter_df = select(NYC_df, entry)
  entry_df = enter_df %>%
    mutate_all(funs(as.logical(.)))
```
