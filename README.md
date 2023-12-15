
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

1.  SF: 6.488
2.  DAL: 5.419
3.  BAL: 4.988
4.  BUF: 2.964
5.  KC: 2.849

##### Five Worst Total CPRs

1.  WAS: -5.544
2.  CAR: -4.796
3.  NYG: -4.559
4.  NE: -3.556
5.  ARI: -3.349

------------------------------------------------------------------------

### Weekly QB CER

![](README_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

------------------------------------------------------------------------

### QB Air Yards v YAC

![](README_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

------------------------------------------------------------------------

### Modeling

First draft basic logistic regression accuracy: 66.35%

##### *This Week’s Predictions*

- NYJ @ MIA: MIA def. NYJ (0.896)
- NYG @ NO: NO def. NYG (0.821)
- SF @ ARI: SF def. ARI (0.819)
- KC @ NE: KC def. NE (0.755)
- ATL @ CAR: ATL def. CAR (0.742)
- PHI @ SEA: PHI def. SEA (0.718)
- PIT @ IND: IND def. PIT (0.709)
- DEN @ DET: DET def. DEN (0.645)
- CHI @ CLE: CLE def. CHI (0.611)
- TB @ GB: GB def. TB (0.602)
- WAS @ LA: LA def. WAS (0.598)
- BAL @ JAX: BAL def. JAX (0.571)
- MIN @ CIN: MIN def. CIN (0.57)
- LAC @ LV: LAC def. LV (0.566)
  <!-- - DAL @ BUF: BUF def. DAL (0.561) -->
  <!-- - HOU @ TEN: HOU def. TEN (0.553) -->

![](README_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

### Team Margins by Half

![](README_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

    ##    team            class
    ## 1   ARI       always bad
    ## 2   ATL second half team
    ## 3   BAL      always good
    ## 4   BUF      always good
    ## 5   CAR       always bad
    ## 6   CHI       always bad
    ## 7   CIN second half team
    ## 8   CLE      always good
    ## 9   DAL      always good
    ## 10  DEN       always bad
    ## 11  DET  first half team
    ## 12   GB second half team
    ## 13  HOU  first half team
    ## 14  IND       always bad
    ## 15  JAX  first half team
    ## 16   KC  first half team
    ## 17   LA second half team
    ## 18  LAC  first half team
    ## 19   LV       always bad
    ## 20  MIA      always good
    ## 21  MIN  first half team
    ## 22   NE       always bad
    ## 23   NO second half team
    ## 24  NYG       always bad
    ## 25  NYJ second half team
    ## 26  PHI second half team
    ## 27  PIT second half team
    ## 28  SEA       always bad
    ## 29   SF      always good
    ## 30   TB       always bad
    ## 31  TEN  first half team
    ## 32  WAS       always bad

``` r
data.frame(team = all_teams) |>
  mutate(off_cpr = sapply(team, get_off_cpr),
         def_cpr = sapply(team, get_def_cpr)) |>
  ggplot(aes(off_cpr, def_cpr)) +
  geom_point(aes(col = team), size = 5, shape = "square", show.legend = F) +
  ggrepel::geom_text_repel(aes(label = team), size = 3.5, max.overlaps = 32) +
  scale_color_manual(values = team_hex) +
  geom_hline(yintercept = 0, linetype = "dashed", alpha = 0.25) +
  geom_vline(xintercept = 0, linetype = "dashed", alpha = 0.25)
```

![](README_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

``` r
data.frame(team = all_teams) |>
  mutate(off_cpr = sapply(team, get_off_cpr),
         def_cpr = sapply(team, get_def_cpr),
         class = case_when(off_cpr > 0 & def_cpr > 0 ~ "good overall",
                           off_cpr > 0 & def_cpr <= 0 ~ "good offense, bad defense",
                           off_cpr <= 0 & def_cpr > 0 ~ "bad offense, good defense",
                           off_cpr <= 0 & def_cpr <= 0 ~ "bad overall")) |>
  count(class)
```

    ##                       class  n
    ## 1 bad offense, good defense  8
    ## 2               bad overall  8
    ## 3 good offense, bad defense  6
    ## 4              good overall 10
