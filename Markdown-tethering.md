HOKN Data, Menti et al. 2024
================

## Requirements

The code was written and tested with the following Rstudio version:

*RStudio 2023.12.1+402 “Ocean Storm” Release
(4da58325ffcff29d157d9264087d4b1ab27f7204, 2024-01-28) for windows
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML,
like Gecko) RStudio/2023.12.1+402 Chrome/116.0.5845.190 Electron/26.2.4
Safari/537.36.*

Before starting the markdown document please read ahead and install the
required R packages. I would advice installing the missing packages by
yourself if you already have a working R installation. Otherwise, it is
also possible to un-comment lines 3-6 in the next chunk of code to
automatically install all the required packages and their requirements.

The following libraries will be loaded:

- car;

- datarium;

- dplyr;

- dunn.test;

- ggExtra;

- ggplot2;

- ggpubr;

- lattice;

- MASS;

- multcomp;

- perm;

- PMCMRplus;

- scales;

- reshape2;

- rstatix;

- rstudioapi;

- stats;

- tidyverse;

- trajr.

***! Important***: please also download and open the provided R.data
files in your environment, as they contain the pre-generated, and
required, data sets.

## Introduction

In this markdown we are analyzing the HOKN events, extracted from data
obtained during tethered flight experiments, by mean of velocity of the
slow phase of the HOKN, the number of peaks, and the time passing from
consecutive peaks or inter-saccadic-intervals (ISI). The intervals are
intended as the time passing from one HOKN event to the subsequent one;
there’s no value for the intervals at T0, because we can’t subtract the
time of the very first HOKN from 0, when the experiment starts.

## Analysis from the provided data sets.

**!** **Remember to load the provided R.data files in your
environment**.

Notes on loaded data sets:

1.  ‘group_full’ is the same from the original ‘group’ except the first
    line of NaN values is ditched from both the NOSTIM and STIM rows;

2.  ‘group_combined’ is a merged DF of all events without phase
    distinction; this is done only for Controls since after testing no
    meaningful difference was found between the distributions of
    interevent time; the ‘\_new’ DF include the group label;

3.  ‘group_stim/nostim’ are DFs splitted from (1). The ‘\_new’ DFs
    subtitue the ‘stim’ column now redundant with group label;

4.  the main dataframe recalled, rearranged, or modified along the code
    chunks is named FDS_ALL.

### Slow phase velocity

#### Plots

Discrete plots for the slow phase velocity of the HOKN in each group.

![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-2.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-3.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-4.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-5.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-6.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-7.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-8.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplots%20mediated%20per%20fly%20number-9.png)<!-- -->

#### Statistics

Performs a normality check on data divided between MOP and RP plus a
check for heteroscedasticity, then runs the statistical test (parametric
as for our data) eventually corrected for heteroscedasticity.

    ## [1] "controls"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.95689, p-value = 8.527e-13
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.9526, p-value = 4.905e-14
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 1.6237, df = 1, p-value = 0.2026
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 13, p-value = 0.1875
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "eugenol"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.98069, p-value = 5.375e-13
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.98776, p-value = 1.116e-08
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 1.801, df = 1, p-value = 0.1796
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 31, p-value = 0.07813
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "eugenol05"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.96165, p-value = 2.44e-14
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.95501, p-value = 5.963e-14
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 0.4716, df = 1, p-value = 0.4922
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 10, p-value = 0.3125
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "lemongrass"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.97644, p-value = 3.274e-13
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.98444, p-value = 3.816e-09
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 0.056708, df = 1, p-value = 0.8118
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 43, p-value = 0.01172
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "lemongrass05"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.98756, p-value = 1.667e-07
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.98848, p-value = 1.13e-06
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 4.93, df = 1, p-value = 0.02639
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 28, p-value = 0.1953
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "picaridin"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.95555, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.96966, p-value = 3.455e-13
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 5.3609, df = 1, p-value = 0.02059
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 27, p-value = 0.6523
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "picaridin05"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.97829, p-value = 3.268e-10
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.97412, p-value = 1.025e-09
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 3.4963, df = 1, p-value = 0.06151
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 11, p-value = 0.6875
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "ir3535"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.98492, p-value = 1.64e-06
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.98877, p-value = 4.027e-05
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 2.0054, df = 1, p-value = 0.1567
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 24, p-value = 0.4609
    ## alternative hypothesis: true location shift is not equal to 0
    ## 
    ## [1] "ir353505"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "nostim"]
    ## W = 0.9695, p-value = 3.973e-13
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j & ALL_SLOWSPVEL$phase == "stim"]
    ## W = 0.96812, p-value = 4.428e-12
    ## 
    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  ALL_SLOWSPVEL$spvel[ALL_SLOWSPVEL$group == j] by ALL_SLOWSPVEL$phase[ALL_SLOWSPVEL$group == j]
    ## Bartlett's K-squared = 0.0053712, df = 1, p-value = 0.9416
    ## 
    ## 
    ##  Wilcoxon signed rank exact test
    ## 
    ## data:  means_nostim and means_stim
    ## V = 11, p-value = 0.6875
    ## alternative hypothesis: true location shift is not equal to 0

