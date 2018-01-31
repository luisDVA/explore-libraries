01\_explore-libraries\_comfy.R
================
luisd
Wed Jan 31 14:10:55 2018

Which libraries does R search for packages?

``` r
# try .libPaths(), .Library

.libPaths()
```

    ## [1] "/home/luisd/R/x86_64-pc-linux-gnu-library/3.4"
    ## [2] "/usr/local/lib/R/site-library"                
    ## [3] "/usr/lib/R/site-library"                      
    ## [4] "/usr/lib/R/library"

``` r
1-1
```

    ## [1] 0

Installed packages

``` r
## use installed.packages() to get all installed packages
## if you like working with data frame or tibble, make it so right away!
## remember to use View() or similar to inspect

## how many packages?
```

Exploring the packages

``` r
## count some things! inspiration
##   * tabulate by LibPath, Priority, or both
##   * what proportion need compilation?
##   * how break down re: version of R they were built on

## for tidyverts, here are some useful patterns
# data %>% count(var)
# data %>% count(var1, var2)
# data %>% count(var) %>% mutate(prop = n / sum(n))
```

Reflections

``` r
## reflect on ^^ and make a few notes to yourself; inspiration
##   * does the number of base + recommended packages make sense to you?
##   * how does the result of .libPaths() relate to the result of .Library?
```

Going further

``` r
## if you have time to do more ...

## is every package in .Library either base or recommended?
## study package naming style (all lower case, contains '.', etc
## use `fields` argument to installed.packages() to get more info and use it!
```
