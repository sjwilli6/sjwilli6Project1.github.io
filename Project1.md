Project 1
================
Spencer Williams
2023-06-25

## Packages Needed

``` r
library(httr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(jsonlite)
library(rmarkdown)
```

### Installing Movie Data

``` r
myData <- GET("https://api.polygon.io/v2/aggs/grouped/locale/us/market/stocks/2023-01-09?adjusted=true&apiKey=BnFt9sqaLWehzloLkpMtEeOG4WjAqkUc")
str(myData, max.level = 1)
```

    ## List of 10
    ##  $ url        : chr "https://api.polygon.io/v2/aggs/grouped/locale/us/market/stocks/2023-01-09?adjusted=true&apiKey=BnFt9sqaLWehzloL"| __truncated__
    ##  $ status_code: int 200
    ##  $ headers    :List of 7
    ##   ..- attr(*, "class")= chr [1:2] "insensitive" "list"
    ##  $ all_headers:List of 1
    ##  $ cookies    :'data.frame': 0 obs. of  7 variables:
    ##  $ content    : raw [1:1123055] 7b 22 71 75 ...
    ##  $ date       : POSIXct[1:1], format: "2023-06-18 01:24:37"
    ##  $ times      : Named num [1:6] 0 0.0013 0.0452 0.1939 0.4222 ...
    ##   ..- attr(*, "names")= chr [1:6] "redirect" "namelookup" "connect" "pretransfer" ...
    ##  $ request    :List of 7
    ##   ..- attr(*, "class")= chr "request"
    ##  $ handle     :Class 'curl_handle' <externalptr> 
    ##  - attr(*, "class")= chr "response"

``` r
parsed <- fromJSON(rawToChar(myData$content))
str(parsed, max.level = 1)
```

    ## List of 7
    ##  $ queryCount  : int 10953
    ##  $ resultsCount: int 10953
    ##  $ adjusted    : logi TRUE
    ##  $ results     :'data.frame':    10953 obs. of  9 variables:
    ##  $ status      : chr "OK"
    ##  $ request_id  : chr "806f253bde2f8ac15268431f5428f880"
    ##  $ count       : int 10953

``` r
parsed$results %>% colnames()
```

    ## [1] "T"  "v"  "vw" "o"  "c"  "h"  "l"  "t"  "n"

### Making Functions