### Peaks

#### Plots

This section of code will generate two plots:

1.  Average number of peaks per fly in each group, divided among MOP
    (dark grey) and RP (light grey);

2.  Percentage of RP peaks with respect to the corresponding MOP (taken
    as reference) in each group.

![](Markdown-tethering_files/figure-gfm/Barplots-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Barplots-2.png)<!-- -->

#### Statistics

The code will run automatically a test for equality of proportions
between each group. This will generate some redundant output due to how
the code is written. ***Please*** ***note*** that the interesting
comparisons are the ones including the “Control” group against the
others (first part of the automated output), and the comparison of the
two different concentrations inside each group. Alternatively, the code
can be easily changed running manually just the 2nd loop and modifying
‘\[y\]’ and ‘\[j\]’ to the desired values 1:9, which are related to the
group vector as defined in the “Groups” chunk above.

    ## [1] "controls"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 22.862, df = 1, p-value = 1.741e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03881661 0.09366666
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4427439 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 25.094, df = 1, p-value = 5.46e-07
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.05188994 0.11934886
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4233661 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 29.809, df = 1, p-value = 4.769e-08
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.04905651 0.10495577
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4319794 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 5.8631, df = 1, p-value = 0.01546
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.007545685 0.072500985
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4689622 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 20.394, df = 1, p-value = 6.304e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03926394 0.10035353
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4391768 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 27.405, df = 1, p-value = 1.65e-07
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.05410747 0.11964761
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4221080 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.6696, df = 1, p-value = 0.4132
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01948470  0.04858636
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4944347 
    ## 
    ## [1] "controls"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.5692, df = 1, p-value = 0.4506
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01840996  0.04239017
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.5089855 0.4969954 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 22.862, df = 1, p-value = 1.741e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.09366666 -0.03881661
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.5089855 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.9219, df = 1, p-value = 0.1657
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.007783579  0.046539101
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4233661 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.1357, df = 1, p-value = 0.2866
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.00886055  0.03038955
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4319794 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 4.0441, df = 1, p-value = 0.04433
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.0518258660 -0.0006107419
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4689622 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.07959, df = 1, p-value = 0.7779
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01956693  0.02670112
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4391768 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 2.3943, df = 1, p-value = 0.1218
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.005336537  0.046608336
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4221080 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 13.781, df = 1, p-value = 0.0002054
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.07922925 -0.02415238
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4944347 
    ## 
    ## [1] "eugenol"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 21.801, df = 1, p-value = 3.024e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.07719707 -0.03130600
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4427439 0.4969954 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 25.094, df = 1, p-value = 5.46e-07
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.11934886 -0.05188994
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.5089855 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.9219, df = 1, p-value = 0.1657
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.046539101  0.007783579
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4427439 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.3473, df = 1, p-value = 0.5556
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03630412  0.01907760
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4319794 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 7.7224, df = 1, p-value = 0.005454
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.07785070 -0.01334143
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4689622 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.013, df = 1, p-value = 0.3142
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.04611848  0.01449715
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4391768 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.0019009, df = 1, p-value = 0.9652
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03129085  0.03380713
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4221080 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 17.188, df = 1, p-value = 3.386e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.10489118 -0.03724597
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4944347 
    ## 
    ## [1] "eugenol05"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 23.066, df = 1, p-value = 1.566e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.1037913 -0.0434673
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4233661 0.4969954 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 29.809, df = 1, p-value = 4.769e-08
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.10495577 -0.04905651
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.5089855 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.1357, df = 1, p-value = 0.2866
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03038955  0.00886055
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4427439 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.3473, df = 1, p-value = 0.5556
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01907760  0.03630412
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4233661 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 7.7695, df = 1, p-value = 0.005314
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.06314992 -0.01081569
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4689622 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.33326, df = 1, p-value = 0.5637
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03094709  0.01655228
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4391768 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.50606, df = 1, p-value = 0.4768
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01665331  0.03639610
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4221080 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 19.441, df = 1, p-value = 1.038e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.09051632 -0.03439430
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4944347 
    ## 
    ## [1] "lemongrass"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 29.712, df = 1, p-value = 5.012e-08
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.08858191 -0.04145017
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4319794 0.4969954 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 5.8631, df = 1, p-value = 0.01546
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.072500985 -0.007545685
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.5089855 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 4.0441, df = 1, p-value = 0.04433
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.0006107419 0.0518258660
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4427439 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 7.7224, df = 1, p-value = 0.005454
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.01334143 0.07785070
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4233661 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 7.7695, df = 1, p-value = 0.005314
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.01081569 0.06314992
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4319794 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 4.0821, df = 1, p-value = 0.04334
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.0008692982 0.0587015006
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4391768 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 8.7034, df = 1, p-value = 0.003176
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.01560197 0.07810644
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4221080 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 2.3265, df = 1, p-value = 0.1272
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.058046687  0.007101671
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4944347 
    ## 
    ## [1] "lemongrass05"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 3.641, df = 1, p-value = 0.05637
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.0567969223  0.0007304581
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4689622 0.4969954 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 20.394, df = 1, p-value = 6.304e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.10035353 -0.03926394
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.5089855 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.07959, df = 1, p-value = 0.7779
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.02670112  0.01956693
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4427439 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.013, df = 1, p-value = 0.3142
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01449715  0.04611848
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4233661 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.33326, df = 1, p-value = 0.5637
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01655228  0.03094709
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4319794 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 4.0821, df = 1, p-value = 0.04334
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.0587015006 -0.0008692982
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4689622 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.278, df = 1, p-value = 0.2583
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.01217307  0.04631068
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4221080 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 12.671, df = 1, p-value = 0.0003714
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.08590504 -0.02461078
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4944347 
    ## 
    ## [1] "picaridin"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 18.388, df = 1, p-value = 1.802e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.08439328 -0.03124398
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4391768 0.4969954 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 27.405, df = 1, p-value = 1.65e-07
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.11964761 -0.05410747
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.5089855 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 2.3943, df = 1, p-value = 0.1218
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.046608336  0.005336537
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4427439 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.0019009, df = 1, p-value = 0.9652
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03380713  0.03129085
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4233661 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.50606, df = 1, p-value = 0.4768
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03639610  0.01665331
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4319794 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 8.7034, df = 1, p-value = 0.003176
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.07810644 -0.01560197
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4689622 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 1.278, df = 1, p-value = 0.2583
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.04631068  0.01217307
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4391768 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 18.882, df = 1, p-value = 1.391e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.10519251 -0.03946091
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4944347 
    ## 
    ## [1] "picaridin05"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 25.672, df = 1, p-value = 4.047e-07
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.10397848 -0.04579639
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4221080 0.4969954 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.6696, df = 1, p-value = 0.4132
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.04858636  0.01948470
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.5089855 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 13.781, df = 1, p-value = 0.0002054
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.02415238 0.07922925
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4427439 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 17.188, df = 1, p-value = 3.386e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03724597 0.10489118
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4233661 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 19.441, df = 1, p-value = 1.038e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03439430 0.09051632
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4319794 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 2.3265, df = 1, p-value = 0.1272
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.007101671  0.058046687
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4689622 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 12.671, df = 1, p-value = 0.0003714
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.02461078 0.08590504
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4391768 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 18.882, df = 1, p-value = 1.391e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03946091 0.10519251
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4221080 
    ## 
    ## [1] "ir3535"
    ## [1] "vs"
    ## [1] "ir353505"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.018622, df = 1, p-value = 0.8915
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.03306358  0.02794213
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4944347 0.4969954 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "controls"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.5692, df = 1, p-value = 0.4506
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.04239017  0.01840996
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.5089855 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "eugenol"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 21.801, df = 1, p-value = 3.024e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03130600 0.07719707
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4427439 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "eugenol05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 23.066, df = 1, p-value = 1.566e-06
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.0434673 0.1037913
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4233661 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "lemongrass"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 29.712, df = 1, p-value = 5.012e-08
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.04145017 0.08858191
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4319794 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "lemongrass05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 3.641, df = 1, p-value = 0.05637
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.0007304581  0.0567969223
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4689622 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "picaridin"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 18.388, df = 1, p-value = 1.802e-05
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.03124398 0.08439328
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4391768 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "picaridin05"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 25.672, df = 1, p-value = 4.047e-07
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  0.04579639 0.10397848
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4221080 
    ## 
    ## [1] "ir353505"
    ## [1] "vs"
    ## [1] "ir3535"
    ## 
    ##  2-sample test for equality of proportions with continuity correction
    ## 
    ## data:  c(stim.peaks[y], stim.peaks[j]) out of c(total.peaks[y], total.peaks[j])
    ## X-squared = 0.018622, df = 1, p-value = 0.8915
    ## alternative hypothesis: two.sided
    ## 95 percent confidence interval:
    ##  -0.02794213  0.03306358
    ## sample estimates:
    ##    prop 1    prop 2 
    ## 0.4969954 0.4944347

