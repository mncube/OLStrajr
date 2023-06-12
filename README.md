
<!-- README.md is generated from README.Rmd. Please edit that file -->

# OLStrajr

<!-- badges: start -->
<!-- badges: end -->

The [Maximizing the Yield of Small Samples in Prevention Research: A
Review of General Strategies and Best
Practices](https://doi.org/10.1007%2Fs11121-014-0542-7) paper by Hopkin,
Hoyle, and Gottfredson(2015) suggests that when working with small
sample sizes, analysts should leverage plots to explore within-group
associations between predictor variables and the outcome variable. These
plots are intended to provide detailed insights about the data rather
than to be used for generalizing to the population.

The paper cites the work of [Carrig, Wirth, and Curran
(2004)](http://dx.doi.org/10.1207/S15328007SEM1101_9) who proposed an
easy-to-use SAS macro for visualizing person-specific growth
trajectories with repeated measures data.

The objective of the OLStrajr package is to bring the functionality of
this SAS macro, OLStraj, to R. While the original link to the macro in
Carrig’s paper is no longer active, it can still be accessed through the
[Wayback Machine](https://archive.org/web/).

## Installation

You can install the development version of OLStrajr as follows:

``` r
# install.packages("devtools")
devtools::install_github("mightymetrika/OLStrajr")
```

## GPT-4 Dog Barking Poisson

This is an illustrative example that shows how to use OLStrajr.

On June 12th, 2023, I asked GPT-4 with Browsing to provide an estimation
of the number of times a dog might bark from 6:00AM to 6:01AM. I
received the following response:

“The number of times a dog barks within a minute can vary widely based
on many factors, such as the dog’s breed, personality, and the stimulus
causing the barking. Some dogs may bark once or twice and then stop,
while others may bark continuously. If we take a rough estimate, a dog
that barks continuously might bark once every two seconds, which would
equate to about 30 barks in a minute. However, this is a very rough
estimate and the actual number could be much higher or lower depending
on the dog and the circumstances.

Please note that excessive barking can be a sign of stress, boredom, or
other issues in a dog, so if a dog is barking a lot, it’s important to
try to identify and address the underlying cause.”

Based on this response, I simulated five days of data from five
different houses, where the dogs start barking at 6AM. The barking
frequency is modeled as a Poisson process where the week becomes
increasingly boring. This basic example shows how to use OLStrajr

``` r
library(OLStrajr)
```

### Simulate Data

``` r
set.seed(489)
barks <- data.frame(dog = c(1, 2, 3, 4, 5),
                    mon = rpois(5, 14),
                    tues = rpois(5, 18),
                    weds = rpois(5, 22),
                    thurs = rpois(5, 26),
                    fri = rpois(5, 30))
```

### OLStraj

To obtain the OLS trajectories, use the following code:

``` r
barks_traj <- OLStraj(data = barks,
                      idvarname = "dog",
                      predvarname = "Day",
                      outvarname = "Barks",
                      varlist = c("mon", "tues", "weds", "thurs", "fri"),
                      timepts = c(1, 2, 3, 4, 5),
                      regtype = "lin",
                      int_bins = 5,
                      lin_bins = 5)
```

The OLStraj function provides several types of plots that facilitate the
examination of your data:

### Group Plots:

``` r
barks_traj$group_plots
#> $simple_joined
```

<img src="man/figures/README-unnamed-chunk-6-1.png" width="100%" />

    #> 
    #> $ols
    #> `geom_smooth()` using formula = 'y ~ x'

<img src="man/figures/README-unnamed-chunk-6-2.png" width="100%" />

Group plots depict the simple-joined trajectories and the estimated
trajectories for each group in your data, allowing for a quick
comparison of trends across different groups.

### Individual OLS Trajectories:

``` r
barks_traj$individual_plots
#> $`ols 1`
#> `geom_smooth()` using formula = 'y ~ x'
```

<img src="man/figures/README-unnamed-chunk-7-1.png" width="100%" />

    #> 
    #> $`ols 2`
    #> `geom_smooth()` using formula = 'y ~ x'

<img src="man/figures/README-unnamed-chunk-7-2.png" width="100%" />

    #> 
    #> $`ols 3`
    #> `geom_smooth()` using formula = 'y ~ x'

<img src="man/figures/README-unnamed-chunk-7-3.png" width="100%" />

    #> 
    #> $`ols 4`
    #> `geom_smooth()` using formula = 'y ~ x'

<img src="man/figures/README-unnamed-chunk-7-4.png" width="100%" />

    #> 
    #> $`ols 5`
    #> `geom_smooth()` using formula = 'y ~ x'

<img src="man/figures/README-unnamed-chunk-7-5.png" width="100%" />

Individual plots represent each subject’s unique trajectory over time,
providing insights into individual patterns of change.

### Histograms for intercepts and slopes:

``` r
barks_traj$histogram_plots
#> $intercepts
```

<img src="man/figures/README-unnamed-chunk-8-1.png" width="100%" />

    #> 
    #> $slopes

<img src="man/figures/README-unnamed-chunk-8-2.png" width="100%" />

These histograms reveal the distribution of intercepts and slopes across
subjects. They are useful for identifying skewness or other interesting
distributional properties in the data.

### Box plots of intercepts and slopes:

``` r
barks_traj$box_plot
```

<img src="man/figures/README-unnamed-chunk-9-1.png" width="100%" />

Box plots provide a summary of the distribution of intercepts and slopes
across subjects. They allow you to visualize the spread and skewness of
the data. Any outliers are clearly highlighted in these plots. In the
OLStraj box plots, the mean value is represented by a blue dot.
Additionally, each outlier is labeled with the corresponding subject
identifier, providing a straightforward way to identify subjects whose
data deviate significantly from the rest of the group.
