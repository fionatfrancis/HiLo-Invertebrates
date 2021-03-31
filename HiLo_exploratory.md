HiLo Exploratory Analysis
================
Fiona Francis
2/8/2021

This analysis is for Sharon Jeffrey and is part of an overall project
looking at the effect of current speed on fish and invertebrate
communities at different sites in the Straight of Georgia. Jillian
Campbell’s MSc analysis was also a component of this project.

Sharon is interested in using a mixed-effects model to look at how
different factors including site current speed, depth and substrate type
affect invert biomass and invert % cover.

The study uses 14 sites, 7 “high” current sites and 7 “low” current
sites. At each site Sharon collected invert data in 9 quadrats that are
seperated into 3 depths (10, 30 and 50 ft depth). There is no repeat
sampling so there are a total of 126 data points (14sites\*9quadrats).
However, because there are multiple collections at each site, a mixed
effects model is necessary to account for replciation at the site level.
I don’t think that we need to nest transect within site because I think
that each transect was at 1 depth (need to confirm this with Sharon) and
so depth is already being included as a fixed effect and accounting for
this replication (We need to worry about transect if they were run
perpendicular to shore (i.e. from 10 to 50 m)).

For substrate, substrate 1 is \>60% of quadrat and the codes are as
follows: Substrate Codes: 1=Bedrock Smooth, 2=Bedrock w crevices,
3=Boulders, 4=Cobble, 5=Gravel, 6=Pea Gravel, 7=Sand, 9=Mud,
0=Wood/Bark, 10=Crushed Shell, 11=Whole/Chunk Shell

## Looking at response data distributions

Biomass

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

