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

### Loading data

First, we load the data in the bike data from the csv file and also rename some columns for easier use in later analysis.

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
colnames(bikes)[colnames(bikes)=="Passholder Type"] <- "passholdertype"
```

### Loading data

First, we load the data in the bike data from the csv file and also rename some columns for easier use in later analysis.

``` r
x = bikes %>%
  group_by(passholdertype) %>%
  summarise_at(vars(duration), funs(mean(., na.rm=TRUE)))
x$duration = x$duration/60

ggplot(x, aes(passholdertype, duration)) + geom_col() + labs(title="The average duration of use between different Pass Holders") + labs(x = "Pass Holder Type") + labs(y = "Average Duration (Minutes)")
```

![](Capital_One_Project_files/figure-markdown_github/duration-1.png)

### What are the most popular start/stop stations?

Let's take a look at the most popular start/stop stations. In order to do this, I first grouped by start and end station id's and then counted the number of occurences of those id's.

``` r
MostFreqentStart = count(bikes, ssid) %>%
  arrange(desc(n)) %>%
  head(5) 
```

    ## Warning: package 'bindrcpp' was built under R version 3.4.4

``` r
names(MostFreqentStart) <- c("Starting Station ID", "Frequency")
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

### What is the average distance traveled?

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

Here, we can see that the mean amount of time people spent on their bikes was 1555 seconds, which is about 26 minutes. If we assume these people were biking at a average pace of around 10mph, we can conclude that the average trip length was around 26/60\*10 miles, or `4.3` miles.

### How many riders include bike sharing as a regular part of their commute?

``` r
RegCommute = count(bikes, passholdertype) %>%
  arrange(desc(n)) %>%
  head(5)
names(RegCommute) <- c("Pass Type", "# of people using")
RegCommute
```

    ## # A tibble: 4 x 2
    ##   `Pass Type`  `# of people using`
    ##   <chr>                      <int>
    ## 1 Monthly Pass               81304
    ## 2 Walk-up                    41224
    ## 3 Flex Pass                   9517
    ## 4 Staff Annual                 382

Let's assume that people who use the walk-up pass aren't regular users and therefore don't use it as part of their commute. Summing up the people who bought passes, 81304+41224+382 = `122,910`. Thus, we conclude that `(122,910)/(122,910+41224)` is the % of people using the bikes for their regular commute. This evalulates to around 74.8% of users.
