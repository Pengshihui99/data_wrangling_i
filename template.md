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

head(litters_df)

tail(litters_df)
```

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

- str(litters_df): looks at the structure of the data frame
