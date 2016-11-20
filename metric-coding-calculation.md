---
layout: page
title: Metric Coding and Calculation Functions
subtitle: 
---


### Edge% 

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

There is also a version of the function that allows you to break out the data by handedness of pitchers or batters faced:

```r
> edge_scrape_split("2015-04-06", "2015-04-07", "batter") %>% .[, c(1:3,7:12)] %>% head(10)
      batter_name batter p_throws Called_Strike Called_strike_rate Upper_Edge Lower_Edge
            <chr>  <dbl>    <chr>         <dbl>              <dbl>      <dbl>      <dbl>
1   Bartolo Colon 112526        R             2              0.500      0.000      0.000
2    Torii Hunter 116338        L             3              0.375      0.000      0.133
3    Torii Hunter 116338        R             0              0.000      0.000      0.000
4     David Ortiz 120074        L             2              0.500      0.083      0.000
5     David Ortiz 120074        R             1              0.333      0.000      0.000
6  Alex Rodriguez 121347        L             1              0.500      0.000      0.000
7  Alex Rodriguez 121347        R             1              0.125      0.000      0.000
8  Aramis Ramirez 133380        R             2              0.250      0.000      0.087
9   Adrian Beltre 134181        R             6              0.429      0.000      0.038
10 Carlos Beltran 136860        L             0              0.000      0.143      0.000
```

If you already have the appropriate PITCHf/x data available and simply want to code each pitch according to Edge% the `edge_code` function will do that for you.

Simply supply a dataframe to the function and it will code each pitch. The dataframe must contain at least three columns of data: `b_height`, `stand`, `px`, and `pz`.

Example (based on data from "2015-04-05"):

```r
> edge_code(df) %>% .[, c(6:7, 27:28, 82)] %>% head(10)
   stand b_height     px    pz    location
1      L      6-3  0.416 2.963 Inside Edge
2      L      6-3 -0.191 2.347       Heart
3      L      6-3 -0.518 3.284  Upper Edge
4      L      6-3 -0.641 1.221 Out of Zone
5      L      6-3 -1.821 2.083 Out of Zone
6      L      6-3  0.627 2.397 Inside Edge
7      L      6-5 -1.088 1.610 Out of Zone
8      L      6-5 -0.257 2.047  Lower Edge
9      L      6-5     NA    NA        <NA>
10     L      6-3 -1.539 1.525 Out of Zone
```

You can also take the data you've coded and calculate Edge% frequency using `edge_frequency`. Supply the function a dataframe as well as a value for `group`; this can be `batter`, `pitcher`, or `NULL` which will calculate the frequency over the entire dataframe.

