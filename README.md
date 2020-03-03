Practice Exam
=============

This practice exam asks you to do several code wrangling tasks that we
have done in class so far.

Clone this repo into Rstudio and fill in the necessary code. Then,
commit and push to github. Finally, turn in a link to canvas.

-   lm(performance ~ origin + ... + dest)
-   ignore "max\_wind\_gust"

<!-- -->

    ## -- Attaching packages ----------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.2.1     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## Warning: package 'ggplot2' was built under R version 3.6.2

    ## Warning: package 'readr' was built under R version 3.6.2

    ## -- Conflicts -------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    ## Warning: package 'nycflights13' was built under R version 3.6.2

    ## 
    ## Attaching package: 'lubridate'

    ## The following object is masked from 'package:base':
    ## 
    ##     date

1. Make a plot with three facets, one for each airport in the weather data. The x-axis should be the day of the year (1:365) and the y-axis should be the mean temperature recorded on that day, at that airport.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

    # check practice.R 

    # add column that counts number of day of the year (2013)
    weather %>% mutate(day_of_year = yday(time_hour))

    ## # A tibble: 26,115 x 16
    ##    origin  year month   day  hour  temp  dewp humid wind_dir wind_speed
    ##    <chr>  <int> <int> <int> <int> <dbl> <dbl> <dbl>    <dbl>      <dbl>
    ##  1 EWR     2013     1     1     1  39.0  26.1  59.4      270      10.4 
    ##  2 EWR     2013     1     1     2  39.0  27.0  61.6      250       8.06
    ##  3 EWR     2013     1     1     3  39.0  28.0  64.4      240      11.5 
    ##  4 EWR     2013     1     1     4  39.9  28.0  62.2      250      12.7 
    ##  5 EWR     2013     1     1     5  39.0  28.0  64.4      260      12.7 
    ##  6 EWR     2013     1     1     6  37.9  28.0  67.2      240      11.5 
    ##  7 EWR     2013     1     1     7  39.0  28.0  64.4      240      15.0 
    ##  8 EWR     2013     1     1     8  39.9  28.0  62.2      250      10.4 
    ##  9 EWR     2013     1     1     9  39.9  28.0  62.2      260      15.0 
    ## 10 EWR     2013     1     1    10  41    28.0  59.6      260      13.8 
    ## # ... with 26,105 more rows, and 6 more variables: wind_gust <dbl>,
    ## #   precip <dbl>, pressure <dbl>, visib <dbl>, time_hour <dttm>,
    ## #   day_of_year <dbl>

    # one for each airport in the weather data

    ## view(weather) to check which airports present

    # EWR <- weather %>% mutate(day_of_year = yday(time_hour)) %>% 
    #   filter(origin == "EWR")
    # 
    # JFK <- weather %>% mutate(day_of_year = yday(time_hour)) %>% 
    #   filter(origin == "JFK")
    # 
    # LGA <- weather %>% mutate(day_of_year = yday(time_hour)) %>% 
    #   filter(origin == "LGA")


    # x-axis = day of the year (1:365)
    # y-axis = mean temperature

    # 1. change time_hour to day of year
    # 2. group by origin and day of year -> ex) EWR-1 one group
    # 3. ggplot with three lines for origin

    weather %>% mutate(day_of_year = yday(time_hour)) %>% group_by(origin, day_of_year) %>% summarize(meanTemp = mean(temp, na.rm = T)) %>% 
      ggplot()+geom_line(aes(x = day_of_year, y = meanTemp, col = origin))

![](README_files/figure-markdown_strict/unnamed-chunk-2-1.png)

facet\_wrap
===========

    weatherQ1 <- weather %>% mutate(day_of_year = yday(time_hour)) %>% group_by(origin, day_of_year) %>% summarize(meanTemp = mean(temp, na.rm = T))

    # str(weatherQ1)

    ggplot(data = weatherQ1) + geom_point(mapping = aes(x= day_of_year, y = meanTemp, color = origin)) + facet_wrap(~origin)

![](README_files/figure-markdown_strict/Q1_answer-1.png)

2. Make a non-tidy matrix of that data where each row is an airport and each column is a day of the year.
---------------------------------------------------------------------------------------------------------

