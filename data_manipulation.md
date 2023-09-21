Data Manipulation
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

Import FAS litters and pups.

``` r
litters_df <- read_csv("data/FAS_litters.csv")
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
litters_df <- janitor::clean_names(litters_df)

pups_df <- read_csv("data/FAS_pups.csv")
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
pups_df <- janitor::clean_names(pups_df)
```

## `select`

`select` is used to select columns

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
select(litters_df, group, litter_number, gd0_weight:gd_of_birth) # ":" allows you to select things in between
```

    ## # A tibble: 49 × 5
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>
    ##  1 Con7  #85                   19.7        34.7          20
    ##  2 Con7  #1/2/95/2             27          42            19
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19
    ##  5 Con7  #4/2/95/3-3           NA          NA            20
    ##  6 Con7  #2/2/95/3-2           NA          NA            20
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20
    ##  8 Con8  #3/83/3-3             NA          NA            20
    ##  9 Con8  #2/95/3               NA          NA            20
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20
    ## # ℹ 39 more rows

``` r
select(litters_df, group, starts_with("pups")) # select all variables that starts with "pups"
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

``` r
# other options: ends_with(), contains(), see ?select

select(litters_df, -starts_with("gd")) # removes those that start with "gd"
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

``` r
select(litters_df, group, litter_id=litter_number) # rename variable
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
select(litters_df, group, litter_id=litter_number, everything()) # keep everything else
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
select(litters_df, gd0_weight, everything()) # give "gd0_weight" first and keep everything after
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
