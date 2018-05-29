---
layout: page
title: Metric Coding and Calculation Functions
subtitle: 
---

### Advanced Hitting and Pitching Metrics

Weighted On-base Average (wOBA per plate appearance) and wOBA on contact can be calculated for any set of data over any date range, provided you have the data available. This can be very useful if you use the `daily_batter_bref` function for batter performance over a custom timeframe.

Simply pass the proper data frame to `woba_plus`:

```r
> x <- woba_plus(df)
> head(x)[,c(1,2,24,26,27)]
              Name         Team season  wOBA wOBA_CON
1     Bryce Harper    Nationals   2015 0.464    0.554
2       Joey Votto         Reds   2015 0.428    0.485
3 Paul Goldschmidt Diamondbacks   2015 0.422    0.517
4       Mike Trout       Angels   2015 0.418    0.519
5   Miguel Cabrera       Tigers   2015 0.415    0.462
6   Josh Donaldson    Blue Jays   2015 0.404    0.467
```
You can also generate these wOBA-based stats, as well as Fielding Independent Pitching (FIP), for pitchers using the `fip_plus()` function:

```r
> daily_pitcher_bref("2015-04-05", "2015-04-30") %>%
+   fip_plus() %>% 
+   select(season, Name, IP, ERA, SO, uBB, HBP, HR, FIP, wOBA_against, wOBA_CON_against) %>%
+   arrange(desc(IP)) %>%
+   head(10)
   season            Name   IP  ERA SO uBB HBP HR  FIP wOBA_against wOBA_CON_against
1    2015    Johnny Cueto 37.0 1.95 38   4   2  3 2.62        0.210            0.276
2    2015  Dallas Keuchel 37.0 0.73 22  11   0  0 2.84        0.169            0.151
3    2015      Sonny Gray 36.1 1.98 25   6   1  1 2.69        0.218            0.239
4    2015      Mike Leake 35.2 3.03 25   7   0  5 4.16        0.240            0.281
5    2015 Felix Hernandez 34.2 1.82 36   6   3  1 2.20        0.225            0.272
6    2015    Corey Kluber 34.0 4.24 36   5   2  2 2.40        0.295            0.391
7    2015   Jake Odorizzi 33.2 2.41 26   8   1  0 2.38        0.213            0.228
8    2015 Josh Collmenter 32.2 2.76 16   3   0  1 2.82        0.290            0.330
9    2015   Bartolo Colon 32.2 3.31 25   1   0  4 3.29        0.280            0.357
10   2015    Zack Greinke 32.2 1.93 27   7   1  2 3.01        0.240            0.274
```

