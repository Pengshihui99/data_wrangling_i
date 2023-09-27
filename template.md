data importing
================
Shihui Peng
2023-09-27

The function we are going to use is in the readr package, which is
loaded by default by the Tidyverse.

# Import the `FAS_litters.csv` csv using a relative path.

**read_csv():** first, tell ‘read_csv’ where to find this file, in ().
If we put all datasets in this corresponding R project folder, we can
use this relative path (“data/FAS_litters.csv”)

``` r
litters_df =
  read_csv("data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = 
  janitor::clean_names(litters_df)
```

**litters_df = janitor::clean_names(litters_df):** if the name has
space, we can switch the name into snake case with this code. Eg. Litter
Number -\> litter_number; Gd0 Weight -\> gd0_weight; etc.

# Import the same dataset using an absolute path.

``` r
litter_df_abs = 
  read_csv("~/Desktop/data_wrangling_i/data/FAS_litters.csv")
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

This might work, but if we move this r project folder to another place,
the relative path does not change (the direction is still from this
project to this data file), but the absolute path would change. So
litter_df is working. but litter_df_abs is not working in this way.

Therefore, **use relative path in a project** and **have all you need in
the same place** to make things easy and your code reproducible.

## Learning assessment for importing dataset FAS_pups.csv

``` r
pups_df = 
  read_csv("data/FAS_pups.csv")
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df =
  janitor::clean_names(pups_df)
```

# Look at data

``` r
litters_df
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
head(litters_df)
```

    ## # A tibble: 6 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
tail(litters_df)
```

    ## # A tibble: 6 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #79                 25.4        43.8          19               8
    ## 2 Low8  #100                20          39.2          20               8
    ## 3 Low8  #4/84               21.8        35.2          20               4
    ## 4 Low8  #108                25.6        47.5          20               8
    ## 5 Low8  #99                 23.5        39            20               6
    ## 6 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

- **litters_df**: show 1st 10 rows by default, might not show not all of
  the columns
- **head(litters_df)**: show 1st six rows
- **tail(litters_df)**: show bottom six rows. more useful b/c when
  something goes wrong, something like weird missing or weird structure
  would often happen in the bottom rows

``` r
view(litters_df)
```

If you want to look at the entire data set, use **view()**. A separate
window will pull up and we can scroll through it.

# Look at a data summary

``` r
str(litters_df)
```

    ## spc_tbl_ [49 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ group          : chr [1:49] "Con7" "Con7" "Con7" "Con7" ...
    ##  $ litter_number  : chr [1:49] "#85" "#1/2/95/2" "#5/5/3/83/3-3" "#5/4/2/95/2" ...
    ##  $ gd0_weight     : num [1:49] 19.7 27 26 28.5 NA NA NA NA NA 28.5 ...
    ##  $ gd18_weight    : num [1:49] 34.7 42 41.4 44.1 NA NA NA NA NA NA ...
    ##  $ gd_of_birth    : num [1:49] 20 19 19 19 20 20 20 20 20 20 ...
    ##  $ pups_born_alive: num [1:49] 3 8 6 5 6 6 9 9 8 8 ...
    ##  $ pups_dead_birth: num [1:49] 4 0 0 1 0 0 0 1 0 0 ...
    ##  $ pups_survive   : num [1:49] 3 7 5 4 6 4 9 8 8 8 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Group = col_character(),
    ##   ..   `Litter Number` = col_character(),
    ##   ..   `GD0 weight` = col_double(),
    ##   ..   `GD18 weight` = col_double(),
    ##   ..   `GD of Birth` = col_double(),
    ##   ..   `Pups born alive` = col_double(),
    ##   ..   `Pups dead @ birth` = col_double(),
    ##   ..   `Pups survive` = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

``` r
skimr::skim(litters_df)
```

|                                                  |            |
|:-------------------------------------------------|:-----------|
| Name                                             | litters_df |
| Number of rows                                   | 49         |
| Number of columns                                | 8          |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |            |
| Column type frequency:                           |            |
| character                                        | 2          |
| numeric                                          | 6          |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |            |
| Group variables                                  | None       |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| group         |         0 |             1 |   4 |   4 |     0 |        6 |          0 |
| litter_number |         0 |             1 |   3 |  15 |     0 |       49 |          0 |

**Variable type: numeric**

