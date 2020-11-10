Iteration and Listcols
================

## lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##  -2.611  -0.567  -0.141  -0.061   0.588   2.356

``` r
# gives the same results: 5 6 7 8
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]]
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]][1:3]
```

    ## [1] 5 6 7

## `for` loop

Create a new list

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )

is.list(list_norm)
```

    ## [1] TRUE

``` r
list_norm
```

    ## $a
    ##  [1] 4.47 3.27 4.11 1.99 3.11 2.65 2.09 2.56 3.20 3.40 3.62 3.92 2.77 2.68 2.69
    ## [16] 2.28 3.53 3.42 3.05 4.05
    ## 
    ## $b
    ##  [1] -8.0690  0.0891 -2.1580 -5.9003 -5.6207 -2.3872 -4.1366 -1.8408 -7.9920
    ## [10] -3.3879  0.0571 -2.8477  6.7033 -0.8484  1.5214  0.7228  2.6309  0.8087
    ## [19]  6.9689 -2.4907 -0.5372  3.8698  0.9528  1.3169  5.4704  5.4459  8.3336
    ## [28] -5.1201  2.9724  0.4346
    ## 
    ## $c
    ##  [1]  9.83 10.02 10.06  9.88  9.90  9.86  9.94 10.02 10.07 10.43 10.15  9.78
    ## [13] 10.19  9.94 10.21  9.97 10.35  9.96 10.02 10.13 10.27 10.09 10.04  9.98
    ## [25]  9.69  9.92  9.85  9.38  9.56  9.90 10.20 10.16 10.29 10.08  9.85 10.13
    ## [37] 10.35 10.47  9.86 10.36
    ## 
    ## $d
    ##  [1] -4.236 -1.787 -2.795 -2.005 -3.450 -2.454 -4.166 -1.346 -3.764 -3.213
    ## [11] -1.967 -4.066 -2.672 -0.801 -2.577 -1.764 -3.483 -2.676 -3.501 -3.245

Pause and get my old function.

``` r
mean_and_sd = function(x) {

   if (!is.numeric(x)) {
     stop("Input must be numeric")
   }
  
  if (length(x) < 3) {
    stop("Input must have atleast three numbers") 
  }
 
  # get both mean and standard deviation 
  mean_x = mean(x)
  sd_x = sd(x)
  
  # return mean and standard deviaiton as a data frame:
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.14 0.688

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.168  4.31

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.228

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.80 0.972

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
output[[i]] = mean_and_sd(list_norm[[i]])

}
```

## Let’s try map\!

``` r
output = map(list_norm, mean_and_sd)
```

What if you want a different functions…?

``` r
output = map(list_norm, IQR)
# map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median)
```

``` r
output = map_df(list_norm, mean_and_sd)
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List Columns\!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 4.47 3.27 4.11 1.99 3.11 2.65 2.09 2.56 3.20 3.40 3.62 3.92 2.77 2.68 2.69
    ## [16] 2.28 3.53 3.42 3.05 4.05
    ## 
    ## $b
    ##  [1] -8.0690  0.0891 -2.1580 -5.9003 -5.6207 -2.3872 -4.1366 -1.8408 -7.9920
    ## [10] -3.3879  0.0571 -2.8477  6.7033 -0.8484  1.5214  0.7228  2.6309  0.8087
    ## [19]  6.9689 -2.4907 -0.5372  3.8698  0.9528  1.3169  5.4704  5.4459  8.3336
    ## [28] -5.1201  2.9724  0.4346
    ## 
    ## $c
    ##  [1]  9.83 10.02 10.06  9.88  9.90  9.86  9.94 10.02 10.07 10.43 10.15  9.78
    ## [13] 10.19  9.94 10.21  9.97 10.35  9.96 10.02 10.13 10.27 10.09 10.04  9.98
    ## [25]  9.69  9.92  9.85  9.38  9.56  9.90 10.20 10.16 10.29 10.08  9.85 10.13
    ## [37] 10.35 10.47  9.86 10.36
    ## 
    ## $d
    ##  [1] -4.236 -1.787 -2.795 -2.005 -3.450 -2.454 -4.166 -1.346 -3.764 -3.213
    ## [11] -1.967 -4.066 -2.672 -0.801 -2.577 -1.764 -3.483 -2.676 -3.501 -3.245

``` r
listcol_df %>%
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations

``` r
listcol_df$samp
```

    ## $a
    ##  [1] 4.47 3.27 4.11 1.99 3.11 2.65 2.09 2.56 3.20 3.40 3.62 3.92 2.77 2.68 2.69
    ## [16] 2.28 3.53 3.42 3.05 4.05
    ## 
    ## $b
    ##  [1] -8.0690  0.0891 -2.1580 -5.9003 -5.6207 -2.3872 -4.1366 -1.8408 -7.9920
    ## [10] -3.3879  0.0571 -2.8477  6.7033 -0.8484  1.5214  0.7228  2.6309  0.8087
    ## [19]  6.9689 -2.4907 -0.5372  3.8698  0.9528  1.3169  5.4704  5.4459  8.3336
    ## [28] -5.1201  2.9724  0.4346
    ## 
    ## $c
    ##  [1]  9.83 10.02 10.06  9.88  9.90  9.86  9.94 10.02 10.07 10.43 10.15  9.78
    ## [13] 10.19  9.94 10.21  9.97 10.35  9.96 10.02 10.13 10.27 10.09 10.04  9.98
    ## [25]  9.69  9.92  9.85  9.38  9.56  9.90 10.20 10.16 10.29 10.08  9.85 10.13
    ## [37] 10.35 10.47  9.86 10.36
    ## 
    ## $d
    ##  [1] -4.236 -1.787 -2.795 -2.005 -3.450 -2.454 -4.166 -1.346 -3.764 -3.213
    ## [11] -1.967 -4.066 -2.672 -0.801 -2.577 -1.764 -3.483 -2.676 -3.501 -3.245

``` r
listcol_df$samp[[1]] # get the first element of my list column
```

    ##  [1] 4.47 3.27 4.11 1.99 3.11 2.65 2.09 2.56 3.20 3.40 3.62 3.92 2.77 2.68 2.69
    ## [16] 2.28 3.53 3.42 3.05 4.05

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.14 0.688

can I just map????

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.14 0.688
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.168  4.31
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.228
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.80 0.972

So… can I add a list column?

``` r
listcol_df =
  listcol_df %>%
  mutate(summary = map(samp, mean_and_sd),
         medians = map_dbl(samp, median)) 
```
