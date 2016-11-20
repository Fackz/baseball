---
layout: page
title: Data Aquisition Functions
subtitle: 
---

`baseballr` comes with a number of functions that help with acquiring data from various sources.

### Standings

You  can acquire the standings for any given date in MLB history from [Baseball-Reference.com](http://www.baseball-reference.com) using the `standings_on_date_bref()` function. Just pass the year, month, day, and division you want:

```R
> standings_on_date_bref(date = "2015-08-01", division = "NL East", from = FALSE)
$`NL East`
   Tm  W  L  W-L%   GB  RS  RA pythW-L%
1 WSN 54 48 0.529   -- 422 391    0.535
2 NYM 54 50 0.519  1.0 368 373    0.494
3 ATL 46 58 0.442  9.0 379 449    0.423
4 MIA 42 62 0.404 13.0 370 408    0.455
5 PHI 41 64 0.390 14.5 386 511    0.374
```

If you want to see the standings from a particular date forward you can set the `from` parameter to `TRUE`:

```R
> standings_on_date_bref(date = "2015-08-01", division = "NL East", from = TRUE)
$`NL East`
   Tm  W  L  W-L%   GB  RS  RA pythW-L%
1 NYM 36 22 0.621   -- 315 240    0.622
2 MIA 29 29 0.500  7.0 243 270    0.452
3 WSN 29 31 0.483  8.0 281 244    0.564
4 PHI 22 35 0.386 13.5 240 298    0.402
5 ATL 21 37 0.362 15.0 194 311    0.297
````

The function can also generate standings across an entire league (AL or NL) by including "Overall" in the division paramater:

```R
> standings_on_date_bref(date = "2015-08-01", division = "NL Overall", from = TRUE)
$`NL Overall`
    Tm  W  L  W-L%   GB  RS  RA pythW-L%
1  CHC 41 18 0.695   -- 293 218    0.632
2  PIT 38 21 0.644  3.0 271 219    0.596
3  NYM 36 22 0.621  4.5 315 240    0.622
4  STL 34 24 0.586  6.5 232 219    0.526
5  LAD 33 25 0.569  7.5 236 229    0.514
6  MIA 29 29 0.500 11.5 243 270    0.452
7  ARI 29 31 0.483 12.5 270 269    0.502
8  WSN 29 31 0.483 12.5 281 244    0.564
9  SFG 27 32 0.458 14.0 247 231    0.531
10 MIL 24 33 0.421 16.0 247 275    0.451
11 COL 24 36 0.400 17.5 270 317    0.427
12 SDP 23 35 0.397 17.5 244 279    0.439
13 PHI 22 35 0.386 18.0 240 298    0.402
14 ATL 21 37 0.362 19.5 194 311    0.297
15 CIN 17 43 0.283 24.5 227 302    0.372
```

### Daily Player Performance

You can also pull data for all hitters or pitchers over a specific date range from [Baseball-Reference.com](http://www.baseball-reference.com) using the `daily` functions. There are two parameters: t1 is the start date and t2 is the end date.

Here are the results for all hitters from August 1st through October 3rd during the 2015 season:

```R
> head(daily_batter_bref(t1 = "2015-08-01", t2 = "2015-10-03"))
  season             Name Age  Level          Team  G  PA  AB  R  H X1B X2B X3B
1   2015    Manny Machado  22 MLB-AL     Baltimore 59 266 237 36 66  43  10   0
2   2015       Matt Duffy  24 MLB-NL San Francisco 59 264 248 33 71  54  12   2
3   2015      Jose Altuve  25 MLB-AL       Houston 57 262 244 30 81  53  19   3
4   2015       Adam Eaton  26 MLB-AL       Chicago 58 262 230 37 74  56  12   1
5   2015    Shin-Soo Choo  32 MLB-AL         Texas 58 260 211 48 71  47  14   1
6   2015 Francisco Lindor  21 MLB-AL     Cleveland 58 259 224 35 79  51  17   4
  HR RBI BB IBB uBB SO HBP SH SF GDP SB CS    BA   OBP   SLG   OPS
1 13  32 26   1  25 42   2  0  1   5  6  4 0.278 0.353 0.485 0.839
2  3  30 15   0  15 35   0  0  1   9  8  0 0.286 0.326 0.387 0.713
3  6  18 10   1   9 28   4  1  3   6 11  4 0.332 0.364 0.508 0.872
4  5  31 23   1  22 55   5  2  2   1  9  4 0.322 0.392 0.448 0.840
5  9  34 39   1  38 51   8  1  1   1  2  0 0.336 0.456 0.540 0.996
6  7  32 18   0  18 38   1 11  5   4 10  2 0.353 0.395 0.558 0.953
```

The function works the same for pitchers:

```R
> head(daily_pitcher_bref(t1 = "2015-08-01", t2 = "2015-10-03"))
  season              Name Age  Level          Team  G GS  W  L SV   IP  H  R ER uBB
1   2015   Clayton Kershaw  27 MLB-NL   Los Angeles 12 12  8  1 NA 89.0 56 17 16  15
2   2015      Jake Arrieta  29 MLB-NL       Chicago 12 12 11 NA NA 88.1 41  7  4  14
3   2015  Justin Verlander  32 MLB-AL       Detroit 12 12  4  5 NA 83.1 63 28 23  19
4   2015   Hisashi Iwakuma  34 MLB-AL       Seattle 12 12  7  3 NA 82.0 65 25 24  12
5   2015    Dallas Keuchel  27 MLB-AL       Houston 12 12  8  3 NA 81.0 69 28 25  17
6   2015 Madison Bumgarner  25 MLB-NL San Francisco 11 11  7  3 NA 80.1 54 21 19  12
  BB  SO HR HBP  ERA  AB X1B X2B X3B IBB GDP SF SB CS PO  BF  Pit  Str  StL  StS GB.FB
1 15 109  4   1 1.62 314  43   9   0   0   3  0  1  3  2 331 1278 0.69 0.17 0.16  0.47
2 14  89  1   3 0.41 302  33   6   1   0   7  0 11  0  0 320 1280 0.65 0.17 0.13  0.65
3 20  78  4   1 2.48 302  43  14   2   1   3  3  2  3  0 327 1330 0.65 0.16 0.10  0.37
4 13  74  7   1 2.63 301  45  13   0   1   5  1  0  1  0 319 1154 0.69 0.18 0.11  0.52
5 17  84  9   2 2.78 304  50   9   1   0   7  2  3  0  0 325 1293 0.63 0.19 0.12  0.58
6 13  95  5   1 2.13 289  40   7   2   1   2  3  4  1  0 308 1208 0.66 0.16 0.13  0.46
    LD   PU  WHIP BAbip  SO9 SO.W SO_perc uBB_perc SO_uBB
1 0.28 0.07 0.798 0.259 11.0 7.27   0.329    0.045      0
2 0.21 0.04 0.623 0.189  9.1 6.36   0.278    0.044      0
3 0.21 0.16 0.996 0.265  8.4 3.90   0.239    0.058      0
4 0.27 0.05 0.951 0.262  8.1 5.69   0.232    0.038      0
5 0.22 0.07 1.062 0.282  9.3 4.94   0.258    0.052      0
6 0.28 0.07 0.834 0.255 10.6 7.31   0.308    0.039      0
```

### FanGraphs Guts and Park Factors 

*Guts*

[FanGraphs](http://fangraphs.com) provides visitors with a table of many of the components and constants they use for calculating some metrics on the site, such as wOBA and FIP. 

The `fg_guts()` function will pull this table and format it as a data frame when called (there are no paramters needed):

```R
> head(fg_guts())
  season lg_woba woba_scale   wBB  wHBP   w1B   w2B   w3B   wHR runSB  runCS lg_r_pa
1   2016   0.318      1.212 0.691 0.721 0.878 1.242 1.569 2.015   0.2 -0.410   0.118
2   2015   0.313      1.251 0.687 0.718 0.881 1.256 1.594 2.065   0.2 -0.392   0.113
3   2014   0.310      1.304 0.689 0.722 0.892 1.283 1.635 2.135   0.2 -0.377   0.108
4   2013   0.314      1.277 0.690 0.722 0.888 1.271 1.616 2.101   0.2 -0.384   0.110
5   2012   0.315      1.245 0.691 0.722 0.884 1.257 1.593 2.058   0.2 -0.398   0.114
6   2011   0.316      1.264 0.694 0.726 0.890 1.270 1.611 2.086   0.2 -0.394   0.112
  lg_r_w  cFIP
1  9.778 3.147
2  9.421 3.134
3  9.117 3.132
4  9.264 3.048
5  9.544 3.095
6  9.454 3.025
```
*Park Factors*

You can also obtain park factors from FanGraphs for every team and every season.

For overall park factors, use the `fg_park()` function, supplying the year you are interested in:

```R> head(fg_park(1986))
  season home_team basic single double triple  hr  so UIBB GB FB LD IFFB FIP
1   1986    Angels    98     99     92     81 105 100   99 NA NA NA   NA 101
2   1986   Orioles    98     99     97     75 103 102  102 NA NA NA   NA 101
3   1986   Red Sox   103    102    112    105  98 101   99 NA NA NA   NA  98
4   1986 White Sox   103    100    102    122  97 100  106 NA NA NA   NA 100
5   1986   Indians   101    104     98     95  99  97   99 NA NA NA   NA 100
6   1986    Tigers    97     97     91     90 105 100   98 NA NA NA   NA 101
```
For park factors by batter handedness, use the `fg_park_hand()` function. Note that handedness park factors are only available going back to the 2002 season:

```R
> head(fg_park_hand(2012))
  season home_team single_as_LHH single_as_RHH double_as_LHH double_as_RHH
1   2012    Angels            99           100            95            96
2   2012   Orioles           101           101           102            98
3   2012   Red Sox           102           104           117           113
4   2012 White Sox            98            99           101            97
5   2012   Indians           101            98           101           102
6   2012    Tigers           103           101            93           102
  triple_as_LHH triple_as_RHH hr_as_LHH hr_as_RHH
1            91            85        91        93
2            88            85       114       104
3           112            88        90       102
4            94            87       106       114
5            82            85       109        93
6           122           126       100       100
```

### Leaderboards




### Statcast

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

### Team Schedule and Results

Sometimes it is helpful to have the game-to-game schedule and results for a given team. This can easily be acquired using the `team_results_bref()` function. You need to supply the team acronym used at [Baseball-Reference.com](http://www.baseball-reference.com) and the season you are interested in.

Here's an example looking at the 1927 Yankees (yes, their third game of the season ended in a tie--odd, I know):

```R
> head(team_results_bref("NYY", 1927))
  Year Rk Gm#              Date  Tm H_A Opp Result  R RA Inn Record Rank     GB
1 1927  1   1   Tuesday, Apr 12 NYY   H PHA      W  8  3  NA    1-0    1   Tied
2 1927  2   2 Wednesday, Apr 13 NYY   H PHA      W 10  4  NA    2-0    1 up 0.5
3 1927  3   3  Thursday, Apr 14 NYY   H PHA      T  9  9  10    2-0    1   Tied
4 1927  4   4    Friday, Apr 15 NYY   H PHA      W  6  3  NA    3-0    1   Tied
5 1927  5   5  Saturday, Apr 16 NYY   H BOS      W  5  2  NA    4-0    1 up 1.0
6 1927  6   6    Sunday, Apr 17 NYY   H BOS      W 14  2  NA    5-0    1 up 2.0
      Win    Loss Save Time D/N Attendance Streak
1    Hoyt   Grove      2:05   D      72000      1
2 Ruether    Gray      2:15   D       8000      2
3                      2:50   D       9000      2
4 Pennock   Ehmke      2:27   D      16000      3
5 Shocker Ruffing      2:05   D      25000      4
6    Hoyt Russell      2:01   D      35000      5
```
