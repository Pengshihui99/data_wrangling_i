data_manipulation
================
Shihui Peng
2023-09-28

Import FAS litters and pups.

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

# `select`

``` r
select(litters_df, group, litter_number, gd0_weight)
```

    ## # A tibble: 49 × 3
    ##    group litter_number   gd0_weight
    ##    <chr> <chr>                <dbl>
    ##  1 Con7  #85                   19.7
    ##  2 Con7  #1/2/95/2             27  
    ##  3 Con7  #5/5/3/83/3-3         26  
    ##  4 Con7  #5/4/2/95/2           28.5
    ##  5 Con7  #4/2/95/3-3           NA  
    ##  6 Con7  #2/2/95/3-2           NA  
    ##  7 Con7  #1/5/3/83/3-3/2       NA  
    ##  8 Con8  #3/83/3-3             NA  
    ##  9 Con8  #2/95/3               NA  
    ## 10 Con8  #3/5/2/2/95           28.5
    ## # ℹ 39 more rows

``` r
select(litters_df, gd0_weight, litter_number, group)
```

    ## # A tibble: 49 × 3
    ##    gd0_weight litter_number   group
    ##         <dbl> <chr>           <chr>
    ##  1       19.7 #85             Con7 
    ##  2       27   #1/2/95/2       Con7 
    ##  3       26   #5/5/3/83/3-3   Con7 
    ##  4       28.5 #5/4/2/95/2     Con7 
    ##  5       NA   #4/2/95/3-3     Con7 
    ##  6       NA   #2/2/95/3-2     Con7 
    ##  7       NA   #1/5/3/83/3-3/2 Con7 
    ##  8       NA   #3/83/3-3       Con8 
    ##  9       NA   #2/95/3         Con8 
    ## 10       28.5 #3/5/2/2/95     Con8 
    ## # ℹ 39 more rows

``` r
select(litters_df, group, gd0_weight:gd_of_birth)
```

    ## # A tibble: 49 × 4
    ##    group gd0_weight gd18_weight gd_of_birth
    ##    <chr>      <dbl>       <dbl>       <dbl>
    ##  1 Con7        19.7        34.7          20
    ##  2 Con7        27          42            19
    ##  3 Con7        26          41.4          19
    ##  4 Con7        28.5        44.1          19
    ##  5 Con7        NA          NA            20
    ##  6 Con7        NA          NA            20
    ##  7 Con7        NA          NA            20
    ##  8 Con8        NA          NA            20
    ##  9 Con8        NA          NA            20
    ## 10 Con8        28.5        NA            20
    ## # ℹ 39 more rows

``` r
select(litters_df, group, starts_with("pups"))
```

    ## # A tibble: 49 × 4
    ##    group pups_born_alive pups_dead_birth pups_survive
    ##    <chr>           <dbl>           <dbl>        <dbl>
    ##  1 Con7                3               4            3
    ##  2 Con7                8               0            7
    ##  3 Con7                6               0            5
    ##  4 Con7                5               1            4
    ##  5 Con7                6               0            6
    ##  6 Con7                6               0            4
    ##  7 Con7                9               0            9
    ##  8 Con8                9               1            8
    ##  9 Con8                8               0            8
    ## 10 Con8                8               0            8
    ## # ℹ 39 more rows

- **`select`**: is used to select columns. If want to extract only some
  columns, we can use this. when using it:
  **`select(data_frame_Im_working_on_now, column1_want_to_keep, column2_want_to_keep, ...)`**.
  If put in diff order, r will put these columns in the order we have
  them in. So we can reorganize data in this way.

- select(litters_df, group, **gd0_weight : gd_of_birth**): keep group
  col & “A:B” here means we want everything between A and B

- select(litters_df, group, **starts_with(“pups”)**): keep group col &
  “starts_with(”ABC”)” here means select everything that starts with
  ABC. There are also **`ends_with`** and **`contains`**

## `select` and remove cols while keeping all other cols

``` r
select(litters_df, - litter_number)
```

    ## # A tibble: 49 × 7
    ##    group gd0_weight gd18_weight gd_of_birth pups_born_alive pups_dead_birth
    ##    <chr>      <dbl>       <dbl>       <dbl>           <dbl>           <dbl>
    ##  1 Con7        19.7        34.7          20               3               4
    ##  2 Con7        27          42            19               8               0
    ##  3 Con7        26          41.4          19               6               0
    ##  4 Con7        28.5        44.1          19               5               1
    ##  5 Con7        NA          NA            20               6               0
    ##  6 Con7        NA          NA            20               6               0
    ##  7 Con7        NA          NA            20               9               0
    ##  8 Con8        NA          NA            20               9               1
    ##  9 Con8        NA          NA            20               8               0
    ## 10 Con8        28.5        NA            20               8               0
    ## # ℹ 39 more rows
    ## # ℹ 1 more variable: pups_survive <dbl>

``` r
select(litters_df, - starts_with("gd"))
```

    ## # A tibble: 49 × 5
    ##    group litter_number   pups_born_alive pups_dead_birth pups_survive
    ##    <chr> <chr>                     <dbl>           <dbl>        <dbl>
    ##  1 Con7  #85                           3               4            3
    ##  2 Con7  #1/2/95/2                     8               0            7
    ##  3 Con7  #5/5/3/83/3-3                 6               0            5
    ##  4 Con7  #5/4/2/95/2                   5               1            4
    ##  5 Con7  #4/2/95/3-3                   6               0            6
    ##  6 Con7  #2/2/95/3-2                   6               0            4
    ##  7 Con7  #1/5/3/83/3-3/2               9               0            9
    ##  8 Con8  #3/83/3-3                     9               1            8
    ##  9 Con8  #2/95/3                       8               0            8
    ## 10 Con8  #3/5/2/2/95                   8               0            8
    ## # ℹ 39 more rows

