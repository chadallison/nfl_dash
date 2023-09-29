
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

1.  MIA: 6.445
2.  BUF: 4.945
3.  DET: 4.584
4.  SF: 4.555
5.  DAL: 4.167

##### Five Worst Total CPRs

1.  DEN: -8.89
2.  CHI: -7.612
3.  LV: -5.944
4.  CAR: -4.334
5.  WAS: -4.222

------------------------------------------------------------------------

### Modeling

First draft basic logistic regression accuracy: 79.59%

##### *This Week’s Predictions*

- MIA @ BUF: MIA def. BUF (0.999)
- SEA @ NYG: SEA def. NYG (0.983)
- BAL @ CLE: CLE def. BAL (0.926)
- NE @ DAL: DAL def. NE (0.92)
- WAS @ PHI: PHI def. WAS (0.901)
- ARI @ SF: SF def. ARI (0.876)
- DET @ GB: DET def. GB (0.841)
- LA @ IND: LA def. IND (0.823)
- MIN @ CAR: MIN def. CAR (0.817)
- LV @ LAC: LV def. LAC (0.812)
- KC @ NYJ: NYJ def. KC (0.811)
- TB @ NO: NO def. TB (0.752)
- DEN @ CHI: DEN def. CHI (0.678)
- PIT @ HOU: PIT def. HOU (0.627)
- ATL @ JAX: ATL def. JAX (0.621)
- CIN @ TEN: CIN def. TEN (0.595)

``` r
# Install and load the XGBoost package
# install.packages("xgboost")
library(xgboost)
```

    ## 
    ## Attaching package: 'xgboost'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     slice

``` r
# Prepare your data
dtrain <- xgb.DMatrix(data = as.matrix(modeling_df[, c("home_pam", "home_avg_margin", "home_ppg",
                                                       "home_papg", "away_pam", "away_avg_margin",
                                                       "away_ppg", "away_papg")]),
                      label = modeling_df$home_win)

# Define hyperparameters
params <- list(
  objective = "binary:logistic",  # Binary classification
  eval_metric = "logloss"         # Log loss as evaluation metric
)

# Train the XGBoost model
xgb_model <- xgboost(data = dtrain, params = params, nrounds = 100)
```

    ## [1]  train-logloss:0.595140 
    ## [2]  train-logloss:0.527997 
    ## [3]  train-logloss:0.478223 
    ## [4]  train-logloss:0.442584 
    ## [5]  train-logloss:0.394084 
    ## [6]  train-logloss:0.366369 
    ## [7]  train-logloss:0.334151 
    ## [8]  train-logloss:0.311424 
    ## [9]  train-logloss:0.290791 
    ## [10] train-logloss:0.267327 
    ## [11] train-logloss:0.254810 
    ## [12] train-logloss:0.240277 
    ## [13] train-logloss:0.228834 
    ## [14] train-logloss:0.222088 
    ## [15] train-logloss:0.215528 
    ## [16] train-logloss:0.208158 
    ## [17] train-logloss:0.203106 
    ## [18] train-logloss:0.198241 
    ## [19] train-logloss:0.191900 
    ## [20] train-logloss:0.186881 
    ## [21] train-logloss:0.182343 
    ## [22] train-logloss:0.177625 
    ## [23] train-logloss:0.174070 
    ## [24] train-logloss:0.170138 
    ## [25] train-logloss:0.166451 
    ## [26] train-logloss:0.164102 
    ## [27] train-logloss:0.161881 
    ## [28] train-logloss:0.159064 
    ## [29] train-logloss:0.157237 
    ## [30] train-logloss:0.155359 
    ## [31] train-logloss:0.152816 
    ## [32] train-logloss:0.151519 
    ## [33] train-logloss:0.149830 
    ## [34] train-logloss:0.148214 
    ## [35] train-logloss:0.145879 
    ## [36] train-logloss:0.143067 
    ## [37] train-logloss:0.140559 
    ## [38] train-logloss:0.139295 
    ## [39] train-logloss:0.137471 
    ## [40] train-logloss:0.136126 
    ## [41] train-logloss:0.135060 
    ## [42] train-logloss:0.133819 
    ## [43] train-logloss:0.131149 
    ## [44] train-logloss:0.130121 
    ## [45] train-logloss:0.128198 
    ## [46] train-logloss:0.126251 
    ## [47] train-logloss:0.125147 
    ## [48] train-logloss:0.122783 
    ## [49] train-logloss:0.121585 
    ## [50] train-logloss:0.120583 
    ## [51] train-logloss:0.119781 
    ## [52] train-logloss:0.117984 
    ## [53] train-logloss:0.116818 
    ## [54] train-logloss:0.115915 
    ## [55] train-logloss:0.115273 
    ## [56] train-logloss:0.114736 
    ## [57] train-logloss:0.112882 
    ## [58] train-logloss:0.112030 
    ## [59] train-logloss:0.111246 
    ## [60] train-logloss:0.110431 
    ## [61] train-logloss:0.109827 
    ## [62] train-logloss:0.109184 
    ## [63] train-logloss:0.108353 
    ## [64] train-logloss:0.107727 
    ## [65] train-logloss:0.107204 
    ## [66] train-logloss:0.106700 
    ## [67] train-logloss:0.106184 
    ## [68] train-logloss:0.105548 
    ## [69] train-logloss:0.105063 
    ## [70] train-logloss:0.103388 
    ## [71] train-logloss:0.102800 
    ## [72] train-logloss:0.101455 
    ## [73] train-logloss:0.100829 
    ## [74] train-logloss:0.100330 
    ## [75] train-logloss:0.099117 
    ## [76] train-logloss:0.098748 
    ## [77] train-logloss:0.098169 
    ## [78] train-logloss:0.097741 
    ## [79] train-logloss:0.097433 
    ## [80] train-logloss:0.096966 
    ## [81] train-logloss:0.096705 
    ## [82] train-logloss:0.096539 
    ## [83] train-logloss:0.096330 
    ## [84] train-logloss:0.095924 
    ## [85] train-logloss:0.095733 
    ## [86] train-logloss:0.095600 
    ## [87] train-logloss:0.095434 
    ## [88] train-logloss:0.095303 
    ## [89] train-logloss:0.095152 
    ## [90] train-logloss:0.095025 
    ## [91] train-logloss:0.094886 
    ## [92] train-logloss:0.094764 
    ## [93] train-logloss:0.094390 
    ## [94] train-logloss:0.094240 
    ## [95] train-logloss:0.094107 
    ## [96] train-logloss:0.093871 
    ## [97] train-logloss:0.093753 
    ## [98] train-logloss:0.093629 
    ## [99] train-logloss:0.093426 
    ## [100]    train-logloss:0.093001

``` r
# Make predictions
# Assuming you have a new dataset called new_data
dtest <- xgb.DMatrix(data = as.matrix(modeling_df[, c("home_pam", "home_avg_margin", "home_ppg", "home_papg", "away_pam", "away_avg_margin", "away_ppg", "away_papg")]))
predictions <- predict(xgb_model, dtest)

# Evaluate the model as needed
modeling_df |>
  select(home_win) |>
  mutate(pred_prob = predictions,
         pred_hw = ifelse(pred_prob >= 0.5, 1, 0)) |>
  count(home_win == pred_hw)
```

    ## # A tibble: 1 × 2
    ##   `home_win == pred_hw`     n
    ##   <lgl>                 <int>
    ## 1 TRUE                     49