Log10 Biomass. Nice and normal looking
![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Species Richness - textbook normal looking :)
![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Data visualization for biomass

Hi vs Lo

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

Hi vs Lo (Log10 Biomass)

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Biomass at each quad depth overall

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

Biomass at quad depth by site

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

Biomass at quad depth by site coloured by Hi Lo

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

## Plots of biomass vs current speed

Biomass vs current speed coloured by hi lo

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Log10Biomass vs current speed coloured by hi lo

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

Biomass vs current speed coloured by quadrat

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Biomass vs rock cover

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

Biomass vs Slope

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

Plot of fixed effects against each other

Avg rock and slope are at the site level
![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-15-2.png)<!-- -->![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-15-3.png)<!-- -->
\#\# mixed-effect models

``` r
head(all.data)
```

    ## # A tibble: 6 x 25
    ##   Date  Site  HiLo  Transect Quadrat SpRichness_quad~ QuadName Diver_invert
    ##   <chr> <chr> <chr> <chr>    <chr>              <dbl> <chr>    <chr>       
    ## 1 10-J~ Anni~ Lo    T1       Q10                   11 Anniver~ sarah       
    ## 2 10-J~ Anni~ Lo    T1       Q30                   14 Anniver~ sarah       
    ## 3 10-J~ Anni~ Lo    T1       Q50                   22 Anniver~ sarah       
    ## 4 10-J~ Anni~ Lo    T2       Q10                    5 Anniver~ sarah       
    ## 5 10-J~ Anni~ Lo    T2       Q30                   12 Anniver~ sarah       
    ## 6 10-J~ Anni~ Lo    T2       Q50                   17 Anniver~ sarah       
    ## # ... with 17 more variables: Diver_algae <chr>, `Invert%` <dbl>,
    ## #   Invert_Biomass <dbl>, DominantSp_1 <chr>, DominantSp_2 <chr>,
    ## #   substrate_1 <dbl>, substrate_1cov <dbl>, substrate2 <dbl>,
    ## #   substrate2cov <dbl>, substrate3 <dbl>, substrate3cov <dbl>, Comments <chr>,
    ## #   site_length_m <dbl>, SlopeAngle <dbl>, AvgDailMaxCurr <dbl>,
    ## #   InvertSpRichness <dbl>, AvgRkCov <dbl>

``` r
glimpse(all.data)
```

    ## Rows: 126
    ## Columns: 25
    ## $ Date               <chr> "10-Jul", "10-Jul", "10-Jul", "10-Jul", "10-Jul"...
    ## $ Site               <chr> "Anniversary_Is", "Anniversary_Is", "Anniversary...
    ## $ HiLo               <chr> "Lo", "Lo", "Lo", "Lo", "Lo", "Lo", "Lo", "Lo", ...
    ## $ Transect           <chr> "T1", "T1", "T1", "T2", "T2", "T2", "T3", "T3", ...
    ## $ Quadrat            <chr> "Q10", "Q30", "Q50", "Q10", "Q30", "Q50", "Q10",...
    ## $ SpRichness_quadrat <dbl> 11, 14, 22, 5, 12, 17, 11, 18, 8, 25, 29, 23, 16...
    ## $ QuadName           <chr> "Anniversary_Is_T1Q10", "Anniversary_Is_T1Q30", ...
    ## $ Diver_invert       <chr> "sarah", "sarah", "sarah", "sarah", "sarah", "sa...
    ## $ Diver_algae        <chr> "sharon", "sharon", "sharon", "sharon", "sharon"...
    ## $ `Invert%`          <dbl> 70, 10, 80, 70, 30, 5, 20, 10, 15, 20, 35, 25, 4...
    ## $ Invert_Biomass     <dbl> 3500, 100, 4000, 7000, 3000, 500, 200, 30100, 15...
    ## $ DominantSp_1       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ DominantSp_2       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ substrate_1        <dbl> 1, 1, 3, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, ...
    ## $ substrate_1cov     <dbl> 100, 100, 100, 100, 100, 100, 90, 80, 90, 100, 1...
    ## $ substrate2         <dbl> NA, NA, NA, NA, NA, NA, 2, 2, 2, NA, NA, NA, NA,...
    ## $ substrate2cov      <dbl> NA, NA, NA, NA, NA, NA, 10, 20, 10, NA, NA, NA, ...
    ## $ substrate3         <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ substrate3cov      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ Comments           <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ site_length_m      <dbl> 21.286, 21.286, 21.286, 21.286, 21.286, 21.286, ...
    ## $ SlopeAngle         <dbl> 29.7, 29.7, 29.7, 29.7, 29.7, 29.7, 29.7, 29.7, ...
    ## $ AvgDailMaxCurr     <dbl> 22.5, 22.5, 22.5, 22.5, 22.5, 22.5, 22.5, 22.5, ...
    ## $ InvertSpRichness   <dbl> 13.1, 13.1, 13.1, 13.1, 13.1, 13.1, 13.1, 13.1, ...
    ## $ AvgRkCov           <dbl> 100.0, 100.0, 100.0, 100.0, 100.0, 100.0, 100.0,...

``` r
# using max current speed, fitting a random effect for site
# need to specify using ML instead of REML. Here is the logic from talking to Dave Iles a statistician at Environment Canada. "I think this is happening because restricted maximum likelihood (REML) is being used to fit the models, rather than ML.  Annoyingly, REML is the default method for fitting mixed-effect models in both the lme4 and nlme packages - and ML needs to be explicitly specified to fit and compare models with the same random effect structure but different fixed effects."

#single variable
Null <- lme(log10(Invert_Biomass) ~ 1, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm1 <-  lme(log10(Invert_Biomass) ~ HiLo, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm2  <- lme(log10(Invert_Biomass) ~ Quadrat, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm3 <- lme(log10(Invert_Biomass) ~ SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm4 <-  lme(log10(Invert_Biomass) ~ AvgRkCov,
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm5 <-  lme(log10(Invert_Biomass) ~ AvgDailMaxCurr, 
            random = ~ 1 | Site, data = all.data, method = "ML")


#two variables
Lm6 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm7 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm8 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm9 <- lme(log10(Invert_Biomass) ~ Quadrat + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm10 <- lme(log10(Invert_Biomass) ~ Quadrat + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm11 <-  lme(log10(Invert_Biomass) ~ SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

#three variables

Lm12 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm13 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm14 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm15 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm16 <- lme(log10(Invert_Biomass) ~ Quadrat + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

# four variables

Lm17 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")


names <- c("Null", "Current", "Depth", "Slope", "Rock", "ContCurrent")

AICtab(Null, Lm1, Lm2, Lm3, Lm4, Lm5, Lm6, Lm7, Lm8,Lm9, Lm10, Lm11, Lm12, Lm13, Lm14, Lm15, Lm16, Lm17, base=TRUE, weights=TRUE, logLik=TRUE)
```

    ##      logLik AIC    dLogLik dAIC   df weight
    ## Lm13 -114.0  242.0    8.9     0.0 7  0.2427
    ## Lm7  -116.2  242.3    6.7     0.3 5  0.2112
    ## Lm17 -113.4  242.8    9.5     0.8 8  0.1627
    ## Lm15 -115.6  243.1    7.3     1.1 6  0.1416
    ## Lm6  -116.5  244.9    6.4     2.9 6  0.0567
    ## Lm12 -116.5  244.9    6.4     2.9 6  0.0567
    ## Lm5  -118.6  245.2    4.3     3.2 4  0.0493
    ## Lm14 -116.3  246.6    6.6     4.6 7  0.0247
    ## Lm8  -118.4  246.9    4.4     4.9 5  0.0215
    ## Lm9  -118.6  249.1    4.3     7.1 6  0.0070
    ## Lm3  -120.7  249.4    2.2     7.4 4  0.0061
    ## Lm1  -120.8  249.6    2.1     7.6 4  0.0055
    ## Lm16 -118.0  249.9    4.9     7.9 7  0.0046
    ## Lm11 -120.1  250.2    2.8     8.2 5  0.0040
    ## Lm2  -120.7  251.5    2.1     9.5 5  0.0021
    ## Null -122.9  251.8    0.0     9.7 3  0.0019
    ## Lm10 -120.6  253.2    2.3    11.2 6  <0.001
    ## Lm4  -122.8  253.5    0.1    11.5 4  <0.001

``` r
Lm13 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm7 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm17 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm15 <- lme(log10(Invert_Biomass) ~ AvgDailMaxCurr + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")
```

So there are several models within 2 delta AIC units but we can see that
current and depth are in all of them so these are clearly important
variables. Let’s take a closer look at the diagnostics of the top model:

    ## Linear mixed-effects model fit by maximum likelihood
    ##   Data: all.data 
    ##   Log-likelihood: -114.0113
    ##   Fixed: log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + SlopeAngle 
    ##    (Intercept) AvgDailMaxCurr     QuadratQ30     QuadratQ50     SlopeAngle 
    ##    2.466370521    0.009673933    0.189729504    0.237886839    0.024794172 
    ## 
    ## Random effects:
    ##  Formula: ~1 | Site
    ##         (Intercept)  Residual
    ## StdDev:   0.3308808 0.5519688
    ## 
    ## Number of Observations: 126
    ## Number of Groups: 14

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-17-2.png)<!-- -->

