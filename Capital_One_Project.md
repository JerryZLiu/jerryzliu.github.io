Capital One Project
================
Jerry Liu
October 2018

### Load libraries

``` r
library("tidyverse")
```

    ## ── Attaching packages ────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.7
    ## ✔ tidyr   0.8.2     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## Warning: package 'ggplot2' was built under R version 3.4.4

    ## Warning: package 'tidyr' was built under R version 3.4.4

    ## Warning: package 'purrr' was built under R version 3.4.4

    ## Warning: package 'dplyr' was built under R version 3.4.4

    ## Warning: package 'stringr' was built under R version 3.4.4

    ## ── Conflicts ───────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

### Which start/stop stations are most popular?

First, we load the data from the Wiki page that we scraped earlier in the r script.

``` r
bikes <- read_csv("metro-bike-share-trip-data.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Trip ID` = col_integer(),
    ##   Duration = col_integer(),
    ##   `Start Time` = col_datetime(format = ""),
    ##   `End Time` = col_datetime(format = ""),
    ##   `Starting Station ID` = col_integer(),
    ##   `Starting Station Latitude` = col_double(),
    ##   `Starting Station Longitude` = col_double(),
    ##   `Ending Station ID` = col_integer(),
    ##   `Ending Station Latitude` = col_double(),
    ##   `Ending Station Longitude` = col_double(),
    ##   `Bike ID` = col_integer(),
    ##   `Plan Duration` = col_integer(),
    ##   `Trip Route Category` = col_character(),
    ##   `Passholder Type` = col_character(),
    ##   `Starting Lat-Long` = col_character(),
    ##   `Ending Lat-Long` = col_character()
    ## )

``` r
colnames(bikes)[colnames(bikes)=="Starting Station Latitude"] <- "slat"
colnames(bikes)[colnames(bikes)=="Starting Station Longitude"] <- "slong"
colnames(bikes)[colnames(bikes)=="Starting Station ID"] <- "ssid"
colnames(bikes)[colnames(bikes)=="Ending Station ID"] <- "esid"
colnames(bikes)[colnames(bikes)=="Duration"] <- "duration"
MostFreqentStart = count(bikes, ssid) %>%
  arrange(desc(n)) %>%
  head(5) 
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

``` r
names(MostFreqentStart) <- c("Starting Station ID", "Frequency")
MostFreqentStart
```

    ## # A tibble: 5 x 2
    ##   `Starting Station ID` Frequency
    ##                   <int>     <int>
    ## 1                  3069      5138
    ## 2                  3030      5059
    ## 3                  3005      4883
    ## 4                  3064      4661
    ## 5                  3031      4629

``` r
MostFreqentEnd = count(bikes, esid) %>%
  arrange(desc(n)) %>%
  head(5) 
names(MostFreqentEnd) <- c("Ending Station ID", "Frequency")
MostFreqentEnd
```

    ## # A tibble: 5 x 2
    ##   `Ending Station ID` Frequency
    ##                 <int>     <int>
    ## 1                3005      6262
    ## 2                3031      5517
    ## 3                3014      5385
    ## 4                3042      5293
    ## 5                3069      5072

``` r
names(MostFreqentStart) <- c("Starting Station ID", "Frequency")
MostFreqentStart
```

    ## # A tibble: 5 x 2
    ##   `Starting Station ID` Frequency
    ##                   <int>     <int>
    ## 1                  3069      5138
    ## 2                  3030      5059
    ## 3                  3005      4883
    ## 4                  3064      4661
    ## 5                  3031      4629

### Question 2

``` r
bikes = bikes %>%
  filter(duration>1)
mean(bikes$duration)
```

    ## [1] 1555.302

``` r
1555.302
```

    ## [1] 1555.302
