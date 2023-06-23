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
    ##  $ content    : raw [1:1123054] 7b 22 71 75 ...
    ##  $ date       : POSIXct[1:1], format: "2023-06-23 02:35:20"
    ##  $ times      : Named num [1:6] 0 0.0169 0.042 0.1495 0.4654 ...
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
    ##  $ request_id  : chr "c08bef0482d5c15c3c803852675e4d4e"
    ##  $ count       : int 10953

``` r
parsedJan$results %>% colnames()
```

    ## [1] "T"  "v"  "vw" "o"  "c"  "h"  "l"  "t"  "n"

I am getting my data from the API using my key. I then created a data
table with the results section from the API.

### Making A Data Frame for January 9th Data

``` r
parsedJanDataFrame <- as.data.frame(parsedJan)
colnames(parsedJanDataFrame) <- c("Query", "Total_Request", "Adjusted", "Symbol", "Volume", "Volume_Weight", "Open", "Close", "High", "Low", "Timestamp", "Transactions", "Status", "Request_ID", "Count")
```

I created a data frame for the January 9th data from the API, and
relabeled the variable names.

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
    ##  $ content    : raw [1:1113109] 7b 22 71 75 ...
    ##  $ date       : POSIXct[1:1], format: "2023-06-23 02:35:21"
    ##  $ times      : Named num [1:6] 0 0.000032 0.000034 0.000109 0.515095 ...
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
    ##  $ request_id  : chr "e340b797e649239b2f78018f3e963283"
    ##  $ count       : int 10844

``` r
parsedMar$results %>% colnames()
```

    ## [1] "T"  "v"  "vw" "o"  "c"  "h"  "l"  "t"  "n"

I am getting my data from the API using my key. I then created a data
table with the results section from the API.

### Making A Data Frame for March 9th Data

``` r
parsedMarDataFrame <- as.data.frame(parsedMar)
colnames(parsedMarDataFrame) <- c("Query", "Total_Request", "Adjusted", "Symbol", "Volume", "Volume_Weight", "Open", "Close", "High", "Low", "Timestamp", "Transactions", "Status", "Request_ID", "Count")
```

I created a data frame for the March 9th data from the API, and
relabeled the variable names.

### Combining the Two Data Frames and Adding a Month, Difference, and Return Variables

I hid these statements because the list of data was very long. I dropped
the na’s from the dataset and created two new variables, month and
difference between the opening stock price and closing stock price. I
then did a full join of the two data frames from each month to create
one big data frame.

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
    ## 1 15.9600 1.673298e+12         5416     OK c08bef0482d5c15c3c803852675e4d4e
    ## 2 18.4500 1.673298e+12         8130     OK c08bef0482d5c15c3c803852675e4d4e
    ## 3 25.8204 1.673298e+12         2417     OK c08bef0482d5c15c3c803852675e4d4e
    ## 4 57.0250 1.673298e+12         7325     OK c08bef0482d5c15c3c803852675e4d4e
    ## 5 72.5900 1.673298e+12        15947     OK c08bef0482d5c15c3c803852675e4d4e
    ## 6  9.3500 1.673298e+12          983     OK c08bef0482d5c15c3c803852675e4d4e
    ##   Count   Month Difference  Returns
    ## 1 10953 January      -0.12 Negative
    ## 2 10953 January      -0.31 Negative
    ## 3 10953 January       0.04 Positive
    ## 4 10953 January       0.87 Positive
    ## 5 10953 January       1.31 Positive
    ## 6 10953 January      -0.10 Negative

I created an else if statement that will determine whether the stocks in
the full dataset had a positive outcome during the day.

### Creating a Contingency Table between Returns and Month

``` r
table(parsedFullData$Returns, parsedFullData$Month)
```

    ##           
    ##            January March
    ##   Negative    4285  1560
    ##   Positive    6668  9284

This contingency table shows that overall, stocks had more of a positive
growth on March 9th than January 9th. Although, both days shows positive
growth, 85.6% of March 9th stocks increased compared to only 60.9% on
January 9th.

### Creating a Contingency Table bewteen Month and Opening Price (\$20.00)

``` r
Twenty <- vector()
for (i in seq_len(nrow(parsedFullData))) {
  if(parsedFullData$Open[i] >= 20) {
    Twenty[i] <- "Higher"
  } else if (parsedFullData$Open[i] <= 20) {
    Twenty[i] <- "Lower"
  } else {
    Twenty[i] <- "Error"
  }
}
parsedFullData$Twenty <- Twenty

table(parsedFullData$Twenty, parsedFullData$Month)
```

    ##         
    ##          January March
    ##   Higher    5315  5417
    ##   Lower     5638  5427

Based on this contingency table comparing the opening price of each
stock to the month variable, a \$20.00 stock seems to be around the
median. In January, 5315 stocks were above an opening price of \$20.00
while 5638 stocks were below this price. In March, these totals were
5417 above and 5427 below the opening price of \$20.00.