-&gt; each element = average temperature -&gt; some function with '\_'
tidy

    # Check organizing.R

    # row = airport
    # col = day of the year

    weatherQ1 %>% pivot_wider(names_from = day_of_year, values_from = meanTemp)

    ## # A tibble: 3 x 365
    ## # Groups:   origin [3]
    ##   origin   `1`   `2`   `3`   `4`   `5`   `6`   `7`   `8`   `9`  `10`  `11`  `12`
    ##   <chr>  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ## 1 EWR     36.8  28.7  29.6  34.3  36.6  39.9  40.3  38.6  42.1  43.6  42.0  46.0
    ## 2 JFK     36.9  28.6  30.1  34.7  36.8  39.3  40.1  39.4  42.7  43.6  41.3  45.0
    ## 3 LGA     37.2  28.8  30.3  35.8  38.3  41.0  41.4  42.3  44.9  44.3  40.3  43.9
    ## # ... with 352 more variables: `13` <dbl>, `14` <dbl>, `15` <dbl>, `16` <dbl>,
    ## #   `17` <dbl>, `18` <dbl>, `19` <dbl>, `20` <dbl>, `21` <dbl>, `22` <dbl>,
    ## #   `23` <dbl>, `24` <dbl>, `25` <dbl>, `26` <dbl>, `27` <dbl>, `28` <dbl>,
    ## #   `29` <dbl>, `30` <dbl>, `31` <dbl>, `32` <dbl>, `33` <dbl>, `34` <dbl>,
    ## #   `35` <dbl>, `36` <dbl>, `37` <dbl>, `38` <dbl>, `39` <dbl>, `40` <dbl>,
    ## #   `41` <dbl>, `42` <dbl>, `43` <dbl>, `44` <dbl>, `45` <dbl>, `46` <dbl>,
    ## #   `47` <dbl>, `48` <dbl>, `49` <dbl>, `50` <dbl>, `51` <dbl>, `52` <dbl>,
    ## #   `53` <dbl>, `54` <dbl>, `55` <dbl>, `56` <dbl>, `57` <dbl>, `58` <dbl>,
    ## #   `59` <dbl>, `60` <dbl>, `61` <dbl>, `62` <dbl>, `63` <dbl>, `64` <dbl>,
    ## #   `65` <dbl>, `66` <dbl>, `67` <dbl>, `68` <dbl>, `69` <dbl>, `70` <dbl>,
    ## #   `71` <dbl>, `72` <dbl>, `73` <dbl>, `74` <dbl>, `75` <dbl>, `76` <dbl>,
    ## #   `77` <dbl>, `78` <dbl>, `79` <dbl>, `80` <dbl>, `81` <dbl>, `82` <dbl>,
    ## #   `83` <dbl>, `84` <dbl>, `85` <dbl>, `86` <dbl>, `87` <dbl>, `88` <dbl>,
    ## #   `89` <dbl>, `90` <dbl>, `91` <dbl>, `92` <dbl>, `93` <dbl>, `94` <dbl>,
    ## #   `95` <dbl>, `96` <dbl>, `97` <dbl>, `98` <dbl>, `99` <dbl>, `100` <dbl>,
    ## #   `101` <dbl>, `102` <dbl>, `103` <dbl>, `104` <dbl>, `105` <dbl>,
    ## #   `106` <dbl>, `107` <dbl>, `108` <dbl>, `109` <dbl>, `110` <dbl>,
    ## #   `111` <dbl>, `112` <dbl>, ...

