Data tidy
================
Shihui Peng
2023-09-29

# PULSE data

## pivot_longer

The observation time is a variable that has been encoded into column
names. I need to switch this from wide format to long format. In **wide
format** data, each column may represent a specific time point,
category, or variable, and each row typically represents an individual
or case. **Long format** means that you have a single column to
represent the time or observation variable and another column to
represent the corresponding values associated with that time or
observation.

``` r
pulse_df =
  haven::read_sas("data/public_pulse_data.sas7bdat") |> 
  janitor::clean_names() |> 
  pivot_longer(
    bdi_score_bl : bdi_score_12m,
    names_to = "visit",
    values_to = "bdi_score",
    names_prefix = "bdi_score_",
  ) |> 
  mutate(
    visit = replace(visit, visit == "bl", "00m"),
    visit = factor(visit)
  )
```

- **`pivot_longer`**: critical for going from wide format to long
  format. in tidyr package.(tidyverse package has included it)
  - **`bdi_score_bl : bdi_score_12m`**: tell r these are the columns
    right now in wide format and need to be in long format.
  - **`names_to = "visit"`**: r is going to take each of columns
    specified above and put them in a new column. then we use this code
    to tell r what is the variable name for this new column going to be.
  - **`values_to = "bdi_score`**: r is going to take the actual numbers
    which belongs to each of above specified cols and put those to its
    own column (this col will store the corresponding values from the
    cols in the specified range). then we use this code to tell r what
    is the variable name of this new col going to be.
  - **`names_prefix = "bdi_score_"`**: this code can get rid of the
    prefix we don’t need. All values for visit col starts w a prefix
    “bdi_score\_”, but we want to get the exact means for this cell.
  - **`mutate(old_variable = replace(old_variable, condition, replacement_variable))`**:
    we want to change “bl” as baseline to 00m. use mutate to change or
    add variable (here, “visit”).
    - `replace(visit, check if visit value is "bl", if yes then replace by "00m")`
    - `factor(visit)`: converting visit to a factor variable. It’s
      possible to want visit to be a numeric variable instead.

### learning assessment

In the litters data, the variables gd0_weight and gd18_weight give the
weight of the mother mouse on gestational days 0 and 18. Write a data
cleaning chain that retains only litter_number and these columns;
produces new variables gd and weight; and makes gd a numeric variable
taking values 0 and 18 (for the last part, you might want to use recode
…). Is this version “tidy”?

``` r
litters_df =
  read_csv("data/FAS_litters.csv") |> 
  janitor::clean_names() |> 
  select(litter_number, ends_with("weight")) |> 
  pivot_longer(
    gd0_weight:gd18_weight, 
    names_to = "gd",
    values_to = "weight",
    names_prefix = "gd"
  ) |> 
  mutate(
    gd = recode(gd, "0_weight" = 0, "18_weight" = 18)
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

we can also use **case_match** when we want to replace multiple things
in a data set.
**`gd = case_match(gd, "0_weight" ~ 0, "18_weight" ~ 18)`**. This is
like, when r see this, replace w this; when r see that, replace w that;
etc.

In one sense, this is “tidy” because I have a variable for day and a
variable for weight rather that using values in my variable names.
However, it’s less useful if I’m interested in computing or analyzing
weight gain during pregnancy.

## pivot_wider

``` r
analysis_result = 
  tibble(
    group = c("treatment", "treatment", "placebo", "placebo"),
    time = c("pre", "post", "pre", "post"),
    mean = c(4, 8, 3.5, 4)
  )
analysis_result
```

    ## # A tibble: 4 × 3
    ##   group     time   mean
    ##   <chr>     <chr> <dbl>
    ## 1 treatment pre     4  
    ## 2 treatment post    8  
    ## 3 placebo   pre     3.5
    ## 4 placebo   post    4

This is the correct format for additional analysis or visualization, but
doesn’t facilitate quick comparisons for human readers.

An alternative presentation of the same data might have groups in rows,
times in columns, and mean values in table cells. This is decidedly
non-tidy; to get there from here we’ll need to use **`pivot_wider`**,
which is the inverse of pivot_longer:

``` r
pivot_wider(
  analysis_result, 
  names_from = "time", 
  values_from = "mean")
```

    ## # A tibble: 2 × 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

# knitr::kable()

We’re pretty much there now – in some cases you might use select to
reorder columns, and (depending on your goal) use **`knitr::kable()`**
to produce a nicer table for reading.

``` r
analysis_result = knitr::kable(analysis_result)
```

# bind rows

import LoTR_Words data. `read_excel` is from readxl package VS
`read_xlsx` is from readxlsx package.

``` r
fellowship_df = 
  readxl::read_excel("data/LoTR_Words.xlsx", range = "B3:D6") |> 
  mutate(movie = "fellowship")

two_towers_df = 
  readxl::read_excel("data/LoTR_Words.xlsx", range = "F3:H6") |> 
  mutate(movie = "two towers")

return_of_the_king_df = 
  readxl::read_excel("data/LoTR_Words.xlsx", range = "J3:L6") |> 
  mutate(movie = "return of the king")

lotr_df =
  bind_rows(fellowship_df, two_towers_df, return_of_the_king_df) |> 
  janitor::clean_names() |> 
  pivot_longer(
    female:male,
    names_to = "gender",
    values_to = "word"
  ) |> 
  relocate(movie)
```

- one thing that is helpful for excel package: we can specify which cols
  we want to read and can specify this w **range** function:
  `range = "B3:D6"` – want to read wi this region.
- **`bind_rows`**: bind rows together.
- **`relocate(movie)`**: relocate movie to the 1st col. see more use in
  ?relocate.
  - We can also use **`select(movie, everything())`** the resulting data
    frame will have the “movie” column as its first column, followed by
    all other columns from the original data frame. This is a convenient
    way to reorder columns in a data frame while ensuring you don’t lose
    any columns in the process.

## join data sets – revisit FAS

``` r
litters_df =
  read_csv("data/FAS_litters.csv") |> 
  janitor::clean_names() |> 
  mutate(wt_gain = gd18_weight - gd0_weight) |> 
  select(litter_number, group, wt_gain) |> 
  separate(group, into = c("dose", "day_of_tx"), 3)
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
pups_df = 
  read_csv("data/FAS_pups.csv") |> 
  janitor::clean_names() |> 
  mutate(
    sex = case_match(
      sex, 1 ~ "male", 2~ "female"
      )
    )
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
fas_df = 
  left_join(pups_df, litters_df, by = "litter_number")
```

- **`separate`**: we can find that group col contain 2 info, dose level
  (Con) and when ppl get dose (7/8). use separate to tell r: what
  variable we want to separate (`group`), what do we want to separate it
  into (`into = c("dose", "day_of_tx")`), and where do we want to do
  this separation (`3`).
- There are four major ways join dataframes x and y:
  - **Inner**: keeps data that appear in both x and y
  - **Left**: keeps data that appear in x
  - **Right**: keeps data that appear in y
  - **Full**: keeps data that appear in either x or y
    - `left_join()` are the most common, because they add data from a
      smaller table y into a larger table x without removing anything
      from x.
- Note that joining is not particularly amenable to the \|\> operator
  because it is fundamentally non-linear: two separate datasets are
  coming together, rather than a single dataset being processed in a
  step-by-step fashion.