- select(litters_df, **- litter_number**): remove “litter_number” and
  keep all of the other columns

- select(litters_df, **- starts_with(“gd”)**): remove everything that
  starts with “gd” and keep all of the other columns

## `select` and rename and `everything()`

``` r
select(litters_df, group, litter_id = litter_number)
```

    ## # A tibble: 49 × 2
    ##    group litter_id      
    ##    <chr> <chr>          
    ##  1 Con7  #85            
    ##  2 Con7  #1/2/95/2      
    ##  3 Con7  #5/5/3/83/3-3  
    ##  4 Con7  #5/4/2/95/2    
    ##  5 Con7  #4/2/95/3-3    
    ##  6 Con7  #2/2/95/3-2    
    ##  7 Con7  #1/5/3/83/3-3/2
    ##  8 Con8  #3/83/3-3      
    ##  9 Con8  #2/95/3        
    ## 10 Con8  #3/5/2/2/95    
    ## # ℹ 39 more rows

``` r
select(litters_df, group, litter_id = litter_number, everything())
```

    ## # A tibble: 49 × 8
    ##    group litter_id       gd0_weight gd18_weight gd_of_birth pups_born_alive
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
rename(litters_df, litter_id = litter_number)
```

    ## # A tibble: 49 × 8
    ##    group litter_id       gd0_weight gd18_weight gd_of_birth pups_born_alive
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
select(litters_df, gd0_weight, everything())
```

    ## # A tibble: 49 × 8
    ##    gd0_weight group litter_number   gd18_weight gd_of_birth pups_born_alive
    ##         <dbl> <chr> <chr>                 <dbl>       <dbl>           <dbl>
    ##  1       19.7 Con7  #85                    34.7          20               3
    ##  2       27   Con7  #1/2/95/2              42            19               8
    ##  3       26   Con7  #5/5/3/83/3-3          41.4          19               6
    ##  4       28.5 Con7  #5/4/2/95/2            44.1          19               5
    ##  5       NA   Con7  #4/2/95/3-3            NA            20               6
    ##  6       NA   Con7  #2/2/95/3-2            NA            20               6
    ##  7       NA   Con7  #1/5/3/83/3-3/2        NA            20               9
    ##  8       NA   Con8  #3/83/3-3              NA            20               9
    ##  9       NA   Con8  #2/95/3                NA            20               8
    ## 10       28.5 Con8  #3/5/2/2/95            NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
relocate(litters_df, litter_number)
```

    ## # A tibble: 49 × 8
    ##    litter_number   group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr>           <chr>      <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 #85             Con7        19.7        34.7          20               3
    ##  2 #1/2/95/2       Con7        27          42            19               8
    ##  3 #5/5/3/83/3-3   Con7        26          41.4          19               6
    ##  4 #5/4/2/95/2     Con7        28.5        44.1          19               5
    ##  5 #4/2/95/3-3     Con7        NA          NA            20               6
    ##  6 #2/2/95/3-2     Con7        NA          NA            20               6
    ##  7 #1/5/3/83/3-3/2 Con7        NA          NA            20               9
    ##  8 #3/83/3-3       Con8        NA          NA            20               9
    ##  9 #2/95/3         Con8        NA          NA            20               8
    ## 10 #3/5/2/2/95     Con8        28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

- select(litters_df, group, **litter_id = litter_number**): keep group
  column and litter_number column, but rename ‘litter_number’ col to
  ‘litter_id’

- select(litters_df, group, litter_id = litter_number,
  **everything()**): keep group col & keep litter_number col, but rename
  ‘litter_number’ col to ‘litter_id’ & keep everything that I have not
  mentioned yet.

  - **rename(litters_df, litter_id = litter_number)** is also for
    renaming the variable and keeping everything else.

- select(litters_df, gd0_weight, everything()): **reorder the cols**.
  Give gd0_weight first, and then the other cols. When i need to put
  cols in a diff order but still *keep everything else*, i can use this
  code.

  - **relocate(litters_df, litter_number)** is also for bringing
    ‘litter_number’ first and keeping everything else.

## `select` vs `pull`

``` r
select(litters_df, group)
```

    ## # A tibble: 49 × 1
    ##    group
    ##    <chr>
    ##  1 Con7 
    ##  2 Con7 
    ##  3 Con7 
    ##  4 Con7 
    ##  5 Con7 
    ##  6 Con7 
    ##  7 Con7 
    ##  8 Con8 
    ##  9 Con8 
    ## 10 Con8 
    ## # ℹ 39 more rows

``` r
pull(litters_df, group)
```

    ##  [1] "Con7" "Con7" "Con7" "Con7" "Con7" "Con7" "Con7" "Con8" "Con8" "Con8"
    ## [11] "Con8" "Con8" "Con8" "Con8" "Con8" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7"
    ## [21] "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Low7" "Low7" "Low7"
    ## [31] "Low7" "Low7" "Low7" "Low7" "Low7" "Mod8" "Mod8" "Mod8" "Mod8" "Mod8"
    ## [41] "Mod8" "Mod8" "Low8" "Low8" "Low8" "Low8" "Low8" "Low8" "Low8"

- **`select`**: get a *data frame* out and it doesn’t care that there’s
  only 1 col here.
- **`pull`**: would break the tidyverse logic by extracting a col from
  your data frame. and the output is just like a vector floating around
  – it *doesn’t exist inside of a data frame* anymore