If you have downloaded pitch-by-pitch data from [Baseball Savent](https://baseballsavant.mlb.com) you can generate run expectancy and linear weights using these functions:

`run_expectancy_code()`

This function formats Baseball Savant data so that users can generate the run expectancy for different base-out or count-base-out states. It will also append the data frame with new variables necessary for generating linear weights (see new function below). The only argument is a data frame downloaded from Baseball Savant

Columns created and appended to Baseball Savant data:

- `final_pitch_game`: whether a pitch was the final one thrown in a game
- `final_pitch_inning`: whether a pitch is the final one thrown in an inning
- `final_pitch_at_bat`: whether a pitch is the final one thrown in an at bat
- `runs_scored_on_pitch`: how many runs scored as a result of the pitch
- `bat_score_start_inning`: the score for the batting team at the beginning of the inning
- `bat_score_end_inning`: the score for the batting team at the end of the inning
- `bat_score_after`: the score for the batting team after the pitch is thrown
- `cum_runs_in_inning`: how many cumulative runs have been scored from the beginning of the inning through the pitch
- `runs_to_end_inning`: how many runs were scored as a result of the pitch through the end of the inning
- `base_out_state` or `count_base_out_state`: the specific combination of base-outs or count-base-outs when the pitch was thrown
- `avg_re`: the average run expectancy of that base-out or count-base-out state
- `next_avg_re`: the average run expectancy of the base-out or count-base-out state that results from the pitch
- `change_re`: the change in run expectancy as a result of the pitch
- `re24`: the total change in run expectancy through the end of the inning resulting from the pitch based on the change in base-out or count-base-out state plus the number of runs scored as a result of the pitch/at bat

Example:

```r
> x2016_statcast_re <- run_expectancy_code(x2016_statcast)

> sample_n(x2016_statcast_re, 10) %>%
    select(final_pitch_inning:re24) %>%
    glimpse()

Observations: 10
Variables: 11
$ final_pitch_inning     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 1, 0
$ bat_score_start_inning <dbl> 1, 0, 5, 0, 3, 2, 1, 0, 0, 0
$ bat_score_end_inning   <dbl> 2, 0, 5, 1, 3, 2, 5, 0, 0, 2
$ cum_runs_in_inning     <dbl> 1, 0, 0, 0, 0, 0, 2, 0, 0, 1
$ runs_to_end_inning     <dbl> 0, 0, 0, 1, 0, 0, 2, 0, 0, 1
$ base_out_state         <chr> "2  outs,  1b _ _", "0  outs,  _ _ _", "0  outs...
$ avg_re                 <dbl> 0.2149885, 0.5057877, 0.5057877, 0.5057877, 0.5...
$ next_base_out_state    <chr> "2  outs,  1b 2b _", "1  outs,  _ _ _", "1  out...
$ next_avg_re            <dbl> 0.4063525, 0.2718802, 0.2718802, 0.8629357, 0.2...
$ change_re              <dbl> 0.1913640, -0.2339075, -0.2339075, 0.3571479, -...
$ re24                   <dbl> 0.1913640, -0.2339075, -0.2339075, 0.3571479, -...
```

`run_expectancy_table()`

This functions works with the `run_expectancy_code` function and does the work of generating the run expectancy tables that are automatically exported into the Global Environment

Example:

```r
> x2016_statcast_re %>%
  run_expectancy_table() %>%
  print(n=Inf)

A tibble: 24 x 2
base_out_state     avg_re
<chr>               <dbl>
  1 0  outs,  1b 2b 3b  2.13
2 0  outs,  _ 2b 3b   1.95
3 0  outs,  1b _ 3b   1.76
4 1  outs,  1b 2b 3b  1.55
5 0  outs,  1b 2b _   1.42
6 1  outs,  _ 2b 3b   1.36
7 0  outs,  _ _ 3b    1.36
8 1  outs,  1b _ 3b   1.18
9 0  outs,  _ 2b _    1.14
10 1  outs,  _ _ 3b    0.951
11 1  outs,  1b 2b _   0.906
12 0  outs,  1b _ _    0.863
13 2  outs,  1b 2b 3b  0.689
14 1  outs,  _ 2b _    0.669
15 2  outs,  _ 2b 3b   0.525
16 1  outs,  1b _ _    0.520
17 0  outs,  _ _ _     0.506
18 2  outs,  1b _ 3b   0.456
19 2  outs,  1b 2b _   0.406
20 2  outs,  _ _ 3b    0.366
21 2  outs,  _ 2b _    0.299
22 1  outs,  _ _ _     0.272
23 2  outs,  1b _ _    0.215
24 2  outs,  _ _ _     0.106
```

`linear_weights_savant()`

This function works in tandem with `run_expectancy_code()` to generate linear weights for offensive events after the Baseball Savant data has been properly formatted. Currently, the function will return linear weights above average and linear weights above outs. It does not apply any scaling to align with league wOBA. Users can do that themselves if they like, or it may be added to a future version of the function.

Example:

```r

> x2016_statcast_re %>%
  linear_weights_savant() %>%
  print(n=Inf)

A tibble: 7 x 3
events       linear_weights_above_average linear_weights_above_outs
<chr>                               <dbl>                     <dbl>
  1 home_run                            1.38                      1.63
2 triple                              1.00                      1.25
3 double                              0.730                     0.980
4 single                              0.440                     0.690
5 hit_by_pitch                        0.320                     0.570
6 walk                                0.290                     0.540
7 outs                               -0.250                     0.
```

### Edge% 

The `edge_scrape()` function allows the user to scrape PITCHf/x data from the GameDay application using Carson Sievert's [pitchRx](https://github.com/cpsievert/pitchRx) package and to calculate metrics associated with [Edge%](https://billpetti.shinyapps.io/edge_shiny/). The function returns a dataframe grouped by either pitchers or batters and the percentge of pitches in each of the various Edge zones.

Example (pitchers):

```r
> edge_scrape("2015-04-06", "2015-04-07", "pitcher") %>%
+   .[, c(1:3,7:12)] %>%
+   head(10)
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
> edge_scrape("2015-04-06", "2015-04-07", "batter") %>%
+   .[, c(1:3,7:12)] %>%
+   head(10)
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
> edge_scrape_split("2015-04-06", "2015-04-07", "batter") %>%
+   .[, c(1:3,7:12)] %>%
+    head(10)
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
> edge_code(df) %>% .[, c(6:7, 27:28, 82)] %>%
+   head(10)
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

### Statcast

The research team at Major League Baseball Advanced Media have developed a way to categorize batted balls that on average having a batting average over .500 and slugging over 1.500. The specific coding criteria can be found in comment #2 [here](http://tangotiger.com/index.php/site/comments/statcast-lab-barrels#2). 

Now, whenver a user scrapes Statcast data using either the `scrape_statcast_savant_batter` or `scrape_statcast_savant_pitcher` functions the results will include a column `barrel`, where if the batted ball matches the barrel criteria it will code as 1, otherwise 0.

Example:

```r
> scrape_statcast_savant_batter(start_date = "2016-04-06", end_date = "2016-04-15", batterid = 621043) %>% 
+     filter(type == "X") %>%
+     filter(!is.na(barrel)) %>%
+     select(player_name, game_date, hit_angle, hit_speed, barrel) %>%
+     tail()
[1] "Be patient, this may take a few seconds..."
[1] "Data courtesy of Baseball Savant and MLBAM (baseballsavant.mlb.com)"
     player_name  game_date hit_angle hit_speed barrel
25 Carlos Correa 2016-04-07     31.10    103.33      1
26 Carlos Correa 2016-04-07     27.77     87.25      0
27 Carlos Correa 2016-04-06     29.62    103.97      1
28 Carlos Correa 2016-04-06      0.11    105.20      0
29 Carlos Correa 2016-04-06     23.76    113.55      1
30 Carlos Correa 2016-04-06     -2.18    113.39      0
```

Howver, if you already have Statcast data--say, in a database that you've been collecting--`baseballr` includes a simple function that will take a dataframe and code whether each row contains a barrel or not. All you need to do is pass your dataframe to `code_barrel`.

`label_statcast_imputed_data()`

[Ben Dilday](https://github.com/bdilday) created an experimental function meant to tag batted ball cases where significant imputation may have been used to generate some of the Statcast values by MLBAM, i.e. `launch_speed` and `launch_angle`. You can read more about Ben's function [here](https://github.com/BillPetti/baseballr/pull/71). Simply pass a Baseball Savant data frame to the function and it will return the data frame with the new columns and coding appended.

### Stat Lines

If you have pitch-by-pitch data, you can transform that raw data into a more traditional stat line using the `statline_from_statcast` function. Example data includes count data such as number of singles, doubles, etc., as well as rate metrics like Slugging and wOBA on swings or contact.

The function only has two arguments:

* `df`: a dataframe that includes pitch-by-pitch information. The function assumes the following columns are present: `events`, `description`, `game_date`, and `type`.
* `base`: base indicates what the denomincator should be for the rate stats that are calculated. The function defaults to "swings", but you can also choose to use "contact"

Here is an example using all data from the week of 2017-09-04. Here, we want to see a statline for all hitters based on swings:

```r
test <- scrape_statcast_savant_batter_all("2017-09-04", "2017-09-10")

statline_from_statcast(test)

year swings batted_balls  X1B X2B X3B  HR swing_and_miss swinging_strike_percent    ba
1 2017  13790        10663 1129 352  37 259           3127                   0.227 0.129

obp   slg   ops  woba
1 0.129 0.216 0.345 0.144
```
### Team Consistency

I created a metric that attemtps to quantify how consistent teams are in terms of their game-to-game run scoring and run prevention. It is based on Gini coefficients, which are one measure of inequality.

Users can calculate the consistency of team scoring and run prevention for any year using the `team_consistency()` function. Simply pass a year to the function. 

Here's an example for 2015. The results give you the raw consistency scores, as well as percentiles for consistency for both runs scored and runs against:

```r
> team_consistency(2015)
Source: local data frame [30 x 5]

    Team Con_R Con_RA Con_R_Ptile Con_RA_Ptile
   (chr) (dbl)  (dbl)       (dbl)        (dbl)
1    ARI  0.37   0.36          22           15
2    ATL  0.41   0.40          87           67
3    BAL  0.40   0.38          70           42
4    BOS  0.39   0.40          52           67
5    CHC  0.38   0.41          33           88
6    CHW  0.39   0.40          52           67
7    CIN  0.41   0.36          87           15
8    CLE  0.41   0.40          87           67
9    COL  0.35   0.34           7            3
10   DET  0.39   0.38          52           42
..   ...   ...    ...         ...          ...
```