### Inter saccadic interval (ISI)

#### Plots

Distributions of the ISI duration (seconds):

- light or dark colours -\> 0.5% or 1.0% concentration;

- dashed vs solid lines -\> MOP vs RP;

- solid black line -\> Controls;

Controls are shown as reference as a single line since not significantly
different.

![](Markdown-tethering_files/figure-gfm/Density%20plot%20(ISI)-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Density%20plot%20(ISI)-2.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Density%20plot%20(ISI)-3.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Density%20plot%20(ISI)-4.png)<!-- -->

#### Statistics

Normality and heteroscedasticity checks. 2–way ANOVA and Games - Howell
post-hoc test.

    ## [1] "controls"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.6057, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.70206, p-value < 2.2e-16
    ## 
    ## [1] "eugenol"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.47869, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.42627, p-value < 2.2e-16
    ## 
    ## [1] "eugenol05"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.60857, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.71676, p-value < 2.2e-16
    ## 
    ## [1] "lemongrass"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.49621, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.45163, p-value < 2.2e-16
    ## 
    ## [1] "lemongrass05"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.62519, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.63236, p-value < 2.2e-16
    ## 
    ## [1] "picaridin"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.59014, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.62561, p-value < 2.2e-16
    ## 
    ## [1] "picaridin05"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.64648, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.50937, p-value < 2.2e-16
    ## 
    ## [1] "ir3535"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.58613, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.58386, p-value < 2.2e-16
    ## 
    ## [1] "ir353505"
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 0, ]$interevent
    ## W = 0.56234, p-value < 2.2e-16
    ## 
    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  FDS_ALL[FDS_ALL$group %in% m & FDS_ALL$stim == 1, ]$interevent
    ## W = 0.66818, p-value < 2.2e-16

