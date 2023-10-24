
##### *Formatting is off right now. Will be adjusting soon :)*

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

1.  SF: 7.216
2.  BAL: 5.113
3.  KC: 4.44
4.  BUF: 3.765
5.  JAX: 3.365

##### Five Worst Total CPRs

1.  CAR: -5.127
2.  DEN: -5.054
3.  WAS: -4.959
4.  LV: -4.909
5.  NYG: -3.965

------------------------------------------------------------------------

### Weekly QB CER

![](README_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

------------------------------------------------------------------------

### QB Air Yards v YAC

![](README_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

------------------------------------------------------------------------

### Modeling

First draft basic logistic regression accuracy: 70.48%

##### *This Week’s Predictions*

- KC @ DEN: KC def. DEN (0.978)
- HOU @ CAR: HOU def. CAR (0.962)
- CIN @ SF: SF def. CIN (0.96)
- BAL @ ARI: BAL def. ARI (0.938)
- NE @ MIA: MIA def. NE (0.873)
- PHI @ WAS: PHI def. WAS (0.829)
- TB @ BUF: BUF def. TB (0.81)
- LV @ DET: DET def. LV (0.799)
- LA @ DAL: DAL def. LA (0.735)
- CLE @ SEA: SEA def. CLE (0.699)
- NO @ IND: NO def. IND (0.697)
- ATL @ TEN: TEN def. ATL (0.671)
- NYJ @ NYG: NYG def. NYJ (0.565)
- JAX @ PIT: JAX def. PIT (0.564)
- MIN @ GB: GB def. MIN (0.542)
  <!-- - CHI @ LAC: CHI def. LAC (0.538) -->

``` r
# team pts scored each week slugs
end_games
```

    ## # A tibble: 105 × 13
    ##    game_id   date        week away_team away_score home_score home_team win_team
    ##    <chr>     <date>     <dbl> <chr>          <dbl>      <dbl> <chr>     <chr>   
    ##  1 2023_01_… 2023-09-10     1 ARI               16         20 WAS       WAS     
    ##  2 2023_01_… 2023-09-11     1 BUF               16         22 NYJ       NYJ     
    ##  3 2023_01_… 2023-09-10     1 CAR               10         24 ATL       ATL     
    ##  4 2023_01_… 2023-09-10     1 CIN                3         24 CLE       CLE     
    ##  5 2023_01_… 2023-09-10     1 DAL               40          0 NYG       DAL     
    ##  6 2023_01_… 2023-09-07     1 DET               21         20 KC        DET     
    ##  7 2023_01_… 2023-09-10     1 GB                38         20 CHI       GB      
    ##  8 2023_01_… 2023-09-10     1 HOU                9         25 BAL       BAL     
    ##  9 2023_01_… 2023-09-10     1 JAX               31         21 IND       JAX     
    ## 10 2023_01_… 2023-09-10     1 LA                30         13 SEA       LA      
    ## # ℹ 95 more rows
    ## # ℹ 5 more variables: win_score <dbl>, lose_team <chr>, lose_score <dbl>,
    ## #   game_margin <dbl>, total_points <dbl>

``` r
get_week_pts_scored = function(team, wk) {
  x = end_games |> filter((home_team == team | away_team == team) & week == wk)
  if (nrow(x) == 0) return(NA)
  if (x$home_team == team) score = x$home_score else score = x$away_score
  return(score)
}

weekly_scored = data.frame()

for (i in 1:length(all_teams)) {
  for (j in 1:max(end_games$week)) {
    x = data.frame(team = all_teams[i], week = j)
    weekly_scored = rbind(weekly_scored, x)
  }
}

weekly_scored |>
  rowwise() |>
  mutate(week_scored = get_week_pts_scored(team, week)) |>
  ungroup() |>
  na.omit() |>
  mutate(total_scored = sapply(team, get_pts_scored),
         pct = week_scored / total_scored) |>
  ggplot(aes(week, pct)) +
  geom_line(aes(col = team), linewidth = 1.5, show.legend = F) +
  scale_color_manual(values = team_hex) +
  facet_wrap(vars(team), scales = "free_x", nrow = 5) +
  scale_y_continuous(breaks = seq(0, 1, by = 0.5), labels = scales::percent) +
  labs(x = "Week", y = "Percent of Total Points Scored",
       title = "Offensive Scoring Trends")
```

![](README_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

### Team Margins by Half

![](README_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->
