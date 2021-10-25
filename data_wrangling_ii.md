Data\_wrangling\_II
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(viridis)
```

    ## Loading required package: viridisLite

``` r
library(ggridges)
library(patchwork)
library(httr)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

New packages: httr and rvest, have to load seperately, part of tidy
verse ecosystem tho

\#\#scrapping table Want the first tabel from \[this page\]:
<http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.html>

reading the html

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"


drug_use_html <- read_html(url)
```

Etratcing the tables: selector gadgets;

``` r
tabl_marj <- 
drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>% 
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

^A total of 15 tables are extratced. we are going to fcus on the first
tabel (first(), nth(), last()); slice used to access specifc data lines,
(-1) removed first line, as\_tibble turns dataframe int a tibble

\#\#star wars want data from here:

``` r
swm_html = 
  read_html("https://www.imdb.com/list/ls070150896/")
```

Now we only grab elements we want, but for this page we dont know what
css tag IMDB uses for movie info pages. This is where selector gagets
comes in

``` r
title_vec <- 
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
  html_text()


gross_rev_vec <- 
   swm_html %>% 
  html_nodes(css = ".text-muted .ghost~ .text-muted+ span") %>% 
  html_text()

run_time_vec <- 
  swm_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

swm_df <- 
  tibble(
    tittle  = title_vec,
    gross_rev = gross_rev_vec,
    runtime = run_time_vec)
```