![](Markdown-tethering_files/figure-gfm/Shapiro-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Shapiro-2.png)<!-- -->

    ## [1] "Leven Test"

    ## # A tibble: 1 × 4
    ##     df1   df2 statistic         p
    ##   <int> <int>     <dbl>     <dbl>
    ## 1     8 24785      72.8 3.76e-119

    ## 
    ##  One-way analysis of means (not assuming equal variances)
    ## 
    ## data:  FDS_ALL$interevent and FDS_ALL$group + FDS_ALL$stim
    ## F = 35.338, num df = 17.0, denom df = 7451.4, p-value < 2.2e-16

    ##                controls.0 eugenol.0 eugenol05.0 ir3535.0 ir353505.0
    ## eugenol.0      0.00432    -         -           -        -         
    ## eugenol05.0    0.00797    6.0e-13   -           -        -         
    ## ir3535.0       0.01298    1.4e-11   1.00000     -        -         
    ## ir353505.0     0.99998    0.06423   8.2e-05     0.00030  -         
    ## lemongrass.0   0.01036    1.00000   4.1e-14     4.6e-11  0.13034   
    ## lemongrass05.0 0.04788    < 2e-16   0.99999     0.99983  0.00045   
    ## picaridin.0    1.00000    1.5e-05   0.00223     0.00529  0.99970   
    ## picaridin05.0  0.99869    0.07712   6.9e-06     4.3e-05  1.00000   
    ## controls.1     1.00000    0.00637   0.00037     0.00109  1.00000   
    ## eugenol.1      1.00000    1.3e-06   0.00011     0.00053  1.00000   
    ## eugenol05.1    4.4e-13    3.4e-13   6.0e-06     0.00013  < 2e-16   
    ## ir3535.1       0.00377    3.0e-13   1.00000     1.00000  4.9e-05   
    ## ir353505.1     0.99781    0.02576   2.6e-06     2.2e-05  1.00000   
    ## lemongrass.1   0.98436    6.2e-10   0.10150     0.13656  0.27251   
    ## lemongrass05.1 3.0e-06    5.1e-13   0.99704     0.99995  2.3e-09   
    ## picaridin.1    0.00269    < 2e-16   1.00000     1.00000  9.0e-06   
    ## picaridin05.1  0.00046    2.2e-12   0.99668     0.99983  8.5e-06   
    ##                lemongrass.0 lemongrass05.0 picaridin.0 picaridin05.0 controls.1
    ## eugenol.0      -            -              -           -             -         
    ## eugenol05.0    -            -              -           -             -         
    ## ir3535.0       -            -              -           -             -         
    ## ir353505.0     -            -              -           -             -         
    ## lemongrass.0   -            -              -           -             -         
    ## lemongrass05.0 < 2e-16      -              -           -             -         
    ## picaridin.0    8.2e-05      0.01348        -           -             -         
    ## picaridin05.0  0.16621      2.7e-05        0.98538     -             -         
    ## controls.1     0.01734      0.00222        1.00000     1.00000       -         
    ## eugenol.1      1.4e-05      0.00048        1.00000     0.99923       1.00000   
    ## eugenol05.1    < 2e-16      3.6e-09        1.8e-13     < 2e-16       < 2e-16   
    ## ir3535.1       5.7e-13      0.99800        0.00116     5.2e-06       0.00021   
    ## ir353505.1     0.07331      8.1e-06        0.97399     1.00000       1.00000   
    ## lemongrass.1   6.7e-09      0.43175        0.94907     0.04883       0.59561   
    ## lemongrass05.1 3.8e-13      0.57140        1.5e-07     4.5e-11       1.7e-08   
    ## picaridin.1    < 2e-16      0.99999        0.00040     3.4e-07       5.4e-05   
    ## picaridin05.1  4.8e-12      0.75979        0.00017     1.3e-06       3.2e-05   
    ##                eugenol.1 eugenol05.1 ir3535.1 ir353505.1 lemongrass.1
    ## eugenol.0      -         -           -        -          -           
    ## eugenol05.0    -         -           -        -          -           
    ## ir3535.0       -         -           -        -          -           
    ## ir353505.0     -         -           -        -          -           
    ## lemongrass.0   -         -           -        -          -           
    ## lemongrass05.0 -         -           -        -          -           
    ## picaridin.0    -         -           -        -          -           
    ## picaridin05.0  -         -           -        -          -           
    ## controls.1     -         -           -        -          -           
    ## eugenol.1      -         -           -        -          -           
    ## eugenol05.1    4.1e-13   -           -        -          -           
    ## ir3535.1       7.4e-05   0.00017     -        -          -           
    ## ir353505.1     0.99809   < 2e-16     2.3e-06  -          -           
    ## lemongrass.1   0.41069   4.1e-13     0.05017  0.02067    -           
    ## lemongrass05.1 7.7e-10   0.00129     1.00000  6.2e-12    4.4e-05     
    ## picaridin.1    6.3e-06   5.7e-07     1.00000  7.8e-08    0.04114     
    ## picaridin05.1  1.5e-05   0.05196     0.99997  7.0e-07    0.00652     
    ##                lemongrass05.1 picaridin.1
    ## eugenol.0      -              -          
    ## eugenol05.0    -              -          
    ## ir3535.0       -              -          
    ## ir353505.0     -              -          
    ## lemongrass.0   -              -          
    ## lemongrass05.0 -              -          
    ## picaridin.0    -              -          
    ## picaridin05.0  -              -          
    ## controls.1     -              -          
    ## eugenol.1      -              -          
    ## eugenol05.1    -              -          
    ## ir3535.1       -              -          
    ## ir353505.1     -              -          
    ## lemongrass.1   -              -          
    ## lemongrass05.1 -              -          
    ## picaridin.1    0.98466        -          
    ## picaridin05.1  1.00000        0.98913