Plotting predictors seperately from Lm13 (current, depth, slope)

    ## Linear mixed-effects model fit by maximum likelihood
    ##  Data: all.data 
    ##        AIC      BIC    logLik
    ##   242.0225 261.8765 -114.0113
    ## 
    ## Random effects:
    ##  Formula: ~1 | Site
    ##         (Intercept)  Residual
    ## StdDev:   0.3308808 0.5519688
    ## 
    ## Fixed effects: log10(Invert_Biomass) ~ AvgDailMaxCurr + Quadrat + SlopeAngle 
    ##                    Value  Std.Error  DF  t-value p-value
    ## (Intercept)    2.4663705 0.25556811 110 9.650541  0.0000
    ## AvgDailMaxCurr 0.0096739 0.00275885  11 3.506509  0.0049
    ## QuadratQ30     0.1897295 0.12291290 110 1.543609  0.1256
    ## QuadratQ50     0.2378868 0.12291290 110 1.935410  0.0555
    ## SlopeAngle     0.0247942 0.01043587  11 2.375860  0.0368
    ##  Correlation: 
    ##                (Intr) AvgDMC QdrQ30 QdrQ50
    ## AvgDailMaxCurr -0.461                     
    ## QuadratQ30     -0.240  0.000              
    ## QuadratQ50     -0.240  0.000  0.500       
    ## SlopeAngle     -0.642 -0.184  0.000  0.000
    ## 
    ## Standardized Within-Group Residuals:
    ##        Min         Q1        Med         Q3        Max 
    ## -2.8945086 -0.5479547  0.0612298  0.5896900  2.3982479 
    ## 
    ## Number of Observations: 126
    ## Number of Groups: 14

