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
    ##  -2.774  -0.893  -0.033  -0.103   0.674   2.577

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
    ##  [1] 4.43 1.07 2.33 1.78 1.18 3.78 2.56 2.43 3.06 4.50 1.91 3.63 3.26 4.36 3.33
    ## [16] 2.81 3.34 3.11 2.44 2.70
    ## 
    ## $b
    ##  [1]  -1.6716   0.2562  10.1277   9.0001  -0.6758   3.6898   7.0889  -1.7847
    ##  [9]  -4.2338   6.0950   8.8273   9.3911   3.9437   7.8752  -6.9540  -0.0896
    ## [17]  -2.3491  -5.1753  -4.2624   2.1864   1.4042   5.2435  -1.6279   3.5387
    ## [25]   2.6742   5.3487  -4.2560  -1.4633  -4.8318 -10.1538
    ## 
    ## $c
    ##  [1]  9.79 10.07 10.30  9.83 10.03  9.88  9.85  9.50 10.29  9.81 10.08  9.91
    ## [13] 10.06 10.31  9.98  9.88 10.18  9.85 10.29 10.08  9.92 10.58 10.18 10.31
    ## [25] 10.00  9.85  9.87  9.98  9.89 10.07  9.93 10.14  9.78  9.73  9.78  9.67
    ## [37]  9.91  9.77 10.08 10.09
    ## 
    ## $d
    ##  [1] -3.50 -3.44 -3.90 -2.29 -1.97 -4.80 -2.39 -5.04 -2.24 -3.36 -3.85 -2.22
    ## [13] -2.44 -1.90 -4.15 -2.46 -4.47 -3.90 -2.88 -2.40

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
    ## 1  2.90 0.982

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.24  5.37

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.212

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.18 0.986

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
output[[i]] = mean_and_sd(list_norm[[i]])

}
```
