Data tidy
================
Shihui Peng
2023-09-29

# PULSE data

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
