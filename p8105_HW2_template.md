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
                select(2:18, 20, 23) %>%
  mutate(
    entry = recode(entry, "YES" = TRUE, "NO" = FALSE))
```

``` r
NYC_station = distinct(NYC_df, line, station_name)
distinct(NYC_df, station_name)
```

    ## # A tibble: 356 x 1
    ##    station_name            
    ##    <chr>                   
    ##  1 25th St                 
    ##  2 36th St                 
    ##  3 45th St                 
    ##  4 53rd St                 
    ##  5 59th St                 
    ##  6 77th St                 
    ##  7 86th St                 
    ##  8 95th St                 
    ##  9 9th St                  
    ## 10 Atlantic Av-Barclays Ctr
    ## # ... with 346 more rows

``` r
NYC_ada = 
          select(NYC_df, ada) %>%
  filter(ada == TRUE)
```

``` r
NYC_vend = 
          select(NYC_df, entry, vending)
```
