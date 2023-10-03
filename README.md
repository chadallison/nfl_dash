
### Contents

- [Team Standings](#team-standings)
- [Offensive and Defensive PPG](#offensive-and-defensive-ppg)
- [Offensive and Defensive YPG](#offensive-and-defensive-ypg)
- [Team Margins](#team-margins)
- [Point-Adjusted Margins](#point-adjusted-margins)
- [Quarter-Based Scoring Trends](#quarter-based-scoring-trends)
- [Offensive and Defensive CPR](#offensive-and-defensive-cpr)
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

1.  BUF: 8.594
2.  DAL: 5.563
3.  SF: 4.782
4.  BAL: 4.688
5.  DET: 4.437

##### Five Worst Total CPRs

1.  DEN: -9.406
2.  CHI: -7.406
3.  CAR: -4.813
4.  LV: -4.812
5.  NYG: -4.625

------------------------------------------------------------------------

### Modeling

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `pred_prob = round(...)`.
    ## Caused by warning in `predict.lm()`:
    ## ! prediction from rank-deficient fit; attr(*, "non-estim") has doubtful cases

First draft basic logistic regression accuracy: 78.12%

##### *This Week’s Predictions*

    ## Warning: There were 32 warnings in `mutate()`.
    ## The first warning was:
    ## ℹ In argument: `pred_winner = predict_game_winner(home_team, away_team)`.
    ## ℹ In row 1.
    ## Caused by warning in `predict.lm()`:
    ## ! prediction from rank-deficient fit; attr(*, "non-estim") has doubtful cases
    ## ℹ Run `dplyr::last_dplyr_warnings()` to see the 31 remaining warnings.

- DEN @ CHI: DEN def. CHI (0.988)
- SEA @ NYG: SEA def. NYG (0.966)
- NE @ DAL: DAL def. NE (0.954)
- ARI @ SF: SF def. ARI (0.94)
- MIA @ BUF: BUF def. MIA (0.832)
- WAS @ PHI: PHI def. WAS (0.816)
- MIN @ CAR: MIN def. CAR (0.788)
- PIT @ HOU: HOU def. PIT (0.774)
- CIN @ TEN: TEN def. CIN (0.75)
- DET @ GB: DET def. GB (0.725)
- LA @ IND: LA def. IND (0.725)
- LV @ LAC: LAC def. LV (0.651)
- ATL @ JAX: JAX def. ATL (0.635)
- KC @ NYJ: KC def. NYJ (0.617)
- BAL @ CLE: BAL def. CLE (0.597)
- TB @ NO: NO def. TB (0.595)

``` r
season_pbp |>
  filter(!is.na(air_yards)) |>
  mutate(is_complete = ifelse(is.na(yards_after_catch), 0, 1)) |>
  select(passer, air_yards, is_complete) |>
  group_by(passer) |>
  summarise(n = n(),
            total_air = sum(air_yards),
            avg_air = mean(air_yards),
            comp_rate = mean(is_complete)) |>
  filter(n >= 50) |>
  transmute(passer, swag_rating = round(avg_air / (1 - comp_rate), 3)) |>
  arrange(desc(swag_rating))
```

    ## # A tibble: 34 × 2
    ##    passer       swag_rating
    ##    <chr>              <dbl>
    ##  1 J.Allen             30.9
    ##  2 J.Herbert           30.5
    ##  3 J.Hurts             30.5
    ##  4 T.Tagovailoa        28.3
    ##  5 J.Garoppolo         27.6
    ##  6 D.Watson            26.9
    ##  7 B.Purdy             26.8
    ##  8 J.Dobbs             26.8
    ##  9 L.Jackson           26.7
    ## 10 R.Tannehill         26.4
    ## # ℹ 24 more rows
