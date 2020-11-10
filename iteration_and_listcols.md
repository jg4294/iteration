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
    ##  -2.248  -0.597   0.034   0.038   0.751   2.294

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
    ##  [1] 1.363 3.042 3.056 2.226 3.046 2.461 2.467 2.543 2.087 0.983 3.220 3.263
    ## [13] 3.384 4.756 2.690 3.175 3.328 2.946 2.802 1.380
    ## 
    ## $b
    ##  [1]  -5.0734   0.8576  -5.0297   0.6806  -2.2188   4.3910  -8.7006  -0.0831
    ##  [9]   4.4745  -3.5701  -1.9953  -5.4435  -0.3484  -1.2260  -4.6678  -1.5468
    ## [17]  -4.6073  -5.4779   7.6508   0.2205   7.8701   0.5534   1.1681  -5.8138
    ## [25]   5.8563  -7.3667   2.7032 -14.8552   2.7665  -3.0103
    ## 
    ## $c
    ##  [1]  9.84 10.02  9.91  9.93 10.06 10.40 10.23 10.07 10.11  9.97  9.79  9.87
    ## [13] 10.36 10.09  9.99 10.24  9.99 10.21  9.81  9.99  9.76  9.90 10.19  9.92
    ## [25] 10.08 10.15  9.88 10.02  9.77 10.01 10.21 10.36  9.76  9.85  9.75  9.77
    ## [37]  9.87 10.05 10.11  9.59
    ## 
    ## $d
    ##  [1] -3.49 -3.37 -3.31 -4.27 -4.35 -3.02 -3.30 -1.85 -4.18 -2.32 -2.54 -4.42
    ## [13] -2.34 -1.60 -1.53 -3.08 -2.90 -3.40 -1.01 -2.40

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
    ## 1  2.71 0.844

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.39  5.01

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.188

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.93 0.983

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