### ISI and Peaks across trials

#### Plots

Box-plots of ISI varying in time (along trials).

The chunks of code plot the following:

1.  For each group: dot-plots for the average ISI per single flies (mean
    and median), plus the average ISI (mean) per single flies *per
    trial.*
2.  For each group: ISI box-plots along trials for each fly.
3.  For each group: dot-plots and raster plots with the density of peaks
    over trials.

**Reminder**: MOP is shown in dark grey, while RP is labelled by light
grey.

![](Markdown-tethering_files/figure-gfm/Dotplot-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-2.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-3.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-4.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-5.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-6.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-7.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-8.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-9.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-10.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-11.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-12.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-13.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-14.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-15.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-16.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-17.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-18.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-19.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-20.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-21.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-22.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-23.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-24.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-25.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-26.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-27.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-28.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-29.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-30.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-31.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-32.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-33.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-34.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-35.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-36.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-37.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-38.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-39.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-40.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-41.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-42.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-43.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-44.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-45.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-46.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-47.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-48.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-49.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-50.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-51.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-52.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-53.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-54.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-55.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-56.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-57.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-58.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-59.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-60.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-61.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-62.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-63.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-64.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-65.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-66.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-67.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-68.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-69.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-70.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-71.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-72.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-73.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-74.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-75.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-76.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-77.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-78.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-79.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-80.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-81.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-82.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-83.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-84.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-85.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-86.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-87.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-88.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-89.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-90.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-91.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-92.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Dotplot-93.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-94.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-95.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-96.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-97.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-98.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-99.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-100.png)<!-- -->

    ## Warning: Width not defined
    ## ℹ Set with `position_dodge(width = ...)`
    ## Width not defined
    ## ℹ Set with `position_dodge(width = ...)`

