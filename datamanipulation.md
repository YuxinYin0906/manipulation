dataManipulation
================
2023-09-30

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

``` r
options(tibble.print_min = 3)

litters_data = read_csv("./Data/FAS_litters.csv",
  col_types = "ccddiiii")

##When you specify col_types like this, it helps the data reading function understand how to interpret the data in the file correctly

litters_data = janitor::clean_names(litters_data)  #clean up the names to R-accepted format


pups_data = read_csv("./Data/FAS_pups.csv",
  col_types = "ciiiii")
pups_data = janitor::clean_names(pups_data)
```

## Select: select Columns

``` r
select(litters_data, group, litter_number, gd0_weight, pups_born_alive)
```

    ## # A tibble: 49 × 4
    ##   group litter_number gd0_weight pups_born_alive
    ##   <chr> <chr>              <dbl>           <int>
    ## 1 Con7  #85                 19.7               3
    ## 2 Con7  #1/2/95/2           27                 8
    ## 3 Con7  #5/5/3/83/3-3       26                 6
    ## # ℹ 46 more rows

### Select suffixes or similar patterns columns

``` r
select(litters_data, starts_with("gd"))
```

    ## # A tibble: 49 × 3
    ##   gd0_weight gd18_weight gd_of_birth
    ##        <dbl>       <dbl>       <int>
    ## 1       19.7        34.7          20
    ## 2       27          42            19
    ## 3       26          41.4          19
    ## # ℹ 46 more rows

### Selct with `everything` function to reorganize the order of cols without discarding anything

``` r
select(litters_data, litter_number, pups_survive, everything())
```

    ## # A tibble: 49 × 8
    ##   litter_number pups_survive group gd0_weight gd18_weight gd_of_birth
    ##   <chr>                <int> <chr>      <dbl>       <dbl>       <int>
    ## 1 #85                      3 Con7        19.7        34.7          20
    ## 2 #1/2/95/2                7 Con7        27          42            19
    ## 3 #5/5/3/83/3-3            5 Con7        26          41.4          19
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_born_alive <int>, pups_dead_birth <int>

#### Assessment

``` r
select(pups_data, litter_number, sex, pd_ears)
```

    ## # A tibble: 313 × 3
    ##   litter_number   sex pd_ears
    ##   <chr>         <int>   <int>
    ## 1 #85               1       4
    ## 2 #85               1       4
    ## 3 #1/2/95/2         1       5
    ## # ℹ 310 more rows

## Filter

### Omitting value: `drop_na`

``` r
drop_na(litters_data)
```

    ## # A tibble: 31 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 28 more rows
    ## # ℹ 2 more variables: pups_dead_birth <int>, pups_survive <int>

### Assessment 2: filter

``` r
pups_data%>%
  filter(sex == 1)
```

    ## # A tibble: 155 × 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <int>   <int>   <int>    <int>   <int>
    ## 1 #85               1       4      13        7      11
    ## 2 #85               1       4      13        7      12
    ## 3 #1/2/95/2         1       5      13        7       9
    ## # ℹ 152 more rows

``` r
pups_data%>%
  filter(pd_walk < 11 & sex == 2)
```

    ## # A tibble: 127 × 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <int>   <int>   <int>    <int>   <int>
    ## 1 #1/2/95/2         2       4      13        7       9
    ## 2 #1/2/95/2         2       4      13        7      10
    ## 3 #1/2/95/2         2       5      13        8      10
    ## # ℹ 124 more rows
