
<!-- ##### *Formatting is off right now. Will be adjusting soon :)* -->

### Contents

- [Team Standings](#team-standings)
- [Offensive and Defensive PPG](#offensive-and-defensive-ppg)
- [Offensive and Defensive YPG](#offensive-and-defensive-ypg)
- [Team Margins](#team-margins)
- [Point-Adjusted Margins](#point-adjusted-margins)
- [Quarter-Based Scoring Trends](#quarter-based-scoring-trends)
- [Offensive and Defensive CPR](#offensive-and-defensive-cpr)
- [Weekly QB CER](#weekly-qb-cer)
- [QB Air Yards v YAC](#qb-air-yards-v-yac)
- [Modeling](#modeling)

------------------------------------------------------------------------

### Team Standings

![](README_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

------------------------------------------------------------------------

### Offensive and Defensive PPG

![](README_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

------------------------------------------------------------------------

### Offensive and Defensive YPG

![](README_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

------------------------------------------------------------------------

### Team Margins

![](README_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

------------------------------------------------------------------------

### Point-Adjusted Margins

![](README_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

------------------------------------------------------------------------

### Quarter-Based Scoring Trends

![](README_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

------------------------------------------------------------------------

### Offensive and Defensive CPR

![](README_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

##### Five Best Total CPRs

1.  BAL: 6.173
2.  SF: 5.342
3.  DAL: 4.441
4.  BUF: 3.47
5.  MIA: 2.476

##### Five Worst Total CPRs

1.  WAS: -5.431
2.  CAR: -4.664
3.  NYG: -3.777
4.  NE: -3.494
5.  ARI: -3.01

------------------------------------------------------------------------

### Weekly QB CER

![](README_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

------------------------------------------------------------------------

### QB Air Yards v YAC

![](README_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

------------------------------------------------------------------------

### Modeling

First draft basic logistic regression accuracy: 63.97%

##### *This Week’s Predictions*

- PIT @ BUF: BUF def. PIT (0.816)
- GB @ DAL: DAL def. GB (0.812)
- LA @ DET: DET def. LA (0.62)
- MIA @ KC: KC def. MIA (0.565)
- CLE @ HOU: HOU def. CLE (0.531)
- PHI @ TB: PHI def. TB (0.504)
- NA
- NA
- NA
- NA
- NA
- NA
- NA
- NA <!-- - NA --> <!-- - NA -->

![](README_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

### Team Margins by Half

![](README_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
season_pbp |>
  filter(str_detect(desc, "J.Fields")) |>
  filter(down == 3 | down == 4) |>
  mutate(first = ifelse(yards_gained >= ydstogo, 1, 0)) |>
  summarise(avg_ytg = round(mean(ydstogo), 1),
            avg_yds = round(mean(yards_gained), 1),
            pct = round(mean(first) * 100, 2))
```

    ## # A tibble: 1 × 3
    ##   avg_ytg avg_yds   pct
    ##     <dbl>   <dbl> <dbl>
    ## 1     7.6     5.6  38.7

``` r
# ydstogo, yards_gained, shotgun, qb_dropback, qb_scramble, pass_length, pass_location, air_yards, yards_after_catch
```

``` r
full_predict = function(home, away) {
  winner = predict_game_winner(home_team = home, away_team = away)
  prob = (predict_winner_prob(home_team = home, away_team = away))[[1]]
  return(paste0(winner, " (", prob, ")"))
}

# wild card weekend
full_predict(home = "BUF", away = "PIT")
```

    ## [1] "BUF (0.816)"

``` r
full_predict(home = "KC", away = "MIA")
```

    ## [1] "KC (0.565)"

``` r
full_predict(home = "HOU", away = "CLE")
```

    ## [1] "HOU (0.531)"

``` r
full_predict(home = "DAL", away = "GB")
```

    ## [1] "DAL (0.812)"

``` r
full_predict(home = "DET", away = "LA")
```

    ## [1] "DET (0.62)"

``` r
full_predict(home = "TB", away = "PHI")
```

    ## [1] "PHI (0.504)"

``` r
# division semis
full_predict(home = "BUF", away = "KC")
```

    ## [1] "BUF (0.54)"

``` r
full_predict(home = "BAL", away = "HOU")
```

    ## [1] "BAL (0.839)"

``` r
full_predict(home = "SF", away = "PHI")
```

    ## [1] "SF (0.793)"

``` r
full_predict(home = "DAL", away = "DET")
```

    ## [1] "DAL (0.775)"

``` r
# division championships
full_predict(home = "BAL", away = "BUF")
```

    ## [1] "BAL (0.683)"

``` r
full_predict(home = "SF", away = "DAL")
```

    ## [1] "SF (0.687)"

``` r
# super bowl
p1 = predict_winner_prob(home_team = "BAL", away_team = "SF")[[1]]
p2 = predict_winner_prob(home_team = "SF", away_team = "BAL")[[1]]
sprintf("super bowl champs: BAL (%s)", (p1 + (1 - p2)) / 2)
```

    ## [1] "super bowl champs: BAL (0.518)"

### new work

``` r
nflfastR::load_pbp(season = 2023)
```

    ## ── nflverse play by play data ──────────────────────────────────────────────────

    ## ℹ Data updated: 2024-01-11 04:06:25 EST

    ## # A tibble: 47,399 × 372
    ##    play_id game_id     old_game_id home_team away_team season_type  week posteam
    ##      <dbl> <chr>       <chr>       <chr>     <chr>     <chr>       <int> <chr>  
    ##  1       1 2023_01_AR… 2023091007  WAS       ARI       REG             1 <NA>   
    ##  2      39 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  3      55 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  4      77 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  5     102 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  6     124 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  7     147 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  8     172 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ##  9     197 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ## 10     220 2023_01_AR… 2023091007  WAS       ARI       REG             1 WAS    
    ## # ℹ 47,389 more rows
    ## # ℹ 364 more variables: posteam_type <chr>, defteam <chr>, side_of_field <chr>,
    ## #   yardline_100 <dbl>, game_date <chr>, quarter_seconds_remaining <dbl>,
    ## #   half_seconds_remaining <dbl>, game_seconds_remaining <dbl>,
    ## #   game_half <chr>, quarter_end <dbl>, drive <dbl>, sp <dbl>, qtr <dbl>,
    ## #   down <dbl>, goal_to_go <dbl>, time <chr>, yrdln <chr>, ydstogo <dbl>,
    ## #   ydsnet <dbl>, desc <chr>, play_type <chr>, yards_gained <dbl>, …