![](Markdown-tethering_files/figure-gfm/Dotplot-101.png)<!-- -->

![](Markdown-tethering_files/figure-gfm/Boxplot-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-2.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-3.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-4.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-5.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-6.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-7.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-8.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-9.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-10.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-11.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-12.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-13.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-14.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-15.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-16.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-17.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Boxplot-18.png)<!-- -->

Raster plots with distribution of ISI along trials.

![](Markdown-tethering_files/figure-gfm/Gradi-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Gradi-2.png)<!-- -->

Raster plots with distribution of Peaks along trials.

![](Markdown-tethering_files/figure-gfm/Peaks%20number%20density%20over%20trials-1.png)<!-- -->![](Markdown-tethering_files/figure-gfm/Peaks%20number%20density%20over%20trials-2.png)<!-- -->

#### Statistics

GLM fitting and permutations.

    ## 
    ## Call:
    ## lm(formula = interevent ~ trial + FlyId, data = FDS_ALL)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.1054 -0.5801 -0.3167  0.0989 26.7240 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  1.179262   0.022442  52.547  < 2e-16 ***
    ## trial       -0.064048   0.004640 -13.803  < 2e-16 ***
    ## FlyId        0.008193   0.002204   3.718 0.000201 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.224 on 24791 degrees of freedom
    ## Multiple R-squared:  0.008127,   Adjusted R-squared:  0.008047 
    ## F-statistic: 101.6 on 2 and 24791 DF,  p-value: < 2.2e-16

