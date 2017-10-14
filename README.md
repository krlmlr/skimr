# skimr

```
## Dev mode: OFF
```

[![Build Status](https://travis-ci.org/ropenscilabs/skimr.svg?branch=master)](https://travis-ci.org/ropenscilabs/skimr)
[![codecov](https://codecov.io/gh/ropenscilabs/skimr/branch/master/graph/badge.svg)](https://codecov.io/gh/ropenscilabs/skimr)

The goal of skimr is to provide a frictionless approach to dealing with summary statistics iteratively and interactively as part of a pipeline, and that conforms to the principle of least surprise. 

`skimr` provides summary statistics that you can skim quickly to understand and your data and see what may be missing. It handles different data types (numerics, factors, etc), and returns a skimr object that can be piped or displayed nicely for the human reader. 

See our blog post [here](https://rawgit.com/ropenscilabs/skimr/master/blog.html).

## Installation


``` r
# install.packages("devtools")
devtools::install_github("hadley/pillar")
devtools::install_github("ropenscilabs/skimr")
```

## Skim statistics in the console

- added missing, complete, n, sd
- reports numeric/int/double separately from factor/chr
- handles dates, logicals
- uses [Hadley Wickham's pillar package](https://github.com/hadley/pillar), specifically `pillar::spark-bar()`

**Nicely separates numeric and factor variables:**  


```r
skim(chickwts)
```

```
## Skim summary statistics
##  n obs: 71 
##  n variables: 2 
## 
## Variable type: factor 
##    var missing complete  n n_unique                         top_counts ordered
## 1 feed       0       71 71        6 soy: 14, cas: 12, lin: 12, sun: 12   FALSE
## 
## Variable type: numeric 
##      var missing complete  n   mean    sd min   p25 median   p75 max     hist
## 1 weight       0       71 71 261.31 78.07 108 204.5    258 323.5 423 ▃▅▅▇▃▇▂▂
```

**Many numeric variables:**  

```r
skim(mtcars)
```

```
## Skim summary statistics
##  n obs: 32 
##  n variables: 11 
## 
## Variable type: numeric 
##     var missing complete  n   mean     sd   min    p25 median    p75    max     hist
## 1    am       0       32 32   0.41   0.5   0      0      0      1      1    ▇▁▁▁▁▁▁▆
## 2  carb       0       32 32   2.81   1.62  1      2      2      4      8    ▆▇▂▇▁▁▁▁
## 3   cyl       0       32 32   6.19   1.79  4      4      6      8      8    ▆▁▁▃▁▁▁▇
## 4  disp       0       32 32 230.72 123.94 71.1  120.83 196.3  326    472    ▇▆▁▂▅▃▁▂
## 5  drat       0       32 32   3.6    0.53  2.76   3.08   3.7    3.92   4.93 ▃▇▁▅▇▂▁▁
## 6  gear       0       32 32   3.69   0.74  3      3      4      4      5    ▇▁▁▆▁▁▁▂
## 7    hp       0       32 32 146.69  68.56 52     96.5  123    180    335    ▃▇▃▅▂▃▁▁
## 8   mpg       0       32 32  20.09   6.03 10.4   15.43  19.2   22.8   33.9  ▃▇▇▇▃▂▂▂
## 9  qsec       0       32 32  17.85   1.79 14.5   16.89  17.71  18.9   22.9  ▃▂▇▆▃▃▁▁
## 10   vs       0       32 32   0.44   0.5   0      0      0      1      1    ▇▁▁▁▁▁▁▆
## 11   wt       0       32 32   3.22   0.98  1.51   2.58   3.33   3.61   5.42 ▃▃▃▇▆▁▁▂
```

 
**Another example:**  

```r
skim(iris)
```

```
## Skim summary statistics
##  n obs: 150 
##  n variables: 5 
## 
## Variable type: factor 
##       var missing complete   n n_unique                       top_counts ordered
## 1 Species       0      150 150        3 set: 50, ver: 50, vir: 50, NA: 0   FALSE
## 
## Variable type: numeric 
##            var missing complete   n mean   sd min p25 median p75 max     hist
## 1 Petal.Length       0      150 150 3.76 1.77 1   1.6   4.35 5.1 6.9 ▇▁▁▂▅▅▃▁
## 2  Petal.Width       0      150 150 1.2  0.76 0.1 0.3   1.3  1.8 2.5 ▇▁▁▅▃▃▂▂
## 3 Sepal.Length       0      150 150 5.84 0.83 4.3 5.1   5.8  6.4 7.9 ▂▇▅▇▆▅▂▂
## 4  Sepal.Width       0      150 150 3.06 0.44 2   2.8   3    3.3 4.4 ▁▂▅▇▃▂▁▁
```

## Handles grouped data

Skim() can handle data that has been grouped using `dplyr::group_by`.


```r
iris %>% dplyr::group_by(Species) %>% skim()
```

```
## Skim summary statistics
##  n obs: 150 
##  n variables: 5 
##  group variables: Species 
## 
## Variable type: numeric 
##       Species          var missing complete  n mean   sd min  p25 median  p75 max     hist
## 1      setosa Petal.Length       0       50 50 1.46 0.17 1   1.4    1.5  1.58 1.9 ▁▁▅▇▇▅▂▁
## 2      setosa  Petal.Width       0       50 50 0.25 0.11 0.1 0.2    0.2  0.3  0.6 ▂▇▁▂▂▁▁▁
## 3      setosa Sepal.Length       0       50 50 5.01 0.35 4.3 4.8    5    5.2  5.8 ▂▃▅▇▇▃▁▂
## 4      setosa  Sepal.Width       0       50 50 3.43 0.38 2.3 3.2    3.4  3.68 4.4 ▁▁▃▅▇▃▂▁
## 5  versicolor Petal.Length       0       50 50 4.26 0.47 3   4      4.35 4.6  5.1 ▁▃▂▆▆▇▇▃
## 6  versicolor  Petal.Width       0       50 50 1.33 0.2  1   1.2    1.3  1.5  1.8 ▆▃▇▅▆▂▁▁
## 7  versicolor Sepal.Length       0       50 50 5.94 0.52 4.9 5.6    5.9  6.3  7   ▃▂▇▇▇▃▅▂
## 8  versicolor  Sepal.Width       0       50 50 2.77 0.31 2   2.52   2.8  3    3.4 ▁▂▃▅▃▇▃▁
## 9   virginica Petal.Length       0       50 50 5.55 0.55 4.5 5.1    5.55 5.88 6.9 ▂▇▃▇▅▂▁▂
## 10  virginica  Petal.Width       0       50 50 2.03 0.27 1.4 1.8    2    2.3  2.5 ▂▁▇▃▃▆▅▃
## 11  virginica Sepal.Length       0       50 50 6.59 0.64 4.9 6.23   6.5  6.9  7.9 ▁▁▃▇▅▃▂▃
## 12  virginica  Sepal.Width       0       50 50 2.97 0.32 2.2 2.8    3    3.18 3.8 ▁▃▇▇▅▃▁▂
```

## skim_df object (long format)

By default `skim` prints beautifully in the console, but it also produces a long, tidy-format skim_df object that can be computed on. 


```r
a <-  skim(chickwts)
dim(a)
```

```
## [1] 23  6
```



```r
print.data.frame(skim(chickwts))
```

```
##       var    type       stat     level    value formatted
## 1  weight numeric    missing      .all   0.0000         0
## 2  weight numeric   complete      .all  71.0000        71
## 3  weight numeric          n      .all  71.0000        71
## 4  weight numeric       mean      .all 261.3099    261.31
## 5  weight numeric         sd      .all  78.0737     78.07
## 6  weight numeric        min      .all 108.0000       108
## 7  weight numeric        p25      .all 204.5000     204.5
## 8  weight numeric     median      .all 258.0000       258
## 9  weight numeric        p75      .all 323.5000     323.5
## 10 weight numeric        max      .all 423.0000       423
## 11 weight numeric       hist      .all       NA  ▃▅▅▇▃▇▂▂
## 12   feed  factor    missing      .all   0.0000         0
## 13   feed  factor   complete      .all  71.0000        71
## 14   feed  factor          n      .all  71.0000        71
## 15   feed  factor   n_unique      .all   6.0000         6
## 16   feed  factor top_counts   soybean  14.0000   soy: 14
## 17   feed  factor top_counts    casein  12.0000   cas: 12
## 18   feed  factor top_counts   linseed  12.0000   lin: 12
## 19   feed  factor top_counts sunflower  12.0000   sun: 12
## 20   feed  factor top_counts  meatmeal  11.0000   mea: 11
## 21   feed  factor top_counts horsebean  10.0000   hor: 10
## 22   feed  factor top_counts      <NA>   0.0000     NA: 0
## 23   feed  factor    ordered      .all   0.0000     FALSE
```


## Compute on the full skim_df object

```r
skim(mtcars) %>% dplyr::filter(stat=="hist")
```

```
## # A tibble: 11 x 6
##      var    type  stat level value formatted
##    <chr>   <chr> <chr> <chr> <dbl>     <chr>
##  1   mpg numeric  hist  .all    NA  ▃▇▇▇▃▂▂▂
##  2   cyl numeric  hist  .all    NA  ▆▁▁▃▁▁▁▇
##  3  disp numeric  hist  .all    NA  ▇▆▁▂▅▃▁▂
##  4    hp numeric  hist  .all    NA  ▃▇▃▅▂▃▁▁
##  5  drat numeric  hist  .all    NA  ▃▇▁▅▇▂▁▁
##  6    wt numeric  hist  .all    NA  ▃▃▃▇▆▁▁▂
##  7  qsec numeric  hist  .all    NA  ▃▂▇▆▃▃▁▁
##  8    vs numeric  hist  .all    NA  ▇▁▁▁▁▁▁▆
##  9    am numeric  hist  .all    NA  ▇▁▁▁▁▁▁▆
## 10  gear numeric  hist  .all    NA  ▇▁▁▆▁▁▁▂
## 11  carb numeric  hist  .all    NA  ▆▇▂▇▁▁▁▁
```

## Works with strings, lists and other column classes!


```r
skim(dplyr::starwars)
```

```
## Skim summary statistics
##  n obs: 87 
##  n variables: 13 
## 
## Variable type: character 
##          var missing complete  n min max empty n_unique
## 1  eye_color       0       87 87   3  13     0       15
## 2     gender       3       84 87   4  13     0        4
## 3 hair_color       5       82 87   4  13     0       12
## 4  homeworld      10       77 87   4  14     0       48
## 5       name       0       87 87   3  21     0       87
## 6 skin_color       0       87 87   3  19     0       31
## 7    species       5       82 87   3  14     0       37
## 
## Variable type: integer 
##      var missing complete  n   mean    sd min p25 median p75 max     hist
## 1 height       6       81 87 174.36 34.77  66 167    180 191 264 ▁▁▁▂▇▃▁▁
## 
## Variable type: list 
##         var missing complete  n n_unique min_length median_length max_length
## 1     films       0       87 87       24          1             1          7
## 2 starships       0       87 87       17          0             0          5
## 3  vehicles       0       87 87       11          0             0          2
## 
## Variable type: numeric 
##          var missing complete  n  mean     sd min  p25 median  p75  max     hist
## 1 birth_year      44       43 87 87.57 154.69   8 35       52 72    896 ▇▁▁▁▁▁▁▁
## 2       mass      28       59 87 97.31 169.46  15 55.6     79 84.5 1358 ▇▁▁▁▁▁▁▁
```


## Specify your own statistics


```r
funs <- list(iqr = IQR,
    quantile = purrr::partial(quantile, probs = .99))
  skim_with(numeric = funs, append = FALSE)
  skim_v(iris$Sepal.Length)
```

```
## # A tibble: 2 x 5
##      type     stat level value formatted
##     <chr>    <chr> <chr> <dbl>     <chr>
## 1 numeric      iqr  .all   1.3       1.3
## 2 numeric quantile   99%   7.7       7.7
```

```r
# Restore defaults
  skim_with_defaults()
```



```r
 funs <- list(iqr = IQR,
    quantile = purrr::partial(quantile, probs = .99))
  skim_with(numeric = funs, append = FALSE)
  skim_v(iris$Sepal.Length)
```

```
## # A tibble: 2 x 5
##      type     stat level value formatted
##     <chr>    <chr> <chr> <dbl>     <chr>
## 1 numeric      iqr  .all   1.3       1.3
## 2 numeric quantile   99%   7.7       7.7
```

```r
# Restore defaults
  skim_with_defaults()
```

## Limitations of current version

Currently the print methods are still in early stages of development. Printing is limited to numeric, character,
and factor data types. Therefore although additional types that are supported by skim() 
and skim_v() will not display with the default printing.  To view these you may view and manipulate the 
skim object.

At the moment in addition to the three types with print support complex, logical, Date, POSIXct, and ts classes
are supported with skim_v methods and the results are in the skim object.

We are also aware that both print.skim and print.data.frame (used for the skim object)  do not handle 
significant digits incorrectly.  

### Windows support for spark histograms

Windows cannot print the spark-histogram characters when printing a data-frame. For example, 
`"▂▅▇"` is printed as `"<U+2582><U+2585><U+2587>"`. This longstanding problem [originates in 
the low-level code](http://r.789695.n4.nabble.com/Unicode-display-problem-with-data-frames-under-Windows-td4707639.html) 
for printing dataframes. One workaround for showing these characters in Windows is to set the CTYPE part of your locale to Chinese/Japanese/Korean with `Sys.setlocale("LC_CTYPE", "Chinese")`. These values do show up by default when printing a data-frame created by `skim()` as a list (`as.list()`) or as a matrix (`as.matrix()`).

### Printing spark histograms and line graphs in knitted documents

Spark-bar and spark-line work in the console but may not work when you knit them to a specific document format.
The same session that produces a correctly rendered HTML document may produce an incorrectly rendered PDF, 
for example. This issue can generally be addressed by changing fonts to one with good building block (for
histogtams) and braille support (for line graphs).  For example, the open font "DejaVu Sans" from 
the `extra font` package supports these.  You may also want to try wrapping your results in `knitr::kable()`.

Displays in documents of different types will vary. For example, one user found that the font 
"Yu Gothic UI Semilight"  produced consistent results for Microsoft Word and Libre Office Write.

## Contributing

We welcome issue reports and pull requests including adding support for different variable classes.