3. For each (airport, day) contruct a tidy data set of the airport's "performance" as the proportion of flights that departed less than an hour late.
-----------------------------------------------------------------------------------------------------------------------------------------------------

    # check handling.R & organizing.R


    # data
    ## flights
    # dep_delay = departure delays in minutes
    ## dep_delay < 1

    perfData <- flights %>% group_by(dest, day) %>% summarise(count = n(), noDelay = length(dep_delay[dep_delay < 1]), performance = noDelay / count)

    # each (airport, day)
    # airport's performance = flights departed late < 1 hour

    perfData %>% select(dest, day, performance) %>% 
      pivot_wider(names_from = dest, values_from = performance)

    ## # A tibble: 31 x 106
    ##      day   ABQ   ACK   ALB   ANC   ATL   AUS   AVL   BDL   BGR   BHM   BNA   BOS
    ##    <int> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
    ##  1     1 0.75  0.75  0.333    NA 0.700 0.549 0.667 0.538 0.182 0.444 0.635 0.708
    ##  2     2 0.375 0.5   0.2      NA 0.678 0.5   0.7   0.5   0.692 0.778 0.650 0.708
    ##  3     3 0.375 0.333 0.533     1 0.629 0.605 0.778 0.636 0.364 0.636 0.570 0.712
    ##  4     4 0.625 0.625 0.5      NA 0.748 0.659 0.889 0.727 0.667 0.556 0.675 0.786
    ##  5     5 0.5   0.571 0.667    NA 0.695 0.747 0.889 0.875 0.7   0.875 0.716 0.742
    ##  6     6 0.75  0.8   0.471     0 0.654 0.558 0.778 0.667 0.833 0.7   0.638 0.762
    ##  7     7 0.375 0.625 0.5      NA 0.625 0.582 0.875 0.75  0.364 0.4   0.616 0.690
    ##  8     8 0.25  0.875 0.5      NA 0.619 0.566 0.444 0.615 0.667 0.8   0.569 0.666
    ##  9     9 0.375 1     0.583    NA 0.595 0.590 0.7   0.692 0.583 0.625 0.613 0.665
    ## 10    10 0.375 0.75  0.588     1 0.614 0.506 0.889 0.714 0.385 0.364 0.565 0.642
    ## # ... with 21 more rows, and 93 more variables: BQN <dbl>, BTV <dbl>,
    ## #   BUF <dbl>, BUR <dbl>, BWI <dbl>, BZN <dbl>, CAE <dbl>, CAK <dbl>,
    ## #   CHO <dbl>, CHS <dbl>, CLE <dbl>, CLT <dbl>, CMH <dbl>, CRW <dbl>,
    ## #   CVG <dbl>, DAY <dbl>, DCA <dbl>, DEN <dbl>, DFW <dbl>, DSM <dbl>,
    ## #   DTW <dbl>, EGE <dbl>, EYW <dbl>, FLL <dbl>, GRR <dbl>, GSO <dbl>,
    ## #   GSP <dbl>, HDN <dbl>, HNL <dbl>, HOU <dbl>, IAD <dbl>, IAH <dbl>,
    ## #   ILM <dbl>, IND <dbl>, JAC <dbl>, JAX <dbl>, LAS <dbl>, LAX <dbl>,
    ## #   LEX <dbl>, LGA <dbl>, LGB <dbl>, MCI <dbl>, MCO <dbl>, MDW <dbl>,
    ## #   MEM <dbl>, MHT <dbl>, MIA <dbl>, MKE <dbl>, MSN <dbl>, MSP <dbl>,
    ## #   MSY <dbl>, MTJ <dbl>, MVY <dbl>, MYR <dbl>, OAK <dbl>, OKC <dbl>,
    ## #   OMA <dbl>, ORD <dbl>, ORF <dbl>, PBI <dbl>, PDX <dbl>, PHL <dbl>,
    ## #   PHX <dbl>, PIT <dbl>, PSE <dbl>, PSP <dbl>, PVD <dbl>, PWM <dbl>,
    ## #   RDU <dbl>, RIC <dbl>, ROC <dbl>, RSW <dbl>, SAN <dbl>, SAT <dbl>,
    ## #   SAV <dbl>, SBN <dbl>, SDF <dbl>, SEA <dbl>, SFO <dbl>, SJC <dbl>,
    ## #   SJU <dbl>, SLC <dbl>, SMF <dbl>, SNA <dbl>, SRQ <dbl>, STL <dbl>,
    ## #   STT <dbl>, SYR <dbl>, TPA <dbl>, TUL <dbl>, TVC <dbl>, TYS <dbl>, XNA <dbl>

4. Construct a tidy data set to that give weather summaries for each (airport, day). Use the total precipitation, minimum visibility, maximum wind\_gust, and average wind\_speed.
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Construct a linear model to predict the performance of each
(airport,day) using the weather summaries and a "fixed effect" for each
airport. Display the summaries.

Repeat the above, but only for EWR. Obviously, exclude the fixed effect
for each airport. -&gt; fixed effect?
