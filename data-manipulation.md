Simple document
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

Import FAS Litters and Pups

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
pups_df = janitor::clean_names(pups_df)
```

## `select` is used to select columns!

Put in your data frame `litters_df`, then the columns you want it to
keep. When you put in the colon, it pulls in everything in between those
two variables. If you use \##starts_with(“pups”), it will pull in every
variable that starts with pups. Also is an “ends with” and a “contains”
options. You can remove variables by subtracting. Can use \###everything
function to keep everything except what you are referring to.

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
select(litters_df, gd0_weight:gd_of_birth, group)
```

    ## # A tibble: 49 × 4
    ##    gd0_weight gd18_weight gd_of_birth group
    ##         <dbl>       <dbl>       <dbl> <chr>
    ##  1       19.7        34.7          20 Con7 
    ##  2       27          42            19 Con7 
    ##  3       26          41.4          19 Con7 
    ##  4       28.5        44.1          19 Con7 
    ##  5       NA          NA            20 Con7 
    ##  6       NA          NA            20 Con7 
    ##  7       NA          NA            20 Con7 
    ##  8       NA          NA            20 Con8 
    ##  9       NA          NA            20 Con8 
    ## 10       28.5        NA            20 Con8 
    ## # ℹ 39 more rows

``` r
select(litters_df, -litter_number)
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

`select` vs. `pull`

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

Put a data frame in, get a data frame out, doesn’t care that there is
only one variable.

``` r
pull(litters_df, group)
```

    ##  [1] "Con7" "Con7" "Con7" "Con7" "Con7" "Con7" "Con7" "Con8" "Con8" "Con8"
    ## [11] "Con8" "Con8" "Con8" "Con8" "Con8" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7"
    ## [21] "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Mod7" "Low7" "Low7" "Low7"
    ## [31] "Low7" "Low7" "Low7" "Low7" "Low7" "Mod8" "Mod8" "Mod8" "Mod8" "Mod8"
    ## [41] "Mod8" "Mod8" "Low8" "Low8" "Low8" "Low8" "Low8" "Low8" "Low8"

Extracts a column from your dataframe, now it’s just like a vector, it
no longer exists inside the dataframe. Pull is still better than a \$,
because it is still a tidyverse function, and you know it’s going to
come out as a vector. If you need to get something out of a dataframe,
use `pull`, it’s much better than the \$.

You can reorganize things a bit with `rename` and `relocate` Relocate
will bring that variable to be the first column.

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

Practicing

``` r
select(litters_df, litter_number)
```

    ## # A tibble: 49 × 1
    ##    litter_number  
    ##    <chr>          
    ##  1 #85            
    ##  2 #1/2/95/2      
    ##  3 #5/5/3/83/3-3  
    ##  4 #5/4/2/95/2    
    ##  5 #4/2/95/3-3    
    ##  6 #2/2/95/3-2    
    ##  7 #1/5/3/83/3-3/2
    ##  8 #3/83/3-3      
    ##  9 #2/95/3        
    ## 10 #3/5/2/2/95    
    ## # ℹ 39 more rows

``` r
select(litters_df, pups_survive)
```

    ## # A tibble: 49 × 1
    ##    pups_survive
    ##           <dbl>
    ##  1            3
    ##  2            7
    ##  3            5
    ##  4            4
    ##  5            6
    ##  6            4
    ##  7            9
    ##  8            8
    ##  9            8
    ## 10            8
    ## # ℹ 39 more rows

These `select` code lines do NOT save the selected columns to be the
dataframe. Essentially just temporarily extracting them… would need to
do \##litters_df = select(###) for it to save the selection as the new
dataframe and overwrite the old dataframe of litters_df

Get rid of rows using `filter()`. Below means “keep everywhere that
group variable is equal to this value… one equal sign means put this
value into the variable, two equals signs means, check if this thing is
true. Keep everywhere that this turns out to be true.

``` r
filter(litters_df, group == "Con7")
```

    ## # A tibble: 7 × 8
    ##   group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                   19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2             27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3           NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2           NA          NA            20               6
    ## 7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

A bit more complicated. WIth select, you name the variables you want to
keep. Filtering is based on something that exists inside the dataframe…
get rid of mod7 group, keep pups born on gestational day 20… keep this
or don’t keep this. Logical conditions. Where this really becomes an
important component.

``` r
filter(litters_df, group == "Mod8")
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Mod8  #97                 24.5        42.8          20               8
    ## 2 Mod8  #5/93               NA          41.1          20              11
    ## 3 Mod8  #5/93/2             NA          NA            19               8
    ## 4 Mod8  #7/82-3-2           26.9        43.2          20               7
    ## 5 Mod8  #7/110/3-2          27.5        46            19               8
    ## 6 Mod8  #2/95/2             28.5        44.5          20               9
    ## 7 Mod8  #82/4               33.4        52.7          20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

Exclamation point != means “not equal to”

``` r
filter(litters_df, gd0_weight > 20)
```

    ## # A tibble: 30 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  4 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  5 Con8  #5/4/3/83/3         28          NA            19               9
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #4/2/95/2           23.5        NA            19               9
    ##  9 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 10 Mod7  #106                21.7        37.8          20               5
    ## # ℹ 20 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, gd0_weight <= 20)
