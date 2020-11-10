Writing fucntion
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec) # z-scores
```

    ##  [1] -0.6631 -0.4608  0.6111 -0.3591 -0.0856 -1.3510 -0.3743 -0.1378  0.8538
    ## [10]  0.6876  0.2658  0.7393  1.2772  1.7224 -0.3440  0.3404 -0.0509 -2.7539
    ## [19] -0.4192  1.3120  0.8147  1.5899 -0.4455  0.6647  0.3345 -1.2720  0.3153
    ## [28] -1.8303 -0.8273 -0.1540

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

    ##  [1] -0.6631 -0.4608  0.6111 -0.3591 -0.0856 -1.3510 -0.3743 -0.1378  0.8538
    ## [10]  0.6876  0.2658  0.7393  1.2772  1.7224 -0.3440  0.3404 -0.0509 -2.7539
    ## [19] -0.4192  1.3120  0.8147  1.5899 -0.4455  0.6647  0.3345 -1.2720  0.3153
    ## [28] -1.8303 -0.8273 -0.1540

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

## Multiple outputs

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
  
  # returns the value of mean and standard deviation by list()
  # list(
  #   mean = mean_x,
  #   sd = sd_x
  # )
  
  # return mean and standard deviaiton as a data frame:
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
  
  
}
```

Check that function works.

``` r
#x_vec = rnorm(100, mean=3, sd=4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.64  3.26

## Multiple inputs

I’d like to do this with a function.

``` r
sim_data = 
  tibble(
    x = rnorm(100, mean = 4, sd = 3)
  )

sim_data %>%
  summarize(
   mean = mean(x),
   sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.12  2.96

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4) {
  
  sim_data = 
  tibble(
    x = rnorm(n = samp_size, mean = mu, sd = sigma)
  )

sim_data %>%
  summarize(
   mean = mean(x),
   sd = sd(x)
  )
}

#gives the same outputs: overwrite the mu=3, sigma=4 in the parameters
sim_mean_sd(100,6,3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.51  3.16

``` r
sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.76  2.60

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.71  3.11