![](Markdown-tethering_files/figure-gfm/Fitting%20ISI-1.png)<!-- -->

    ## 
    ## Call:
    ## glm(formula = peak ~ sec, family = gaussian(link = log), data = count_df[count_df$group %in% 
    ##     "controls", ])
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 1.3368919  0.0545507   24.51  < 2e-16 ***
    ## sec         0.0008911  0.0001435    6.21 1.47e-09 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 5.213535)
    ## 
    ##     Null deviance: 2072.9  on 357  degrees of freedom
    ## Residual deviance: 1856.0  on 356  degrees of freedom
    ## AIC: 1611.1
    ## 
    ## Number of Fisher Scoring iterations: 5

    ## [1] 0.1046405

![](Markdown-tethering_files/figure-gfm/Fitting%20ISI-2.png)<!-- -->

    ## Summary for glm model:
    ## 
    ## Call:
    ## glm(formula = peak ~ sec, family = gaussian, data = Peak_DF[Peak_DF$group == 
    ##     "controls", ])
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -83.093628   0.773227 -107.46   <2e-16 ***
    ## sec           0.022370   0.002157   10.37   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for gaussian family taken to be 229.208)
    ## 
    ##     Null deviance: 433323  on 1784  degrees of freedom
    ## Residual deviance: 408678  on 1783  degrees of freedom
    ## AIC: 14770
    ## 
    ## Number of Fisher Scoring iterations: 2
    ## 
    ## 
    ## Summary for lm model:
    ## 
    ## Call:
    ## lm(formula = peak ~ sec, data = count_df)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -7.367 -2.687 -0.623  2.122 18.851 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 5.4835887  0.1268415   43.23   <2e-16 ***
    ## sec         0.0052054  0.0003762   13.84   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 3.663 on 3219 degrees of freedom
    ## Multiple R-squared:  0.05614,    Adjusted R-squared:  0.05584 
    ## F-statistic: 191.4 on 1 and 3219 DF,  p-value: < 2.2e-16

    ## [1] 0.08747246

    ## [1] 0.08834753

    ## [1] 0.09165324

    ## [1] 0.1231381

    ## [1] 0.13215

    ## [1] 0.08834753

