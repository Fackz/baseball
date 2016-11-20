---
layout: page
title: Data Aquisition Functions
subtitle: 
---

`baseballr` comes with a number of functions that help with acquiring data from various sources.

###Standings

For example, if you want to see the standings for a specific MLB division on a given date, you can use the `standings_on_date_bref()` function. Just pass the year, month, day, and division you want:

```R
> standings_on_date_bref("2015-08-01", "NL East", from = FALSE)
$`NL East`
   Tm  W  L  W-L%   GB  RS  RA pythW-L%
1 WSN 54 48 0.529   -- 422 391    0.535
2 NYM 54 50 0.519  1.0 368 373    0.494
3 ATL 46 58 0.442  9.0 379 449    0.423
4 MIA 42 62 0.404 13.0 370 408    0.455
5 PHI 41 64 0.390 14.5 386 511    0.374
```

###Daily Player Performance

You can also pull data for all hitters over a specific date range. Here are the results for all hitters from August 1st through October 3rd during the 2015 season:

```R
> head(daily_batter_bref("2015-08-01", "2015-10-03"))
  season             Name Age  Level          Team  G  PA  AB  R  H X1B X2B X3B HR RBI BB IBB uBB SO HBP SH SF GDP SB CS    BA   OBP
1   2015    Manny Machado  22 MLB-AL     Baltimore 59 266 237 36 66  43  10   0 13  32 26   1  25 42   2  0  1   5  6  4 0.278 0.353
2   2015       Matt Duffy  24 MLB-NL San Francisco 59 264 248 33 71  54  12   2  3  30 15   0  15 35   0  0  1   9  8  0 0.286 0.326
3   2015      Jose Altuve  25 MLB-AL       Houston 57 262 244 30 81  53  19   3  6  18 10   1   9 28   4  1  3   6 11  4 0.332 0.364
4   2015       Adam Eaton  26 MLB-AL       Chicago 58 262 230 37 74  56  12   1  5  31 23   1  22 55   5  2  2   1  9  4 0.322 0.392
5   2015    Shin-Soo Choo  32 MLB-AL         Texas 58 260 211 48 71  47  14   1  9  34 39   1  38 51   8  1  1   1  2  0 0.336 0.456
6   2015 Francisco Lindor  21 MLB-AL     Cleveland 58 259 224 35 79  51  17   4  7  32 18   0  18 38   1 11  5   4 10  2 0.353 0.395
    SLG   OPS
1 0.485 0.839
2 0.387 0.713
3 0.508 0.872
4 0.448 0.840
5 0.540 0.996
6 0.558 0.953
```

###FanGraphs Leaderboards

###Statcast

The `edge_scrape()` function allows the user to scrape PITCHf/x data from the GameDay application using Carson Sievert's [pitchRx](https://github.com/cpsievert/pitchRx) package and to calculate metrics associated with [Edge%](https://billpetti.shinyapps.io/edge_shiny/). The function returns a dataframe grouped by either pitchers or batters and the percentge of pitches in each of the various Edge zones.

Example (pitchers):

```r
> edge_scrape("2015-04-06", "2015-04-07", "pitcher") %>% .[, c(1:3,7:12)] %>% head(10)
     pitcher_name pitcher All_pitches Upper_Edge Lower_Edge Inside_Edge Outside_Edge Heart Out_of_Zone
            (chr)   (dbl)       (int)      (dbl)      (dbl)       (dbl)        (dbl) (dbl)       (dbl)
1   Bartolo Colon  112526          86      0.035      0.081       0.058        0.151 0.209       0.465
2  LaTroy Hawkins  115629          12      0.000      0.333       0.000        0.000 0.083       0.583
3      Joe Nathan  150274           4      0.000      0.000       0.000        0.000 0.000       1.000
4   Buddy Carlyle  234194           9      0.000      0.222       0.000        0.000 0.333       0.444
5    Jason Grilli  276351          14      0.000      0.000       0.214        0.000 0.286       0.500
6     Kevin Gregg  276514          17      0.000      0.000       0.118        0.176 0.235       0.471
7  Joaquin Benoit  276542          19      0.053      0.053       0.105        0.000 0.158       0.632
8  Ryan Vogelsong  285064          99      0.010      0.051       0.141        0.061 0.182       0.556
9  Jeremy Affeldt  346793           5      0.000      0.000       0.200        0.000 0.000       0.800
10  Grant Balfour  346797          21      0.095      0.000       0.000        0.048 0.333       0.524
```

Example (batters):

```r
> edge_scrape("2015-04-06", "2015-04-07", "batter") %>% .[, c(1:3,7:12)] %>% head(10)
       batter_name batter All_pitches Upper_Edge Lower_Edge Inside_Edge Outside_Edge Heart Out_of_Zone
             (chr)  (dbl)       (int)      (dbl)      (dbl)       (dbl)        (dbl) (dbl)       (dbl)
1    Bartolo Colon 112526           7      0.000      0.000       0.429        0.000 0.143       0.429
2     Torii Hunter 116338          19      0.000      0.105       0.105        0.105 0.000       0.684
3      David Ortiz 120074          18      0.056      0.000       0.111        0.056 0.222       0.556
4   Alex Rodriguez 121347          17      0.000      0.000       0.353        0.000 0.118       0.529
5   Aramis Ramirez 133380          23      0.000      0.087       0.261        0.000 0.261       0.391
6    Adrian Beltre 134181          26      0.000      0.038       0.154        0.115 0.231       0.462
7   Carlos Beltran 136860          22      0.136      0.045       0.136        0.000 0.136       0.545
8  Michael Cuddyer 150212          14      0.000      0.214       0.214        0.000 0.214       0.357
9    Jimmy Rollins 276519          41      0.024      0.122       0.049        0.049 0.220       0.537
10  Ryan Vogelsong 285064          10      0.000      0.200       0.300        0.000 0.200       0.300
```