| skim_variable   | n_missing | complete_rate |  mean |   sd |   p0 |   p25 |   p50 |   p75 | p100 | hist  |
|:----------------|----------:|--------------:|------:|-----:|-----:|------:|------:|------:|-----:|:------|
| gd0_weight      |        15 |          0.69 | 24.38 | 3.28 | 17.0 | 22.30 | 24.10 | 26.67 | 33.4 | ▃▇▇▆▁ |
| gd18_weight     |        17 |          0.65 | 41.52 | 4.05 | 33.4 | 38.88 | 42.25 | 43.80 | 52.7 | ▃▃▇▂▁ |
| gd_of_birth     |         0 |          1.00 | 19.65 | 0.48 | 19.0 | 19.00 | 20.00 | 20.00 | 20.0 | ▅▁▁▁▇ |
| pups_born_alive |         0 |          1.00 |  7.35 | 1.76 |  3.0 |  6.00 |  8.00 |  8.00 | 11.0 | ▁▃▂▇▁ |
| pups_dead_birth |         0 |          1.00 |  0.33 | 0.75 |  0.0 |  0.00 |  0.00 |  0.00 |  4.0 | ▇▂▁▁▁ |
| pups_survive    |         0 |          1.00 |  6.41 | 2.05 |  1.0 |  5.00 |  7.00 |  8.00 |  9.0 | ▁▃▂▇▇ |

- **str(litters_df)**: looks at the structure of the data frame
- **skimr::skim(litters_df)**: useful for 1st time to look at a dataset.
  can give info about variable type, min, max, empty, n, mean, sd,
  p0/25/50/75/100, histograms

# Options in `read_*`

## `skip` and `col_names`

``` r
litters_df =
  read_csv(
    "data/FAS_litters.csv",
    skip = 10,
    col_names = FALSE
  )
```

    ## Rows: 40 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): X1, X2
    ## dbl (6): X3, X4, X5, X6, X7, X8
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

- **skip=**: read_csv assumes the 1st row in us data set is variable
  names and it starts reading things in from the very top row. If do
  *“skip=10”*, 1st 10 rows would be skipped. Now the 11th row is assumed
  as a collection of variable names by default. This would make
  troubles.
- **col_names = FALSE**: r would make up its own column names, such as
  x1, x2, …. But it still read in the last rows that we have here.
  - When you have a dataset which 1st 10 rows are variable explanations
    or data dictionary, we can skip these 10 rows and start importing us
    data. The 11th row would not be the variable name row but the 1st
    row of record we need.

## Look at NA values: `na = c()`

``` r
litters_df =
  read_csv(
    "data/FAS_litters.csv",
    na = c("NA", 99, " ", ".")
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

- **na = c(…)**: whenever you see NA, number value 99, space, or .,
  treat them as missing/NA

## `col_types`

``` r
litters_df =
  read_csv(
    "data/FAS_litters.csv",
    col_types = 
      cols(
        'GD0 weight' = col_character(),Group = col_factor()
      )
  )

litters_df
```

    ## # A tibble: 49 × 8
    ##    Group `Litter Number` `GD0 weight` `GD18 weight` `GD of Birth`
    ##    <fct> <chr>           <chr>                <dbl>         <dbl>
    ##  1 Con7  #85             19.7                  34.7            20
    ##  2 Con7  #1/2/95/2       27                    42              19
    ##  3 Con7  #5/5/3/83/3-3   26                    41.4            19
    ##  4 Con7  #5/4/2/95/2     28.5                  44.1            19
    ##  5 Con7  #4/2/95/3-3     <NA>                  NA              20
    ##  6 Con7  #2/2/95/3-2     <NA>                  NA              20
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>                  NA              20
    ##  8 Con8  #3/83/3-3       <NA>                  NA              20
    ##  9 Con8  #2/95/3         <NA>                  NA              20
    ## 10 Con8  #3/5/2/2/95     28.5                  NA              20
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: `Pups born alive` <dbl>, `Pups dead @ birth` <dbl>,
    ## #   `Pups survive` <dbl>

- r would by default guess what kinds of data for each column, but it
  won’t look through all rows. So if needed, we can change or define the
  column types for specific columns.
- **col_types = cols()** can be used:
  - **‘GD0 weight’ = col_character()**: since there is a space in (GD0
    Weight), we need to use “” to include this variable name. This code
    is changing the column type of GD0 weight to character.
  - **Group = col_factor()**: changing the column type of Group to
    factor.