![](Markdown-tethering_files/figure-gfm/Fitting%20ISI-3.png)<!-- -->

    ## [1] "ANOVA test is significant\n"
    ##   Kruskal-Wallis rank sum test
    ## 
    ## data: residuals and group
    ## Kruskal-Wallis chi-squared = 0.817, df = 8, p-value = 1
    ## 
    ## 
    ##                        Comparison of residuals by group                        
    ##                                  (Bonferroni)                                  
    ## Col Mean-|
    ## Row Mean |   controls    eugenol   eugenol0     ir3535   ir353505   lemongra
    ## ---------+------------------------------------------------------------------
    ##  eugenol |  -0.274174
    ##          |     1.0000
    ##          |
    ## eugenol0 |   0.300906   0.575081
    ##          |     1.0000     1.0000
    ##          |
    ##   ir3535 |  -0.224733   0.049440  -0.525640
    ##          |     1.0000     1.0000     1.0000
    ##          |
    ## ir353505 |   0.266804   0.540978  -0.034102   0.491538
    ##          |     1.0000     1.0000     1.0000     1.0000
    ##          |
    ## lemongra |  -0.060555   0.213618  -0.361462   0.164178  -0.327360
    ##          |     1.0000     1.0000     1.0000     1.0000     1.0000
    ##          |
    ## lemongra |   0.220351   0.494525  -0.080555   0.445085  -0.046452   0.280907
    ##          |     1.0000     1.0000     1.0000     1.0000     1.0000     1.0000
    ##          |
    ## picaridi |  -0.006493   0.267680  -0.307400   0.218240  -0.273298   0.054062
    ##          |     1.0000     1.0000     1.0000     1.0000     1.0000     1.0000
    ##          |
    ## picaridi |   0.318236   0.592411   0.017330   0.542970   0.051432   0.378792
    ##          |     1.0000     1.0000     1.0000     1.0000     1.0000     1.0000
    ## Col Mean-|
    ## Row Mean |   lemongra   picaridi
    ## ---------+----------------------
    ## picaridi |  -0.226845
    ##          |     1.0000
    ##          |
    ## picaridi |   0.097885   0.324730
    ##          |     1.0000     1.0000
    ## 
    ## alpha = 0.05
    ## Reject Ho if p <= alpha/2
    ## $chi2
    ## [1] 0.8170078
    ## 
    ## $Z
    ##  [1] -0.274174476  0.300906687  0.575081163 -0.224733830  0.049440646
    ##  [6] -0.525640517  0.266804195  0.540978671 -0.034102492  0.491538025
    ## [11] -0.060555827  0.213618649 -0.361462514  0.164178003 -0.327360022
    ## [16]  0.220351501  0.494525977 -0.080555186  0.445085331 -0.046452694
    ## [21]  0.280907328 -0.006493816  0.267680661 -0.307400503  0.218240015
    ## [26] -0.273298010  0.054062012 -0.226845316  0.318236808  0.592411285
    ## [31]  0.017330122  0.542970639  0.051432614  0.378792636  0.097885308
    ## [36]  0.324730624
    ## 
    ## $P
    ##  [1] 0.3919753 0.3817428 0.2826182 0.4110932 0.4802841 0.2995690 0.3948100
    ##  [8] 0.2942611 0.4863977 0.3115230 0.4758565 0.4154222 0.3588769 0.4347955
    ## [15] 0.3716978 0.4127987 0.3104674 0.4678979 0.3281290 0.4814747 0.3893907
    ## [22] 0.4974094 0.3944726 0.3792693 0.4136211 0.3923121 0.4784429 0.4102720
    ## [29] 0.3751527 0.2767876 0.4930866 0.2935750 0.4794904 0.3524209 0.4610117
    ## [36] 0.3726925
    ## 
    ## $P.adjusted
    ##  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ## 
    ## $comparisons
    ##  [1] "controls - eugenol"         "controls - eugenol05"      
    ##  [3] "eugenol - eugenol05"        "controls - ir3535"         
    ##  [5] "eugenol - ir3535"           "eugenol05 - ir3535"        
    ##  [7] "controls - ir353505"        "eugenol - ir353505"        
    ##  [9] "eugenol05 - ir353505"       "ir3535 - ir353505"         
    ## [11] "controls - lemongrass"      "eugenol - lemongrass"      
    ## [13] "eugenol05 - lemongrass"     "ir3535 - lemongrass"       
    ## [15] "ir353505 - lemongrass"      "controls - lemongrass05"   
    ## [17] "eugenol - lemongrass05"     "eugenol05 - lemongrass05"  
    ## [19] "ir3535 - lemongrass05"      "ir353505 - lemongrass05"   
    ## [21] "lemongrass - lemongrass05"  "controls - picaridin"      
    ## [23] "eugenol - picaridin"        "eugenol05 - picaridin"     
    ## [25] "ir3535 - picaridin"         "ir353505 - picaridin"      
    ## [27] "lemongrass - picaridin"     "lemongrass05 - picaridin"  
    ## [29] "controls - picaridin05"     "eugenol - picaridin05"     
    ## [31] "eugenol05 - picaridin05"    "ir3535 - picaridin05"      
    ## [33] "ir353505 - picaridin05"     "lemongrass - picaridin05"  
    ## [35] "lemongrass05 - picaridin05" "picaridin - picaridin05"

    ## [1] "Probability of casually obtaining similar distributions"

    ## [1] "p < 0.0001"

## Generating data sets from raw data

***NOTE: the present section is not required per se, as the relevant
data sets for the further analysis are provided for download. The code
section below can be used when starting the analysis from the files
generated through the MATLAB script “Peaks_and_velocities_odours” (also
available for download).***

The following code is split into two parts:

1.  analyzes head saccades tracked through
    ‘[*FlyAlyzer*](https://github.com/michaelrauscher/flyalyzer "Rauscher and Fox, 2021")’
    ([Rauscher and Fox,
    2021](https://royalsocietypublishing.org/doi/full/10.1098/rspb.2020.2374))
    and then tagged through a Matlab script. The code takes in input the
    ‘.txt’ or ‘.csv’ files generate from a Matlab script (‘findpeaks’
    function, script also available for downloading); then it divides
    data reflecting the phases of the experimental paradigm. Note that,
    due to how the code was written, you should conduct the analysis
    only on ONE variable (peaks, slow phase velocity, or others) at a
    time, although the script let you load all the variables eventually,
    and all the relevant files must be placed togheter within a common
    folder;

2.  takes all the generated ‘peaks.txt’ and processes them to write a
    new data frame for the remaining analysis.
