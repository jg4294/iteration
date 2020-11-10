Writing fucntion
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec) # z-scores
```

    ##  [1] -1.2730  1.3551 -0.1842  0.2376  0.0511 -1.1626 -0.9718  1.0458 -0.5388
    ## [10]  0.9373 -0.6334  0.7105 -1.3405 -0.4533 -0.0518  0.9151  1.2141 -0.7673
    ## [19]  0.1402  1.7784  1.0582 -0.8365  1.3809 -1.1224 -0.2421 -1.1595  0.4642
    ## [28]  1.0744 -1.9997  0.3739

I want a function to compute the z-sxores

``` r
z_scores = function(x) {
  #update my function:
   if (!is.numeric(x)) {
     stop("Input must be numeric")
   }
  
  if (length(x) < 3) {
    stop("Input must have atleast three numbers") 
  }
 
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}
z_scores(x_vec)
```

    ##  [1] -1.2730  1.3551 -0.1842  0.2376  0.0511 -1.1626 -0.9718  1.0458 -0.5388
    ## [10]  0.9373 -0.6334  0.7105 -1.3405 -0.4533 -0.0518  0.9151  1.2141 -0.7673
    ## [19]  0.1402  1.7784  1.0582 -0.8365  1.3809 -1.1224 -0.2421 -1.1595  0.4642
    ## [28]  1.0744 -1.9997  0.3739

Try my function o some other things: these should give errors.

``` r
z_scores(3) # NA, need more numbers to calculate the standard deviation
```

    ## Error in z_scores(3): Input must have atleast three numbers

``` r
# Error in z_scores(3) : Input must have atleast three numbers

z_scores("my name is Jeff") #error, can't take a mean of a character
```

    ## Error in z_scores("my name is Jeff"): Input must be numeric

``` r
# Error in z_scores("my name is Jeff") : Input must be numeric

data(mtcars) # a build-in dataset in R, it's about cars and miles/gallon
z_scores(mtcars) #error, can't coerce to type double
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
# Error in z_scores(mtcars) : Input must be numeric

z_scores(c(TRUE, TRUE, FALSE, TRUE)) # 0.5, 0.5, 1.5, 0.5
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric

``` r
# Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)) : Input must be numeric
```
