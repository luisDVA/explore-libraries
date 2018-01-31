CRISP DOCUMENT OUTPUT
================
luisd
Wed Jan 31 14:38:41 2018

Load packages (install first if needed)

``` r
library(fs)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.7.2     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Which libraries does R search for packages?

``` r
.libPaths()
```

    ## [1] "/home/luisd/R/x86_64-pc-linux-gnu-library/3.4"
    ## [2] "/usr/local/lib/R/site-library"                
    ## [3] "/usr/lib/R/site-library"                      
    ## [4] "/usr/lib/R/library"

``` r
## let's confirm the second element is, in fact, the default library
.Library
```

    ## [1] "/usr/lib/R/library"

``` r
path_real(.Library)
```

    ## /usr/lib/R/library

Installed packages

``` r
ipt <- installed.packages() %>%
  as_tibble()

## how many packages?
nrow(ipt)
```

    ## [1] 121

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
ipt %>%
  count(LibPath, Priority)
```

    ## # A tibble: 3 x 3
    ##   LibPath                                       Priority        n
    ##   <chr>                                         <chr>       <int>
    ## 1 /home/luisd/R/x86_64-pc-linux-gnu-library/3.4 <NA>           92
    ## 2 /usr/lib/R/library                            base           14
    ## 3 /usr/lib/R/library                            recommended    15

``` r
##   * what proportion need compilation?
ipt %>%
  count(NeedsCompilation) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 3 x 3
    ##   NeedsCompilation     n   prop
    ##   <chr>            <int>  <dbl>
    ## 1 no                  53 0.438 
    ## 2 yes                 63 0.521 
    ## 3 <NA>                 5 0.0413

``` r
##   * how break down re: version of R they were built on
ipt %>%
  count(Built) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 6 x 3
    ##   Built     n    prop
    ##   <chr> <int>   <dbl>
    ## 1 3.2.2     1 0.00826
    ## 2 3.3.1     1 0.00826
    ## 3 3.3.3     1 0.00826
    ## 4 3.4.0     7 0.0579 
    ## 5 3.4.1     2 0.0165 
    ## 6 3.4.3   109 0.901

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
all_default_pkgs <- list.files(.Library)
all_br_pkgs <- ipt %>%
  filter(Priority %in% c("base", "recommended")) %>%
  pull(Package)
setdiff(all_default_pkgs, all_br_pkgs)
```

    ## [1] "translations"

``` r
## study package naming style (all lower case, contains '.', etc

## use `fields` argument to installed.packages() to get more info and use it!
ipt2 <- installed.packages(fields = "URL") %>%
  as_tibble()
ipt2 %>%
  mutate(github = grepl("github", URL)) %>%
  count(github) %>%
  mutate(prop = n / sum(n))
```

    ## # A tibble: 2 x 3
    ##   github     n  prop
    ##   <lgl>  <int> <dbl>
    ## 1 F         55 0.455
    ## 2 T         66 0.545

For reproducibility

``` r
sessionInfo()
```

    ## R version 3.4.3 (2017-11-30)
    ## Platform: x86_64-pc-linux-gnu (64-bit)
    ## Running under: Linux Mint 18.3
    ## 
    ## Matrix products: default
    ## BLAS: /usr/lib/libblas/libblas.so.3.6.0
    ## LAPACK: /usr/lib/lapack/liblapack.so.3.6.0
    ## 
    ## locale:
    ##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
    ##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
    ##  [5] LC_MONETARY=es_MX.UTF-8    LC_MESSAGES=en_US.UTF-8   
    ##  [7] LC_PAPER=es_MX.UTF-8       LC_NAME=C                 
    ##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
    ## [11] LC_MEASUREMENT=es_MX.UTF-8 LC_IDENTIFICATION=C       
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ##  [1] bindrcpp_0.2    forcats_0.2.0   stringr_1.2.0   dplyr_0.7.4    
    ##  [5] purrr_0.2.4     readr_1.1.1     tidyr_0.7.2     tibble_1.4.2   
    ##  [9] ggplot2_2.2.1   tidyverse_1.2.1 fs_1.1.0       
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] Rcpp_0.12.15     cellranger_1.1.0 pillar_1.1.0     compiler_3.4.3  
    ##  [5] plyr_1.8.4       bindr_0.1        tools_3.4.3      digest_0.6.14   
    ##  [9] lubridate_1.7.1  jsonlite_1.5     evaluate_0.10.1  nlme_3.1-131    
    ## [13] gtable_0.2.0     lattice_0.20-35  pkgconfig_2.0.1  rlang_0.1.6     
    ## [17] psych_1.7.8      cli_1.0.0        rstudioapi_0.7   yaml_2.1.16     
    ## [21] parallel_3.4.3   haven_1.1.1      xml2_1.2.0       httr_1.3.1      
    ## [25] knitr_1.18       hms_0.4.1        rprojroot_1.3-2  grid_3.4.3      
    ## [29] glue_1.2.0       R6_2.2.2         readxl_1.0.0     foreign_0.8-69  
    ## [33] rmarkdown_1.8    modelr_0.1.1     reshape2_1.4.3   magrittr_1.5    
    ## [37] backports_1.1.2  scales_0.5.0     htmltools_0.3.6  rvest_0.3.2     
    ## [41] assertthat_0.2.0 mnormt_1.5-5     colorspace_1.3-2 utf8_1.1.3      
    ## [45] stringi_1.1.6    lazyeval_0.2.1   munsell_0.4.3    broom_0.4.3     
    ## [49] crayon_1.3.4
