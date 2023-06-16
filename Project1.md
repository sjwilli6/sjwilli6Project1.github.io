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
```

### Installing Movie Data

``` r
myData <- GET("http://www.omdbapi.com/?i=tt3896198&apikey=b1e5e254")
str(myData, max.level = 1)
```

    ## List of 10
    ##  $ url        : chr "http://www.omdbapi.com/?i=tt3896198&apikey=b1e5e254"
    ##  $ status_code: int 200
    ##  $ headers    :List of 15
    ##   ..- attr(*, "class")= chr [1:2] "insensitive" "list"
    ##  $ all_headers:List of 1
    ##  $ cookies    :'data.frame': 0 obs. of  7 variables:
    ##  $ content    : raw [1:1064] 7b 22 54 69 ...
    ##  $ date       : POSIXct[1:1], format: "2023-06-16 01:54:46"
    ##  $ times      : Named num [1:6] 0 0.0479 0.0776 0.0777 0.1855 ...
    ##   ..- attr(*, "names")= chr [1:6] "redirect" "namelookup" "connect" "pretransfer" ...
    ##  $ request    :List of 7
    ##   ..- attr(*, "class")= chr "request"
    ##  $ handle     :Class 'curl_handle' <externalptr> 
    ##  - attr(*, "class")= chr "response"

``` r
parsed <- fromJSON(rawToChar(myData$content))
str(parsed, max.level = 1)
```

    ## List of 25
    ##  $ Title     : chr "Guardians of the Galaxy Vol. 2"
    ##  $ Year      : chr "2017"
    ##  $ Rated     : chr "PG-13"
    ##  $ Released  : chr "05 May 2017"
    ##  $ Runtime   : chr "136 min"
    ##  $ Genre     : chr "Action, Adventure, Comedy"
    ##  $ Director  : chr "James Gunn"
    ##  $ Writer    : chr "James Gunn, Dan Abnett, Andy Lanning"
    ##  $ Actors    : chr "Chris Pratt, Zoe Saldana, Dave Bautista"
    ##  $ Plot      : chr "The Guardians struggle to keep together as a team while dealing with their personal family issues, notably Star"| __truncated__
    ##  $ Language  : chr "English"
    ##  $ Country   : chr "United States"
    ##  $ Awards    : chr "Nominated for 1 Oscar. 15 wins & 60 nominations total"
    ##  $ Poster    : chr "https://m.media-amazon.com/images/M/MV5BNjM0NTc0NzItM2FlYS00YzEwLWE0YmUtNTA2ZWIzODc2OTgxXkEyXkFqcGdeQXVyNTgwNzI"| __truncated__
    ##  $ Ratings   :'data.frame':  3 obs. of  2 variables:
    ##  $ Metascore : chr "67"
    ##  $ imdbRating: chr "7.6"
    ##  $ imdbVotes : chr "712,189"
    ##  $ imdbID    : chr "tt3896198"
    ##  $ Type      : chr "movie"
    ##  $ DVD       : chr "22 Aug 2017"
    ##  $ BoxOffice : chr "$389,813,101"
    ##  $ Production: chr "N/A"
    ##  $ Website   : chr "N/A"
    ##  $ Response  : chr "True"

``` r
parsed$articles %>%
colnames()
```

    ## NULL
