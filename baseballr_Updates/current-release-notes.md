---
layout: page
title: baseballr current release notes
tags: rstats, web-scraping, baseballr, statcast, baseballsavant
---

## May 8, 2017

The latest release of the [`baseballr`](https://billpetti.github.io/baseballr/) package for `R` includes a number of enhancement to acquiring data from [Baseball Savant](http://baseballsavant.com) as well as minor grammatical clean up in the documentation.

Previous functions `scrape_statcast_savant_batter` and `scrape_statcast_savant_pitcher` allowed for the acquistion of data from baseballsavant.com for a given player over a user-determined time frame. However, this is somewhat inefficient if you want to acquire data on all players over a given time frame. 

Two new functions have been added, `scrape_statcast_savant_batter_all` and `scrape_statcast_savant_pitcher_all`, that allow a user to acquire data for either all pitchers or all hitters over a given time frame.

Both functions take only two arguments:

`start_date`: the first date for which the user wants records returned
`end_date`: the final date for which the user wants records returned

Remember, baseballsavant.com's csv download option allows for about 50,000 records in a single query. That works out to roughly 10-12 days of games. Longer time frames will take longer to download.

Example: acquire data for all batters from 2017-04-03 through 2017-04-10

```r
> head(scrape_statcast_savant_batter_all('2017-04-03', '2017-04-10'))
[1] "These data are from BaseballSevant and are property of MLB Advanced Media, L.P. All rights reserved."
[1] "Grabbing data, this may take a minute..."
URL read and payload aquired successfully.
  pitch_type  game_date release_speed release_pos_x release_pos_z  player_name
1         FF 2017-04-10          92.7       -1.0367        5.7934   Eric Fryer
2         FF 2017-04-10          93.2       -0.9753        5.6007   Eric Fryer
3         FF 2017-04-10          93.0       -1.1196        5.6958   Eric Fryer
4         FF 2017-04-10          93.1       -0.9952        5.7978   Eric Fryer
5         SL 2017-04-10          83.4       -1.2385        5.8164   Eric Fryer
6         FF 2017-04-10          93.7       -1.0307        5.8740 Aledmys Diaz
  batter pitcher    events     description spin_dir spin_rate_deprecated
1 518700  518875 strikeout swinging_strike       NA                   NA
2 518700  518875      <NA>            ball       NA                   NA
3 518700  518875      <NA>            ball       NA                   NA
4 518700  518875      <NA> swinging_strike       NA                   NA
5 518700  518875      <NA>   called_strike       NA                   NA
6 649557  518875 field_out   hit_into_play       NA                   NA
  break_angle_deprecated break_length_deprecated zone
1                     NA                      NA    5
2                     NA                      NA   12
3                     NA                      NA   12
4                     NA                      NA    3
5                     NA                      NA    6
6                     NA                      NA    6
                                                      des game_type stand
1                      Eric Fryer strikes out swinging.           R     R
2                                                    <NA>         R     R
3                                                    <NA>         R     R
4                                                    <NA>         R     R
5                                                    <NA>         R     R
6 Aledmys Diaz flies out to right fielder Bryce Harper.           R     R
  p_throws home_team away_team type hit_location  bb_type balls strikes
1        R       WSH       STL    S         <NA>     <NA>     2       2
2        R       WSH       STL    B         <NA>     <NA>     1       2
3        R       WSH       STL    B         <NA>     <NA>     0       2
4        R       WSH       STL    S         <NA>     <NA>     0       1
5        R       WSH       STL    S         <NA>     <NA>     0       0
6        R       WSH       STL    X            9 fly_ball     0       1
  game_year   pfx_x  pfx_z plate_x plate_z on_3b on_2b  on_1b outs_when_up
1      2017 -0.4262 1.7261 -0.0042  2.9680    NA    NA 594824            2
2      2017  0.2420 1.3633  1.3747  3.5269    NA    NA 594824            2
3      2017  0.4912 1.6758  0.5389  4.3795    NA    NA 594824            2
4      2017  0.1924 1.7964  0.6868  3.5700    NA    NA 594824            2
5      2017 -0.1604 0.3532  0.6048  2.6308    NA    NA 594824            2
6      2017  0.5956 1.8068  0.4993  3.1386    NA    NA 594824            1
  inning inning_topbot   hc_x   hc_y tfs_deprecated tfs_zulu_deprecated
1      9           Top   <NA>   <NA>             NA                  NA
2      9           Top   <NA>   <NA>             NA                  NA
3      9           Top   <NA>   <NA>             NA                  NA
4      9           Top   <NA>   <NA>             NA                  NA
5      9           Top   <NA>   <NA>             NA                  NA
6      9           Top 186.56 105.27             NA                  NA
  pos2_person_id umpire         sv_id vx0 vy0 vz0 ax ay az sz_top sz_bot
1         446308     NA 170411_025210  NA  NA  NA NA NA NA 3.8420 1.5890
2         446308     NA 170411_025153  NA  NA  NA NA NA NA 3.5602 1.7127
3         446308     NA 170411_025133  NA  NA  NA NA NA NA 3.6761 1.6780
4         446308     NA 170411_025117  NA  NA  NA NA NA NA 3.6760 1.5040
5         446308     NA 170411_025104  NA  NA  NA NA NA NA 3.5139 1.6548
6         446308     NA 170411_025018  NA  NA  NA NA NA NA 3.9500 1.6810
  hit_distance_sc launch_speed launch_angle effective_speed release_spin_rate
1              NA           NA           NA          93.033              2285
2              NA           NA           NA          93.301              2323
3              NA           NA           NA          92.892              2322
4              NA           NA           NA          92.906              2324
5              NA           NA           NA          83.371                NA
6             266         87.5       47.444          93.529              2406
  release_extension game_pk pos1_person_id pos2_person_id.1 pos3_person_id
1             6.248  490201         518875           446308         475582
2             6.265  490201         518875           446308         475582
3             6.281  490201         518875           446308         475582
4             6.187  490201         518875           446308         475582
5             6.155  490201         518875           446308         475582
6             6.269  490201         518875           446308         475582
  pos4_person_id pos5_person_id pos6_person_id pos7_person_id pos8_person_id
1         502517         543685         452220         594809         572191
2         502517         543685         452220         594809         572191
3         502517         543685         452220         594809         572191
4         502517         543685         452220         594809         572191
5         502517         543685         452220         594809         572191
6         502517         543685         452220         594809         572191
  pos9_person_id release_pos_y estimated_ba_using_speedangle
1         547180       54.2491                         0.000
2         547180       54.2319                         0.000
3         547180       54.2163                         0.000
4         547180       54.3096                         0.000
5         547180       54.3420                         0.000
6         547180       54.2282                         0.007
  estimated_woba_using_speedangle woba_value woba_denom babip_value iso_value
1                           0.000       0.00          1           0         0
2                           0.000       <NA>       <NA>        <NA>      <NA>
3                           0.000       <NA>       <NA>        <NA>      <NA>
4                           0.000       <NA>       <NA>        <NA>      <NA>
5                           0.000       <NA>       <NA>        <NA>      <NA>
6                           0.008       0.00          1           0         0
  barrel
1     NA
2     NA
3     NA
4     NA
5     NA
6      0
```
##  November 22, 2016

_(Previous release notes can be found [here](https://github.com/BillPetti/baseballr/tree/master/baseballr_Updates))_

The latest release of the [`baseballr`](https://billpetti.github.io/baseballr/) (0.3.1) includes a function for acquiring player statistics from the [NCAA's website](http://stats.ncaa.org) for baseball teams across the three major divisions (I, II, III).

The function, `ncaa_scrape`, requires the user to pass values for three parameters for the function to work:

`school_id`: numerical code used by the NCAA for each school
`year`: a four-digit year
`type`: whether to pull data for batters or pitchers

If you want to pull batting statistics for Vanderbilt for the 2013 season, you would use the following:

```r
> baseballr::ncaa_scrape(736, 2013, "batting") %>%
+     select(year:OBPct)
   year     school   conference division Jersey            Player Yr Pos GP GS    BA OBPct
1  2013 Vanderbilt Southeastern        1     18 Yastrzemski, Mike Sr  OF 66 66 0.312 0.411
2  2013 Vanderbilt Southeastern        1     20   Harrell, Connor Sr  OF 66 66 0.312 0.418
3  2013 Vanderbilt Southeastern        1      3      Conde, Vince So INF 66 65 0.307 0.380
4  2013 Vanderbilt Southeastern        1      6        Kemp, Tony Jr  OF 66 66 0.391 0.471
5  2013 Vanderbilt Southeastern        1     55    Gregor, Conrad Jr  OF 65 65 0.308 0.440
6  2013 Vanderbilt Southeastern        1      9    Turner, Xavier Fr INF 59 51 0.324 0.387
7  2013 Vanderbilt Southeastern        1      5    Navin, Spencer Jr   C 57 56 0.302 0.430
8  2013 Vanderbilt Southeastern        1     51        Lupo, Jack Sr  OF 57 51 0.297 0.352
9  2013 Vanderbilt Southeastern        1      8    Wiseman, Rhett Fr  OF 54 11 0.289 0.360
10 2013 Vanderbilt Southeastern        1     10     Norwood, John So  OF 33  9 0.328 0.388
11 2013 Vanderbilt Southeastern        1     43      Wiel, Zander So INF 33 15 0.305 0.406
12 2013 Vanderbilt Southeastern        1     44     Harvey, Chris So   C 29 13 0.250 0.328
13 2013 Vanderbilt Southeastern        1     42   McKeithan, Joel Jr INF 25 12 0.220 0.267
14 2013 Vanderbilt Southeastern        1     39       Smith, Kyle Fr INF 23  7 0.250 0.455
15 2013 Vanderbilt Southeastern        1     17    Harris, Andrew Sr INF 21  0 0.125 0.222
16 2013 Vanderbilt Southeastern        1      2   Campbell, Tyler Fr INF 12  2 0.312 0.389
17 2013 Vanderbilt Southeastern        1      7   Swanson, Dansby Fr INF 11  4 0.188 0.435
18 2013 Vanderbilt Southeastern        1     25        Luna, D.J. Jr INF  8  0 0.000 0.333
19 2013 Vanderbilt Southeastern        1     23      Cooper, Will So  OF  4  0 1.000 1.000
20 2013 Vanderbilt Southeastern        1      -            Totals  -   -  -  - 0.313 0.407
21 2013 Vanderbilt Southeastern        1      -   Opponent Totals  -   -  -  - 0.220 0.320
```

The same can be done for pitching, just by changing the `type` parameter:

```r
> baseballr::ncaa_scrape(736, 2013, "pitching") %>%
+     select(year:ERA)
   year     school   conference division Jersey           Player Yr Pos GP App GS  ERA
1  2013 Vanderbilt Southeastern        1     11     Beede, Tyler So   P 37  17 17 2.32
2  2013 Vanderbilt Southeastern        1     33    Miller, Brian So   P 32  32 NA 1.58
3  2013 Vanderbilt Southeastern        1     35    Ziomek, Kevin Jr   P 32  17 17 2.12
4  2013 Vanderbilt Southeastern        1     15   Fulmer, Carson Fr   P 26  26 NA 2.39
5  2013 Vanderbilt Southeastern        1     39      Smith, Kyle Fr INF 23   1 NA 0.00
6  2013 Vanderbilt Southeastern        1     28    Miller, Jared So   P 22  22 NA 2.31
7  2013 Vanderbilt Southeastern        1     19     Rice, Steven Jr   P 21  21 NA 2.57
8  2013 Vanderbilt Southeastern        1     13  Buehler, Walker Fr   P 16  16  9 3.14
9  2013 Vanderbilt Southeastern        1     22  Pfeifer, Philip So   P 15  15 12 3.68
10 2013 Vanderbilt Southeastern        1     12  Ravenelle, Adam So   P 11  11 NA 3.18
11 2013 Vanderbilt Southeastern        1     40   Pecoraro, T.J. Jr   P 10  10  7 5.97
12 2013 Vanderbilt Southeastern        1     45  Ferguson, Tyler Fr   P  8   8  4 4.21
13 2013 Vanderbilt Southeastern        1     27 Kolinsky, Keenan Jr   P  2   2 NA 0.00
14 2013 Vanderbilt Southeastern        1     24    Wilson, Nevin So   P  1   1 NA 0.00
15 2013 Vanderbilt Southeastern        1      -           Totals  -   -  -  NA NA 2.76
16 2013 Vanderbilt Southeastern        1      -  Opponent Totals  -   -  -  NA NA 6.19
```

Now, the function is dependent on the user knowing the `school_id` used by the NCAA website. Given that, I've included a `school_id_lu` function so that users can find the `school_id` they need.

Just pass a string to the function and it will return possible matches based on the school's name:

```r
> school_id_lu("Vand")
# A tibble: 4 × 6
      school   conference school_id  year division conference_id
       <chr>        <chr>     <dbl> <dbl>    <dbl>         <dbl>
1 Vanderbilt Southeastern       736  2013        1           911
2 Vanderbilt Southeastern       736  2014        1           911
3 Vanderbilt Southeastern       736  2015        1           911
4 Vanderbilt Southeastern       736  2016        1           911
```