Current speed
![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

Depth
![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->
Slope
![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

# Invertebrate Percent Cover

## Same plots but looking at % invert cover instead of biomass

![](HiLo_exploratory_files/figure-gfm/cover%20ploting-1.png)<!-- -->![](HiLo_exploratory_files/figure-gfm/cover%20ploting-2.png)<!-- -->![](HiLo_exploratory_files/figure-gfm/cover%20ploting-3.png)<!-- -->![](HiLo_exploratory_files/figure-gfm/cover%20ploting-4.png)<!-- -->

Richness vs current

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

Richness vs current speed

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

Richness vs rock cover

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

Richness vs Slope

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

# Preliminary models for species richness

``` r
head(all.data)
```

    ## # A tibble: 6 x 25
    ##   Date  Site  HiLo  Transect Quadrat SpRichness_quad~ QuadName Diver_invert
    ##   <chr> <chr> <chr> <chr>    <chr>              <dbl> <chr>    <chr>       
    ## 1 10-J~ Anni~ Lo    T1       Q10                   11 Anniver~ sarah       
    ## 2 10-J~ Anni~ Lo    T1       Q30                   14 Anniver~ sarah       
    ## 3 10-J~ Anni~ Lo    T1       Q50                   22 Anniver~ sarah       
    ## 4 10-J~ Anni~ Lo    T2       Q10                    5 Anniver~ sarah       
    ## 5 10-J~ Anni~ Lo    T2       Q30                   12 Anniver~ sarah       
    ## 6 10-J~ Anni~ Lo    T2       Q50                   17 Anniver~ sarah       
    ## # ... with 17 more variables: Diver_algae <chr>, `Invert%` <dbl>,
    ## #   Invert_Biomass <dbl>, DominantSp_1 <chr>, DominantSp_2 <chr>,
    ## #   substrate_1 <dbl>, substrate_1cov <dbl>, substrate2 <dbl>,
    ## #   substrate2cov <dbl>, substrate3 <dbl>, substrate3cov <dbl>, Comments <chr>,
    ## #   site_length_m <dbl>, SlopeAngle <dbl>, AvgDailMaxCurr <dbl>,
    ## #   InvertSpRichness <dbl>, AvgRkCov <dbl>

``` r
glimpse(all.data)
```

    ## Rows: 126
    ## Columns: 25
    ## $ Date               <chr> "10-Jul", "10-Jul", "10-Jul", "10-Jul", "10-Jul"...
    ## $ Site               <chr> "Anniversary_Is", "Anniversary_Is", "Anniversary...
    ## $ HiLo               <chr> "Lo", "Lo", "Lo", "Lo", "Lo", "Lo", "Lo", "Lo", ...
    ## $ Transect           <chr> "T1", "T1", "T1", "T2", "T2", "T2", "T3", "T3", ...
    ## $ Quadrat            <chr> "Q10", "Q30", "Q50", "Q10", "Q30", "Q50", "Q10",...
    ## $ SpRichness_quadrat <dbl> 11, 14, 22, 5, 12, 17, 11, 18, 8, 25, 29, 23, 16...
    ## $ QuadName           <chr> "Anniversary_Is_T1Q10", "Anniversary_Is_T1Q30", ...
    ## $ Diver_invert       <chr> "sarah", "sarah", "sarah", "sarah", "sarah", "sa...
    ## $ Diver_algae        <chr> "sharon", "sharon", "sharon", "sharon", "sharon"...
    ## $ `Invert%`          <dbl> 70, 10, 80, 70, 30, 5, 20, 10, 15, 20, 35, 25, 4...
    ## $ Invert_Biomass     <dbl> 3500, 100, 4000, 7000, 3000, 500, 200, 30100, 15...
    ## $ DominantSp_1       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ DominantSp_2       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ substrate_1        <dbl> 1, 1, 3, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, ...
    ## $ substrate_1cov     <dbl> 100, 100, 100, 100, 100, 100, 90, 80, 90, 100, 1...
    ## $ substrate2         <dbl> NA, NA, NA, NA, NA, NA, 2, 2, 2, NA, NA, NA, NA,...
    ## $ substrate2cov      <dbl> NA, NA, NA, NA, NA, NA, 10, 20, 10, NA, NA, NA, ...
    ## $ substrate3         <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ substrate3cov      <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ Comments           <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ site_length_m      <dbl> 21.286, 21.286, 21.286, 21.286, 21.286, 21.286, ...
    ## $ SlopeAngle         <dbl> 29.7, 29.7, 29.7, 29.7, 29.7, 29.7, 29.7, 29.7, ...
    ## $ AvgDailMaxCurr     <dbl> 22.5, 22.5, 22.5, 22.5, 22.5, 22.5, 22.5, 22.5, ...
    ## $ InvertSpRichness   <dbl> 13.1, 13.1, 13.1, 13.1, 13.1, 13.1, 13.1, 13.1, ...
    ## $ AvgRkCov           <dbl> 100.0, 100.0, 100.0, 100.0, 100.0, 100.0, 100.0,...

``` r
# using max current speed, fitting a random effect for site
# need to specify using ML instead of REML. Here is the logic from talking to Dave Iles a statistician at Environment Canada. "I think this is happening because restricted maximum likelihood (REML) is being used to fit the models, rather than ML.  Annoyingly, REML is the default method for fitting mixed-effect models in both the lme4 and nlme packages - and ML needs to be explicitly specified to fit and compare models with the same random effect structure but different fixed effects."

#single variable
Null.rich <- lme(SpRichness_quadrat ~ 1, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm1.rich <-  lme(SpRichness_quadrat ~ HiLo, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm2.rich  <- lme(SpRichness_quadrat ~ Quadrat, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm3.rich <- lme(SpRichness_quadrat ~ SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm4.rich <-  lme(SpRichness_quadrat ~ AvgRkCov,
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm5.rich <-  lme(SpRichness_quadrat ~ AvgDailMaxCurr, 
            random = ~ 1 | Site, data = all.data, method = "ML")


#two variables
Lm6.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + Quadrat, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm7.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm8.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm9.rich <- lme(SpRichness_quadrat ~ Quadrat + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm10.rich <- lme(SpRichness_quadrat ~ Quadrat + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm11.rich <-  lme(SpRichness_quadrat ~ SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

#three variables

Lm12.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + Quadrat, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm13.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + Quadrat + SlopeAngle, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm14.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + Quadrat + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm15.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

Lm16.rich <- lme(SpRichness_quadrat ~ Quadrat + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")

# four variables

Lm17.rich <- lme(SpRichness_quadrat ~ AvgDailMaxCurr + Quadrat + SlopeAngle + AvgRkCov, 
            random = ~ 1 | Site, data = all.data, method = "ML")


names <- c("Null", "Current", "Depth", "Slope", "Rock", "ContCurrent")

AICtab(Null.rich, Lm1.rich, Lm2.rich, Lm3.rich, Lm4.rich, Lm5.rich, Lm6.rich, Lm7.rich, Lm8.rich,Lm9.rich, Lm10.rich, Lm11.rich, Lm12.rich, Lm13.rich, Lm14.rich, Lm15.rich, Lm16.rich, Lm17.rich, base=TRUE, weights=TRUE, logLik=TRUE)
```

    ##           logLik AIC    dLogLik dAIC   df weight
    ## Lm14.rich -411.3  836.6   16.5     0.0 7  0.4083
    ## Lm17.rich -410.9  837.7   17.0     1.1 8  0.2363
    ## Lm13.rich -411.9  837.9   15.9     1.2 7  0.2197
    ## Lm6.rich  -414.1  840.3   13.7     3.7 6  0.0650
    ## Lm12.rich -414.1  840.3   13.7     3.7 6  0.0650
    ## Lm8.rich  -418.8  847.6    9.0    11.0 5  0.0017
    ## Lm15.rich -418.3  848.7    9.5    12.1 6  <0.001
    ## Lm7.rich  -419.4  848.8    8.4    12.2 5  <0.001
    ## Lm9.rich  -418.6  849.2    9.2    12.6 6  <0.001
    ## Lm10.rich -419.2  850.3    8.7    13.7 6  <0.001
    ## Lm2.rich  -420.3  850.7    7.5    14.1 5  <0.001
    ## Lm16.rich -418.4  850.9    9.4    14.3 7  <0.001
    ## Lm5.rich  -421.6  851.3    6.2    14.7 4  <0.001
    ## Lm1.rich  -425.3  858.6    2.5    22.0 4  <0.001
    ## Lm3.rich  -426.1  860.2    1.7    23.5 4  <0.001
    ## Lm4.rich  -426.7  861.3    1.2    24.7 4  <0.001
    ## Null.rich -427.8  861.7    0.0    25.0 3  <0.001
    ## Lm11.rich -425.9  861.9    1.9    25.2 5  <0.001

``` r
Lm14.rich
```

    ## Linear mixed-effects model fit by maximum likelihood
    ##   Data: all.data 
    ##   Log-likelihood: -411.3094
    ##   Fixed: SpRichness_quadrat ~ AvgDailMaxCurr + Quadrat + AvgRkCov 
    ##    (Intercept) AvgDailMaxCurr     QuadratQ30     QuadratQ50       AvgRkCov 
    ##   -19.66121870     0.09686269     2.30952381     5.38095238     0.33509377 
    ## 
    ## Random effects:
    ##  Formula: ~1 | Site
    ##         (Intercept) Residual
    ## StdDev:    1.517749 6.180085
    ## 
    ## Number of Observations: 126
    ## Number of Groups: 14

``` r
plot(Lm14.rich) # looks good
```

![](HiLo_exploratory_files/figure-gfm/sp%20richness-1.png)<!-- -->

``` r
qqnorm(resid(Lm14.rich)) # looks good
```

![](HiLo_exploratory_files/figure-gfm/sp%20richness-2.png)<!-- -->

Again there are several models within 2 AIC so it is very clear that all
of these factors are important in predicting species richness.

Plotting individual predictors from Lm14

Current

![](HiLo_exploratory_files/figure-gfm/current-1.png)<!-- -->

Depth

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

Rock Cover

![](HiLo_exploratory_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->