```

    ## # A tibble: 4 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Mod7  #59                 17          33.4          19               8
    ## 3 Mod7  #62                 19.5        35.9          19               7
    ## 4 Low8  #100                20          39.2          20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group == "Con7" | group == "Con8")
```

    ## # A tibble: 15 × 8
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
    ## 11 Con8  #5/4/3/83/3           28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 14 Con8  #2/2/95/2             NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

\##The vertical bar means “OR” (\|)##

## %in% also an OR

``` r
filter(litters_df, group %in% c("Con7", "Con8"))
```

    ## # A tibble: 15 × 8
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
    ## 11 Con8  #5/4/3/83/3           28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 14 Con8  #2/2/95/2             NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, !(group == "Con7"))
```

    ## # A tibble: 42 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con8  #3/83/3-3             NA          NA            20               9
    ##  2 Con8  #2/95/3               NA          NA            20               8
    ##  3 Con8  #3/5/2/2/95           28.5        NA            20               8
    ##  4 Con8  #5/4/3/83/3           28          NA            19               9
    ##  5 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ##  6 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ##  7 Con8  #2/2/95/2             NA          NA            19               5
    ##  8 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ##  9 Mod7  #59                   17          33.4          19               8
    ## 10 Mod7  #103                  21.4        42.1          19               9
    ## # ℹ 32 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

\##Filtere where group is not equal to

Can also combine terms…

``` r
filter(litters_df, group %in% c("Con7", "Con8"), gd0_weight > 20) 
```

    ## # A tibble: 5 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #1/2/95/2           27          42            19               8
    ## 2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 4 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## 5 Con8  #5/4/3/83/3         28          NA            19               9
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

\##Filter the dataframe so that group is con7 or con8 and gd0 weight is
greater than 20.

## `mutate` is used to add or change variables.

``` r
mutate(litters_df, wt_gain = gd18_weight - gd0_weight)
```

    ## # A tibble: 49 × 9
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
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

``` r
mutate(litters_df, group = str_to_lower(group))
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

you can put as many `mutate` functions together in one command as you
want

``` r
mutate(
  litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group))
```

    ## # A tibble: 49 × 9
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 con7  #85                   19.7        34.7          20               3
    ##  2 con7  #1/2/95/2             27          42            19               8
    ##  3 con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 con8  #3/83/3-3             NA          NA            20               9
    ##  9 con8  #2/95/3               NA          NA            20               8
    ## 10 con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

## if you need to omit missing values, that is more of a filtering rows issue. Drop NA function will get rid of missing values anywhere, and you can also specify certain columns to drop missing values.

``` r
drop_na(litters_df)
```

    ## # A tibble: 31 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Mod7  #59                 17          33.4          19               8
    ##  6 Mod7  #103                21.4        42.1          19               9
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Mod7  #94/2               24.4        42.9          19               7
    ## # ℹ 21 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
drop_na(litters_df, gd0_weight)
```

    ## # A tibble: 34 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con8  #3/5/2/2/95         28.5        NA            20               8
    ##  6 Con8  #5/4/3/83/3         28          NA            19               9
    ##  7 Mod7  #59                 17          33.4          19               8
    ##  8 Mod7  #103                21.4        42.1          19               9
    ##  9 Mod7  #3/82/3-2           28          45.9          20               5
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## # ℹ 24 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## `arrange` works the same way, puts it in order of the values, either alphabetic or numeric, of the data points in each column

``` r
arrange(litters_df, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Mod7  #59                 17          33.4          19               8
    ##  2 Mod7  #62                 19.5        35.9          19               7
    ##  3 Con7  #85                 19.7        34.7          20               3
    ##  4 Low8  #100                20          39.2          20               8
    ##  5 Mod7  #103                21.4        42.1          19               9
    ##  6 Mod7  #106                21.7        37.8          20               5
    ##  7 Low8  #53                 21.8        37.2          20               8
    ##  8 Low8  #4/84               21.8        35.2          20               4
    ##  9 Low7  #85/2               22.2        38.5          20               8
    ## 10 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

## pipes. Two years ago, everyone was writing coder and putting it online and the only one that existed looke like %\>%. Moving forward, mroe and more people are using \|\>, and most people are using this. They both exist, it doesn’t really matter which you use, anytime you load tidyverse you have access to both and in MOST cases they will give you the same info. Shortcut on a Mac is shift + CMD + M, which puts in this “and then.” Puts two lines of code together.

With the code below, I will create the dataframe, read in the data,
clean the names, get rid of everything that starts with “pups,” then
create two new varibles via `mutate` then drop missing variables, and
then arrange by group and weight gain.

Generally speaking, this is how we would import dataset and create a
dataframe. Manipulate the data and then just save that. And this is your
save, until you get to the actual analysis portion, which will be a
different set of code.

``` r
litters_df =
  read_csv("data/FAS_litters.csv") |> 
  janitor::clean_names() |> 
  select(-starts_with("pups")) |> 
  mutate(
    group = str_to_lower(group),
    wt_gain = gd18_weight - gd0_weight,
  ) |> 
  drop_na(wt_gain) |> 
  arrange(group, wt_gain)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
