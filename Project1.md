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
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ forcats   1.0.0     ✔ readr     2.1.4
    ## ✔ ggplot2   3.4.2     ✔ stringr   1.5.0
    ## ✔ lubridate 1.9.2     ✔ tibble    3.2.1
    ## ✔ purrr     1.0.1     ✔ tidyr     1.3.0

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter()  masks stats::filter()
    ## ✖ purrr::flatten() masks jsonlite::flatten()
    ## ✖ dplyr::lag()     masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

### Installing Stock Data from January 9

``` r
library(dplyr)
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
    ##  $ content    : raw [1:1123052] 7b 22 71 75 ...
    ##  $ date       : POSIXct[1:1], format: "2023-06-21 00:43:51"
    ##  $ times      : Named num [1:6] 0 0.00227 0.04133 0.17773 0.46311 ...
    ##   ..- attr(*, "names")= chr [1:6] "redirect" "namelookup" "connect" "pretransfer" ...
    ##  $ request    :List of 7
    ##   ..- attr(*, "class")= chr "request"
    ##  $ handle     :Class 'curl_handle' <externalptr> 
    ##  - attr(*, "class")= chr "response"

``` r
parsedJan <- fromJSON(rawToChar(myData$content))
str(parsedJan, max.level = 1)
```

    ## List of 7
    ##  $ queryCount  : int 10953
    ##  $ resultsCount: int 10953
    ##  $ adjusted    : logi TRUE
    ##  $ results     :'data.frame':    10953 obs. of  9 variables:
    ##  $ status      : chr "OK"
    ##  $ request_id  : chr "868f160edd3af514e2ae4e06759d08c0"
    ##  $ count       : int 10953

``` r
parsedJan$results %>% colnames()
```

    ## [1] "T"  "v"  "vw" "o"  "c"  "h"  "l"  "t"  "n"

### Making A Data Frame for January 9th Data

``` r
parsedJanDataFrame <- as.data.frame(parsedJan)
colnames(parsedJanDataFrame) <- c("Query", "Total_Request", "Adjusted", "Symbol", "Volume", "Volume_Weight", "Open", "Close", "High", "Low", "Timestamp", "Transactions", "Status", "Request_ID", "Count")
```

### Installing Stock Data from March 9

``` r
library(dplyr)
myData2 <- GET("https://api.polygon.io/v2/aggs/grouped/locale/us/market/stocks/2023-03-09?adjusted=true&apiKey=BnFt9sqaLWehzloLkpMtEeOG4WjAqkUc")
str(myData2, max.level = 1)
```

    ## List of 10
    ##  $ url        : chr "https://api.polygon.io/v2/aggs/grouped/locale/us/market/stocks/2023-03-09?adjusted=true&apiKey=BnFt9sqaLWehzloL"| __truncated__
    ##  $ status_code: int 200
    ##  $ headers    :List of 7
    ##   ..- attr(*, "class")= chr [1:2] "insensitive" "list"
    ##  $ all_headers:List of 1
    ##  $ cookies    :'data.frame': 0 obs. of  7 variables:
    ##  $ content    : raw [1:1113102] 7b 22 71 75 ...
    ##  $ date       : POSIXct[1:1], format: "2023-06-21 00:43:52"
    ##  $ times      : Named num [1:6] 0 0.000031 0.000033 0.000095 0.447737 ...
    ##   ..- attr(*, "names")= chr [1:6] "redirect" "namelookup" "connect" "pretransfer" ...
    ##  $ request    :List of 7
    ##   ..- attr(*, "class")= chr "request"
    ##  $ handle     :Class 'curl_handle' <externalptr> 
    ##  - attr(*, "class")= chr "response"

``` r
parsedMar <- fromJSON(rawToChar(myData2$content))
str(parsedMar, max.level = 1)
```

    ## List of 7
    ##  $ queryCount  : int 10844
    ##  $ resultsCount: int 10844
    ##  $ adjusted    : logi TRUE
    ##  $ results     :'data.frame':    10844 obs. of  9 variables:
    ##  $ status      : chr "OK"
    ##  $ request_id  : chr "f82498e6c7d7fbff1c71b9c6ab8d33cc"
    ##  $ count       : int 10844

``` r
parsedMar$results %>% colnames()
```

    ## [1] "T"  "v"  "vw" "o"  "c"  "h"  "l"  "t"  "n"

### Making A Data Frame for March 9th Data

``` r
parsedMarDataFrame <- as.data.frame(parsedMar)
colnames(parsedMarDataFrame) <- c("Query", "Total_Request", "Adjusted", "Symbol", "Volume", "Volume_Weight", "Open", "Close", "High", "Low", "Timestamp", "Transactions", "Status", "Request_ID", "Count")
```

### Combining the Two Data Frames and Adding a Month, Difference, and Return Variables

``` r
Returns <- vector()
for (i in seq_len(nrow(parsedFullData))) {
  if(parsedFullData$Difference[i] >= 0) {
    Returns[i] <- "Positive"
  } else if (parsedFullData$Difference[i] <= 0) {
    Returns[i] <- "Negative"
  } else {
    Returns[i] <- "Error"
  }
}
parsedFullData$Returns <- Returns

head(parsedFullData)
```

    ##   Query Total_Request Adjusted Symbol  Volume Volume_Weight  Open Close   High
    ## 1 10953         10953     TRUE   TTMI  394280       16.1078 15.96 16.08 16.335
    ## 2 10953         10953     TRUE    OEC  485157       19.0627 18.67 18.98 19.430
    ## 3 10953         10953     TRUE   USDU  642813       25.8745 25.92 25.88 25.950
    ## 4 10953         10953     TRUE   BWXT  389669       57.4657 58.15 57.28 58.180
    ## 5 10953         10953     TRUE    WRB 1298028       73.2759 74.29 72.98 74.380
    ## 6 10953         10953     TRUE    WIW  161653        9.4369  9.38  9.48  9.500
    ##       Low    Timestamp Transactions Status                       Request_ID
    ## 1 15.9600 1.673298e+12         5416     OK 868f160edd3af514e2ae4e06759d08c0
    ## 2 18.4500 1.673298e+12         8130     OK 868f160edd3af514e2ae4e06759d08c0
    ## 3 25.8204 1.673298e+12         2417     OK 868f160edd3af514e2ae4e06759d08c0
    ## 4 57.0250 1.673298e+12         7325     OK 868f160edd3af514e2ae4e06759d08c0
    ## 5 72.5900 1.673298e+12        15947     OK 868f160edd3af514e2ae4e06759d08c0
    ## 6  9.3500 1.673298e+12          983     OK 868f160edd3af514e2ae4e06759d08c0
    ##   Count   Month Difference  Returns
    ## 1 10953 January      -0.12 Negative
    ## 2 10953 January      -0.31 Negative
    ## 3 10953 January       0.04 Positive
    ## 4 10953 January       0.87 Positive
    ## 5 10953 January       1.31 Positive
    ## 6 10953 January      -0.10 Negative

### Creating a Contingency Table

``` r
table(parsedFullData$Returns, parsedFullData$Month)
```

    ##           
    ##            January March
    ##   Negative    4285  1560
    ##   Positive    6668  9284
