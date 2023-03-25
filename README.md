# Google Summer of Code 2023

## Organisation: The R Project for Statistical Computing

### Project: imputeTestbench for multivariate time series
<https://github.com/rstats-gsoc/gsoc2023/wiki/imputeTestbench-for-multivariate-time-series>

#### Tests
- Easy: Download the `imputeTestbench` package and demonstrate it with a naturally occurring time series. Document it with RMarkdown.

- Medium: Suggest possible updates or a new feature you would like to include in the next version of the `imputeTestbench` package.

- Hard: Develop a dummy code of 5 functions and a vignette and pass it with no Error/Warning/Note through <https://win-builder.r-project.org/>

#### Easy Test
Install the `imputeTestbench` package using `install.packages("imputeTestbench")` and load it.

``` r
library(imputeTestbench)
```
```
    ## Registered S3 method overwritten by 'quantmod':
    ##   method            from
    ##   as.zoo.data.frame zoo
```
``` r
library(imputeTS)
```

Load temperature dataset. The dataset contains \~5 years of **high temporal resolution** (hourly measurements) temperature data from Vancouver, Canada (<https://www.kaggle.com/selfishgene/historical-hourly-weather-data?select=wind_speed.csv>)

``` r
library(readr)
temperature <- read_csv("temperature.csv")
```
```
    ## Rows: 45252 Columns: 2
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): datetime
    ## dbl (1): Vancouver
    ##
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```
``` r
summary(temperature)
```
```
    ##    datetime           Vancouver
    ##  Length:45252       Min.   :245.2
    ##  Class :character   1st Qu.:279.2
    ##  Mode  :character   Median :283.4
    ##                     Mean   :283.9
    ##                     3rd Qu.:288.6
    ##                     Max.   :307.0
    ##                     NA's   :794
```
Use `imputeTestbench` to compare various imputation methods

``` r
errs <- impute_errors(temperature$Vancouver, methods = c("na_interpolation", "na_locf", "na_ma", "na_kalman"), missPercentFrom = 10, missPercentTo = 50)
data.frame(errs)
```
```
    ##   Parameter MissingPercent na_interpolation   na_locf     na_ma na_kalman
    ## 1      rmse             10        0.1995260 0.3403313 0.2371795 0.1976207
    ## 2      rmse             20        0.2919222 0.5256478 0.3468655 0.2875770
    ## 3      rmse             30        0.3843370 0.7153512 0.4595359 0.3741581
    ## 4      rmse             40        0.4785488 0.9284088 0.5719623 0.4584718
    ## 5      rmse             50        0.6126679 1.1819694 0.7250498 0.5730364
```
``` r
plot_errors(errs, plotType = "line")
```

![](files/proposal_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

#### Medium Test
`imputeTestbench` is a great package for comparing various methods of imputation. This project modifies the package to work with genomics data. Few updates that I would suggest are:
-   Introducing new sampling methods such as monte carlo.
-   Making the package interactive.
-   Developing a ShinyApp for the package.

#### Hard Test
For the final test, I created a R package called `Calculator`. It contains five functions, `add()`, `substract()`, `multiply()`, `divide()` and `modulo()` , with documentation and tests. Then, using `devtools::check()`, I checked for any errors or warning, and uploaded the source file to <https://win-builder.r-project.org/>. It passed without errors/warnings/note.

The result of <https://win-builder.r-project.org/> is included in the github repository(`00check.log`).
