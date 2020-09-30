Homework 2
================
Wuraola Olawole

``` r
    library(tidyverse)
      library(readxl)
```

# Problem 1

Set a path

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
path_2_data = "./Data/Trash-Wheel-Collection-Totals-8-6-19.xlsx"

  precipi_17_df =
  
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
precipi_18_df =
  
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
precipi_df = 
            bind_rows(precipi_17_df, precipi_18_df)

precipi_df =
            left_join(precipi_df, month_df, by = "month")
```

Mr. TrashWheel data set contained data about trash that was collected in
a dumpster (by Mr. TrashWheel) as it entered the inner harbor in
Baltimore Maryland. The data set contained variables year and date of
trash collection as well as the weights in tons and volume of the trash
collected. It also provided information about the categories of trash
collected in the dumpster. In this data set (final outcome), there were
344. There were also additional sheets that provided information about
precipitation; the month and total amount of precipitation in 2017 and
2018.

The total precipitation observed in 2018 was 70.33 inches and the median
number of sports balls in the dumpster in 2017 was 8.

# problem 2

Read the NYC\~ csv data, retain select variables,convert ‘entry’
variable from character to logical variable using the ‘recode’ function.

``` r
path_3_data = "./Data/NYC_Transit_Subway_Entrance_And_Exit_Data.csv"
  
  NYC_df = 
    
    read_csv(
             path_3_data) %>% 
              janitor::clean_names() %>% 
                select(2:18, 20, 23) %>%
  mutate(
    entry = recode(entry, "YES" = TRUE, "NO" = FALSE)) %>%
    mutate_at(vars(route8:route11), as.character)             # convert dbl variables to chr variables
```

Tidy data\! and convert route\~ from rows to column and stored their
values as route numbers

``` r
  NYC_df =
          NYC_df %>%
                pivot_longer(                                                    # Tidy data! 
                              route1:route11, 
                              names_to = "route_name", 
                              values_to = "route_num") %>%
    
                      relocate(route_name, route_num, .before = entrance_type)
```

The NYC\_Transit dataset provides information about New York city
Transportation system. The data includes information about train lines,
station name, station location, available routes, types of entrance,
entry and vending information. The data also includes ADA compliance of
the various stations. Some steps I have taken to clean this data
include, cleaning variable names, selecting the variables I need, and
converting select variables to a format that would be useful and easy to
manipulate. I also tidied the data, converting the route names from
numerous rows into a single column thereby improving the readability of
my dataset. Finally, I moved a variable around to keep with the flow of
data. The dimension of the resulting dataset is (20548, 10). The
resulting data is tidier than the initial data.

There are 465 distinct stations in this dataset and 84 of these stations
are ADA compliant. The proportion of station entrances that allow
entrance without vending is 43. There are 60 distinct stations that
serve the A train. Of these, only 17 of them are ADA compliant.

# Problem 3

Read dataset pols-month.csv, clean the datasets, and use function
‘separate’ to split “mon” into year, month and day. Also, convert from
character variable to numeric.

``` r
path_4_data = "./Data/fivethirtyeight_datasets/pols-month.csv"

  pols_month_df =
  
                  read_csv(
                           path_4_data) %>%
    janitor::clean_names() %>%
      separate(col = mon, into = c("Year", "Month", "Day"), sep = "-") %>%
        mutate(Year = as.numeric(Year),
               Month = as.numeric(Month),
               Day = as.numeric(Day)) 
```

Replace Month number with Month name by creating a tibble helper and
then perform a left\_join \*(Note to self: This Month\_df variable
differs from month\_df in Trashwheel. This is uppercased the other is
not)

``` r
Month_df =
    tibble(
            Month = 1:12,
            Month_name = month.name)

    pols1_month_df =
                    left_join(pols_month_df, Month_df, by = "Month") %>%
                      relocate(Month_name, .before = Day) 
```

Create a ‘president’ variable that takes values gop and dem but remove
Prez\_dem, prez\_gop and day variable. The outcome gives a df containing
info about the number of politician in a certain timeframe who are
either democrats or republicans

``` r
  pols2_month_df =
                  pols1_month_df %>%
                    select(-prez_gop, -prez_dem, -Day) %>%
  
                              pivot_longer(                                                    # Tidy data! 
                                           gov_gop:rep_dem, 
                                              names_to = "president", 
                                                values_to = "num_of_cand")
```

Read snp.csv dataset, clean and perform similar steps as above, also
keep positions of year and month consistent

``` r
path_5_data = "./Data/fivethirtyeight_datasets/snp.csv"

  snp_df =
  
                  read_csv(
                           path_5_data) %>%
    janitor::clean_names() %>%
      separate(col = date, into = c("Day", "Month", "Year"), sep = "/") %>%
        mutate(Year = as.numeric(Year),
               Month = as.numeric(Month),
               Day = as.numeric(Day)) %>%
    
    relocate(Year, .before = Month)
```

    ## Parsed with column specification:
    ## cols(
    ##   date = col_character(),
    ##   close = col_double()
    ## )

Replace Month number with Month name by creating a tibble helper and
then perform a left\_join. Also remove variable ‘Day’

``` r
    snp1_df =
                    left_join(snp_df, Month_df, by = "Month") %>%
                      relocate(Month_name, .after = Month) %>%
                        select(-Day)
```

Read unemployment dataset, tidy it and switch from “wide” to “long”
format using ‘pivot\_longer’

``` r
path_6_data = "./Data/fivethirtyeight_datasets/unemployment.csv"

  unemploy_df =
  
                  read_csv(
                           path_6_data) %>%
    janitor::clean_names() %>%
      rename(Year = year) %>%
    
        pivot_longer(                                                    # Tidy data! 
                      jan:dec, 
                        names_to = "Month_name", 
                          values_to = "percent")
```

Merge snp dataset into pols data set\!

``` r
merge1_df = merge(pols2_month_df, snp1_df, all = TRUE)

merge2_df = 
            merge(merge1_df, unemploy_df, all = TRUE) %>%
              select(-Month) 
```

This dataset is made up of 3 separate data sets. The first dataset, pols
contains data about the number of politician in a certain timeframe
(1947- 2015) who were either democrats or republicans. The resulting
dataset had variables year, month name, president and number of
candidates. The dimensions of this data set was 4932, 5. The second
dataset snp.csv gives information about the date and closing value of
s\&p stock index.The key variables in the final dataset were year, month
name and closing value. The year ranged from the most recent 2015 to
1950. The dimension of the data was 787, 4. The last dataset,
Unemployment.csv contains data about the percentage of unemployment in
certain years 1948 - 2015. The key variables in this dataset were years,
month names and percent. The dimension of the final dataset was 816, 3.
The final merged version of the datasets has dimension 9162, 6.
