Writing fucntion
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec) # z-scores
```

    ##  [1]  1.6758  2.2498 -0.2037 -1.4106 -0.7381 -0.3152 -0.6085 -1.2191 -1.1526
    ## [10] -0.8457  0.2852 -0.4275  0.1305 -0.5466 -0.2076  1.3624  0.3422 -0.3612
    ## [19] -0.7669  1.3455  0.5051  0.4972  0.6831 -0.9181  1.6239 -0.0288  1.1210
    ## [28] -0.2906  0.1226 -1.9035

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

    ##  [1]  1.6758  2.2498 -0.2037 -1.4106 -0.7381 -0.3152 -0.6085 -1.2191 -1.1526
    ## [10] -0.8457  0.2852 -0.4275  0.1305 -0.5466 -0.2076  1.3624  0.3422 -0.3612
    ## [19] -0.7669  1.3455  0.5051  0.4972  0.6831 -0.9181  1.6239 -0.0288  1.1210
    ## [28] -0.2906  0.1226 -1.9035

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
    ## 1  5.95  3.23

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
    ## 1  3.90  2.79

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
    ## 1  5.54  2.77

``` r
sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.13  2.91

``` r
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.81  3.55

Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews_page1 = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)

reviews_page1
```

    ## # A tibble: 10 x 3
    ##    title                  stars text                                            
    ##    <chr>                  <dbl> <chr>                                           
    ##  1 Vote for Pedro!            5 Just watch the movie. Gosh!                     
    ##  2 Just watch the freaki~     5 Its a great movie, gosh!!                       
    ##  3 Great Value                5 Great Value                                     
    ##  4 I LOVE THIS MOVIE          5 THIS MOVIE IS SO FUNNY ONE OF MY FAVORITES      
    ##  5 Don't you wish you co~     5 Watch it 100 times. Never. Gets. Old.           
    ##  6 Stupid, but very funn~     5 If you like stupidly funny '90s teenage movies ~
    ##  7 The beat                   5 The best                                        
    ##  8 Hilarious                  5 Super funny! Loved the online rental.           
    ##  9 Love this movie            5 We love this product.  It came in a timely mann~
    ## 10 Entertaining, limited~     4 Entertainment level gets a 5 star but having pr~

What about the next page of reviews….

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews_page2 = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
reviews_page2
```

    ## # A tibble: 10 x 3
    ##    title                               stars text                               
    ##    <chr>                               <dbl> <chr>                              
    ##  1 "Boo"                                   1 "We rented this movie because our ~
    ##  2 "Movie is still silly fun....amazo~     1 "We are getting really frustrated ~
    ##  3 "Brilliant and awkwardly funny."        5 "I've watched this movie repeatedl~
    ##  4 "Great purchase price for great mo~     5 "Great movie and real good digital~
    ##  5 "Movie for memories"                    5 "I've been looking for this movie ~
    ##  6 "Love!"                                 5 "Love this movie. Great quality"   
    ##  7 "Hilarious!"                            5 "Such a funny movie, definitely br~
    ##  8 "napoleon dynamite"                     5 "cool movie"                       
    ##  9 "Top 5"                                 5 "Best MOVIE ever! Funny one liners~
    ## 10 "\U0001f44d"                            5 "Exactly as described and came on ~

Let’s turn that codeinto a function

``` r
read_page_reviews = function(url){
  
  html = read_html(url)

  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()

  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()

  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()

  reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )
  
  reviews

}
```

Let me try my function:

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"


read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                  stars text                                            
    ##    <chr>                  <dbl> <chr>                                           
    ##  1 Vote for Pedro!            5 Just watch the movie. Gosh!                     
    ##  2 Just watch the freaki~     5 Its a great movie, gosh!!                       
    ##  3 Great Value                5 Great Value                                     
    ##  4 I LOVE THIS MOVIE          5 THIS MOVIE IS SO FUNNY ONE OF MY FAVORITES      
    ##  5 Don't you wish you co~     5 Watch it 100 times. Never. Gets. Old.           
    ##  6 Stupid, but very funn~     5 If you like stupidly funny '90s teenage movies ~
    ##  7 The beat                   5 The best                                        
    ##  8 Hilarious                  5 Super funny! Loved the online rental.           
    ##  9 Love this movie            5 We love this product.  It came in a timely mann~
    ## 10 Entertaining, limited~     4 Entertainment level gets a 5 star but having pr~

Let’s read a few pages of reviews:

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

dynamite_urls[1]
```

    ## [1] "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

``` r
all_reviews =  bind_rows(
  read_page_reviews(dynamite_urls[1]),
  read_page_reviews(dynamite_urls[2]),
  read_page_reviews(dynamite_urls[3]),
  read_page_reviews(dynamite_urls[4]),
  read_page_reviews(dynamite_urls[5])
)
```

## Mean scoping example

``` r
f = function(x) {
  
  z = x + y
  z
}

x = 1
y = 2

f(x = y) #4
```

    ## [1] 4

## Function as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
}

x_vec = rnorm(100,3,7)

mean(x_vec)
```

    ## [1] 3.47

``` r
median(x_vec)
```

    ## [1] 3.04

``` r
my_summary(x_vec, sd) 
```

    ## [1] 7.32

``` r
#my_summary(x_vec, median)
#my_summary(x_vec, IQR)
#my_summary(x_vec, mean)
```
