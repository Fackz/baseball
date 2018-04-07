---
layout: page
title: Data Visualization Functions
subtitle: 
---

### Standings

`viz_gb_on_period` allows a user to generate a time series of standings for a division and automatically visualize the data in an interactive chart.

```r
> viz_gb_on_period("2018-03-29", "2018-04-05", "NL East")
   |++++++++++++++++++++++++++++++++++++++++++++++++++| 100% elapsed = 16s
# A tibble: 10 x 7
   League  Date       Team      W     L WLpct    GB
   <chr>   <date>     <chr> <int> <int> <dbl> <dbl>
 1 NL East 2018-03-29 NYM       1     0 1.00  0.   
 2 NL East 2018-03-29 ATL       1     0 1.00  0.   
 3 NL East 2018-03-29 WSN       0     0 0.    0.500
 4 NL East 2018-03-29 PHI       0     1 0.    1.00 
 5 NL East 2018-03-29 MIA       0     1 0.    1.00 
 6 NL East 2018-04-05 NYM       5     1 0.833 0.   
 7 NL East 2018-04-05 ATL       4     2 0.667 1.00 
 8 NL East 2018-04-05 WSN       4     3 0.571 1.50 
 9 NL East 2018-04-05 PHI       2     4 0.333 3.00 
10 NL East 2018-04-05 MIA       2     5 0.286 3.50 
```

![alt text](https://github.com/BillPetti/baseballr/blob/gh-pages/vz_gb_chart_ex.png?raw=true "vz_gb ex")

### Spray Charts

`ggspraychart` can generate either a typical spray chart or a density chart for a given hitter. The function takes a data frame with hit coordinates and allows users to customize fill colors and values and the transparency of points. Users can also adjust the bin value when generating density plots.

Keep in mind that the `hc_y` coordinate provided by baseballsavant needs to be inverted in order to properly plot the data. (I typically create a variable, `hc_y_rotated` by multiplying `hc_y` and use that for plotting.)

Here's are point and density examples using data for Jose Altuve:

```r
ggspraychart(data, point_alpha = .6, fill_legend_title = "Hit Type", fill_value = "hit_type", 
               fill_palette = c("1"="#A2C8EC", "2"="#006BA4", "3"="#FF940E",
                                "Out"="#595959", "4"="#C85200")) + 
  facet_wrap(~game_year, nrow = 2) +
  ggtitle("\nJose Altuve") +
  labs(subtitle = "Spray Charts Since 2013\n")
```
![alt text](https://github.com/BillPetti/baseballr/blob/gh-pages/altuve_facet_ex.png?raw=true "facet ex")

```r
ggspraychart(data, point_alpha = .2, density = TRUE, bin_size = 30) + 
  facet_wrap(~game_year, nrow = 2) +
  ggtitle("\nJose Altuve") +
  labs(subtitle = "Spray Charts Since 2013\n")
```
![alt text](https://github.com/BillPetti/baseballr/blob/gh-pages/altuve_facet_density.png?raw=true "density ex")
	
The function is also written in such a way where it can be combined with `gganimate` to create animated plots:

```r
require(gganimate)

years <- c(2013, 2014, 2015, 2016, 2017)

p <- ggspraychart(data, density = TRUE, point_alpha = .2, bin_size = 30, frame = "game_year") + 
  ggtitle("\n\n Jose Altuve's Evolution by Year:") +
  labs(caption = "@BillPetti\nData source: baseballsavant.com\nBuilt with the baseballr package\n") +
  theme(plot.caption = element_text(face = "bold", size = 14))

gganimate(p, ani.width=800, ani.height=800)
```

![alt text](https://github.com/BillPetti/baseballr/blob/gh-pages/Altuve_evolution.gif?raw=true "gif e example") 
	
Be sure whatever variable you assign to the `frame` argument is a factor and the levels are in the desired order for the animation.
