Introduction to the Normal distribution
================
Erika Duan
2021-09-19

-   [Normal distribution](#normal-distribution)
    -   [Standard normal distribution](#standard-normal-distribution)
    -   [Bivariate normal distribution](#bivariate-normal-distribution)
    -   [Multivariate normal
        distribution](#multivariate-normal-distribution)
-   [Other resources](#other-resources)

``` r
# Load required packages -------------------------------------------------------  
if (!require("pacman")) install.packages("pacman")
pacman::p_load(here,  
               tidyverse,
               patchwork,
               mnormt)   
```

# Normal distribution

The normal distribution, or Gaussian distribution, is frequently
observed in nature and is characterised by the following properties:

-   It is a symmetrical continuous distribution where
    ![-\\infty &lt; x &lt; \\infty](https://latex.codecogs.com/png.latex?-%5Cinfty%20%3C%20x%20%3C%20%5Cinfty "-\infty < x < \infty").  
-   Its mean, median and mode are identical due to its symmetrical
    distribution.  
-   Its probability density function contains two parameters; the mean
    i.e. ![\\mu](https://latex.codecogs.com/png.latex?%5Cmu "\mu") and
    the standard deviation
    i.e. ![\\sigma](https://latex.codecogs.com/png.latex?%5Csigma "\sigma")
    where
    ![-\\infty &lt; \\mu &lt; \\infty](https://latex.codecogs.com/png.latex?-%5Cinfty%20%3C%20%5Cmu%20%3C%20%5Cinfty "-\infty < \mu < \infty")
    and
    ![\\sigma &gt; 0](https://latex.codecogs.com/png.latex?%5Csigma%20%3E%200 "\sigma > 0").  
-   Its probability density function is described by the equation
    below:  
    ![f(x) = \\frac{1}{\\sqrt{2 \\pi \\sigma^2}} \\times e^{\\frac{-(x - \\mu) ^2}{2\\sigma^2}}](https://latex.codecogs.com/png.latex?f%28x%29%20%3D%20%5Cfrac%7B1%7D%7B%5Csqrt%7B2%20%5Cpi%20%5Csigma%5E2%7D%7D%20%5Ctimes%20e%5E%7B%5Cfrac%7B-%28x%20-%20%5Cmu%29%20%5E2%7D%7B2%5Csigma%5E2%7D%7D "f(x) = \frac{1}{\sqrt{2 \pi \sigma^2}} \times e^{\frac{-(x - \mu) ^2}{2\sigma^2}}").

Imagine that the height of tomato plants in a field is normally
distributed. It is known that the average tomato plant height is 140 cm,
with a standard deviation of 16 cm.

-   What would the distribution of tomato plant height look like if the
    height of 100 plants was randomly measured?
-   What would the distribution of tomato plant height look like if the
    height of 10, 100 or 1000 plants was randomly measured?

``` r
# Simulate tomato plant heights from a known normal distribution ---------------
set.seed(111)  

plant_height <- tibble(x = seq(1, 100),
                       height = rnorm(100, mean = 140, sd = 16))  

# Plot tomato plant heights ----------------------------------------------------
measurements <- plant_height %>%
  ggplot(aes(x = x, y = height)) + 
  geom_point(aes(x = x, y = height),
             shape = 21, fill = "tomato") + 
  labs(x = "X",
       y = "Plant height (cm)",
       title = "Sample plant heights") + 
  theme_minimal() +
  theme(panel.grid = element_blank(),
        panel.border = element_rect(fill = NA, colour = "black"),
        plot.title = element_text(hjust = 0.5)) 

density <- plant_height %>%
  ggplot(aes(y = height)) + 
  geom_density(aes(y = height)) + 
  labs(x = "Density",
       y = "Plant height (cm)",
       title = "Density of sample plant heights") + 
  theme_minimal() +
  theme(panel.grid = element_blank(),
        panel.border = element_rect(fill = NA, colour = "black"),
        plot.title = element_text(hjust = 0.5)) 

(measurements + density)
```

<img src="st-normal_distribution_files/figure-gfm/plot norm dist-1.png" style="display: block; margin: auto;" />

``` r
# Simulate tomato plant heights from a known normal distribution ---------------
# Simulate for n = 10, 100 or 1000 samples 
set.seed(111)

sample_10 <- tibble(x = seq(1, 10), 
                    height = rnorm(10, mean = 140, sd = 16))  

sample_100 <- tibble(x = seq(1, 100), 
                     height = rnorm(100, mean = 140, sd = 16))  

sample_1000 <- tibble(x = seq(1, 1000),
                      height = rnorm(1000, mean = 140, sd = 16))  

# Create plotting function -----------------------------------------------------
plot_density <- function(df, value) {
  value <- enquo(value)
  
  plot <- df %>%
    ggplot(aes(x = !! value)) + 
    geom_density(aes(x = !! value)) + 
    geom_vline(xintercept = 140, colour = "tomato", linetype = "dotted") +
    scale_x_continuous(limits = c(70, 210)) +
    labs(x = "Plant height (cm)",
         y = "Density",
         title = paste0("Density of ", nrow(df), " sample plant heights")) + 
    theme_minimal() +
    theme(panel.grid = element_blank(),
          panel.border = element_rect(fill = NA, colour = "black"),
          plot.title = element_text(hjust = 0.5))
  
  return(plot)
}

# Plot sample density plots ----------------------------------------------------
(plot_density(sample_10, height) / plot_density(sample_100, height) / plot_density(sample_1000, height))
```

<img src="st-normal_distribution_files/figure-gfm/plot norm dist for different n-1.png" style="display: block; margin: auto;" />

-   What would the distribution of sample means
    i.e. ![\\bar x](https://latex.codecogs.com/png.latex?%5Cbar%20x "\bar x"),
    look like if 100 tomato plants were randomly measured from 1000
    random locations?

``` r
# Simulate 1000 iterations of n = 100 tomato plants ----------------------------
# set.seed(111) cannot be used as it fixes the sample values from each iteration 

sample_means <- vector("double", length = 1000L)
for (i in 1:1000) {
  plot <- rnorm(100, mean = 140, sd = 16)
  sample_means[[i]] <- mean(plot)    
}

# Plot distribution of 1000 sample means ---------------------------------------
sample_means %>%
  tibble(height = .) %>% # Convert to tibble for plotting
  ggplot(aes(x = height)) +
  geom_histogram(binwidth = 1, fill = "white", colour = "black") +
  geom_vline(xintercept = 140, colour = "tomato", linetype = "dotted") + 
  labs(x = "Plant height mean (cm)",
       y = "Count") + 
  theme_minimal() +
  theme(panel.grid = element_blank(),
        panel.border = element_rect(fill = NA, colour = "black"),
        plot.title = element_text(hjust = 0.5))  
```

<img src="st-normal_distribution_files/figure-gfm/plot CLT-1.png" width="50%" style="display: block; margin: auto;" />

The phenomenon above is an example of the [central limit
theorem](https://statisticsbyjim.com/basics/central-limit-theorem/),
which states that the sampling distribution of the mean for any
distribution approximates the normal distribution as the sample size
increases.

The normal distribution has a well-know probability density function
where:  
+ \~68% of values fall within one standard deviation of the mean.  
+ \~95.5% of values fall within two standard deviations of the mean.  
+ \~99.7% of values fall within three standard deviations of the mean.

``` r
# Calculate probability from probability density function ----------------------
# Store f(x) as a normal distribution with mean 140 and sd 16      
funs_x_fx <- function(x) (1/ (sqrt(2 * pi * 16 ^ 2)) * exp(-((x - 140) ^ 2) / (2 * 16 ^ 2)))

# Probability of a value falling within 1 sd of the mean -----------------------  
integrate(funs_x_fx, lower = 140 - 16, upper = 140 + 16)
#> 0.6826895 with absolute error < 7.6e-15

# Probability of a value falling within 2 sd of the mean -----------------------  
integrate(funs_x_fx, lower = 140 - (2 * 16), upper = 140 + (2 * 16))
#> 0.9544997 with absolute error < 1.8e-11 

# Probability of a value falling within 1 sd of the mean -----------------------  
integrate(funs_x_fx, lower = 140 - (3 * 16), upper = 140 + (3 * 16))
#> 0.9973002 with absolute error < 9.3e-07  
```

## Standard normal distribution

The standard normal distribution is a special form of the normal
distribution with a mean of 0 and standard deviation of 1.  
Its probability density function is therefore described by the equation
below:  

![f(x) = \\frac{1}{\\sqrt{2 \\pi}} \\times e^{\\frac{-x^2}{2}}](https://latex.codecogs.com/png.latex?f%28x%29%20%3D%20%5Cfrac%7B1%7D%7B%5Csqrt%7B2%20%5Cpi%7D%7D%20%5Ctimes%20e%5E%7B%5Cfrac%7B-x%5E2%7D%7B2%7D%7D "f(x) = \frac{1}{\sqrt{2 \pi}} \times e^{\frac{-x^2}{2}}")

The linear transformation below is used to standardise randomly
distribution variables from
![X \\sim N(\\mu, \\sigma^2)](https://latex.codecogs.com/png.latex?X%20%5Csim%20N%28%5Cmu%2C%20%5Csigma%5E2%29 "X \sim N(\mu, \sigma^2)")
into
![Z \\sim N(0, 1)](https://latex.codecogs.com/png.latex?Z%20%5Csim%20N%280%2C%201%29 "Z \sim N(0, 1)").  
![Z = \\frac{X - \\mu}{\\sigma}](https://latex.codecogs.com/png.latex?Z%20%3D%20%5Cfrac%7BX%20-%20%5Cmu%7D%7B%5Csigma%7D "Z = \frac{X - \mu}{\sigma}")

Let’s return to our simulation of 100 tomato plant heights knowing that
the average tomato plant height is 140 cm, with a standard deviation of
16 cm.

-   What would the distribution look like if we standardised tomato
    plant height from a sample of 100 plants measured?

``` r
# Standardise normal distribution of tomato plant heights ----------------------
mean(plant_height$height)
#> [1] 139.7949

sd(plant_height$height) # In R, sd() is always calculated with n - 1 df
#> [1] 17.13368

plant_height <- plant_height %>%
  mutate(std_height = ((height - mean(plant_height$height))/ sd(plant_height$height)))  
```

``` r
# Plot distribution of standardised tomato plant heights -----------------------
plant_height %>% 
  ggplot(aes(x = std_height)) +
  geom_histogram(binwidth = 0.4, fill = "lavender", colour = "black") +
  scale_x_continuous(limits = c(-3, 3)) + 
  labs(x = "Standardised plant height (cm)",
       y = "Count") + 
  theme_minimal() +
  theme(panel.grid = element_blank(),
        panel.border = element_rect(fill = NA, colour = "black"),
        plot.title = element_text(hjust = 0.5)) 
```

<img src="st-normal_distribution_files/figure-gfm/plot standard norm dist-1.png" width="70%" style="display: block; margin: auto;" />

## Bivariate normal distribution

The bivariate normal distribution is the simplest example of a
multivariate normal distribution where two random variables are both
normally distributed and their [joint probability
distribution](https://en.wikipedia.org/wiki/Joint_probability_distribution)
is therefore also characterised by a normal distribution.

<img src="../../figures/st-normal_distribution-bivariate_normal_distribution.jpg" width="80%" style="display: block; margin: auto;" />

The probability density function of the bivariate normal distribution is
represented by the following equation:

![f(x\_1, x\_2) = \\frac{1}{2\\pi\|\\Sigma\|^{1/2}}e \\begin{Bmatrix} \\frac{-1}{2} \\begin{pmatrix} x\_1 - u\_1 \\\\ x\_2 - u\_2 \\end{pmatrix}^\\intercal \\Sigma^{-1} \\begin{pmatrix} x\_1 - u\_1 \\\\ x\_2 - u\_2 \\end{pmatrix}\\end{Bmatrix}](https://latex.codecogs.com/png.latex?f%28x_1%2C%20x_2%29%20%3D%20%5Cfrac%7B1%7D%7B2%5Cpi%7C%5CSigma%7C%5E%7B1%2F2%7D%7De%20%5Cbegin%7BBmatrix%7D%20%5Cfrac%7B-1%7D%7B2%7D%20%5Cbegin%7Bpmatrix%7D%20x_1%20-%20u_1%20%5C%5C%20x_2%20-%20u_2%20%5Cend%7Bpmatrix%7D%5E%5Cintercal%20%5CSigma%5E%7B-1%7D%20%5Cbegin%7Bpmatrix%7D%20x_1%20-%20u_1%20%5C%5C%20x_2%20-%20u_2%20%5Cend%7Bpmatrix%7D%5Cend%7BBmatrix%7D "f(x_1, x_2) = \frac{1}{2\pi|\Sigma|^{1/2}}e \begin{Bmatrix} \frac{-1}{2} \begin{pmatrix} x_1 - u_1 \\ x_2 - u_2 \end{pmatrix}^\intercal \Sigma^{-1} \begin{pmatrix} x_1 - u_1 \\ x_2 - u_2 \end{pmatrix}\end{Bmatrix}")

**Note:** ![x\_1](https://latex.codecogs.com/png.latex?x_1 "x_1") and
![x\_2](https://latex.codecogs.com/png.latex?x_2 "x_2") should be
thought of as two different column vectors of random values.

``` r
# Create a bivariate normal distribution ---------------------------------------
# Create two independent variables ~ N(0,1)  
x1 <- seq(-5, 5, length = 100L) 
x2 <- seq(-5, 5, length = 100L)

mean <- c(0, 0) # Both variables have mean = 0  

var_cov <- matrix(c(1, 0, 0, 1), # Variables are not correlated with each other  
                  nrow = 2,
                  byrow = TRUE)

# Calculate joint probability density function ---------------------------------
f_x <- function(x1, x2) dmnorm(cbind(x1, x2), mean, var_cov)
z <- outer(x1, x2, f_x) # Value z represents the density at (x1, x2)  

# Plot 2D contour plot of joint probability density function -------------------
contour(x1, x2, z,
        xlab = "X1", ylab = "X2", 
        main = "Joint probability density function (2D contour plot)")  
```

<img src="st-normal_distribution_files/figure-gfm/unnamed-chunk-1-1.png" style="display: block; margin: auto;" />

``` r
# plot 3D curve of joint probability density function --------------------------
persp(x1, x2, z, theta = 30, phi = 30, 
      ltheta = 15, shade = 0.1, col = "lavender", expand = 0.5, r = 1, ticktype = "detailed",
      xlab = "X1", ylab = "X2", zlab = "Probability density",
      main = "Joint probability density function (3D curve)")
```

<img src="st-normal_distribution_files/figure-gfm/unnamed-chunk-1-2.png" style="display: block; margin: auto;" />

``` r
# Create a bivariate normal distribution ---------------------------------------
# Create two highly correlated variables ~ N(0,1)  
x1 <- seq(-5, 5, length = 100L) 
x2 <- seq(-5, 5, length = 100L)

mean <- c(0, 0) # Both variables have mean = 0  

var_cov <- matrix(c(1, 0.8, 0.8, 1), # Highly correlated variables i.e. r = 0.8  
                  nrow = 2,
                  byrow = TRUE)

# Calculate joint probability density function ---------------------------------
f_x <- function(x1, x2) dmnorm(cbind(x1, x2), mean, var_cov)
z <- outer(x1, x2, f_x) # Value z represents the density at (x1, x2)  

# Plot 2D contour plot of joint probability density function -------------------
contour(x1, x2, z,
        xlab = "X1", ylab = "X2", 
        main = "Joint probability density function (2D contour plot)")  
```

<img src="st-normal_distribution_files/figure-gfm/unnamed-chunk-2-1.png" style="display: block; margin: auto;" />

``` r
# plot 3D curve of joint probability density function --------------------------
persp(x1, x2, z, theta = 30, phi = 30, 
      ltheta = 15, shade = 0.1, col = "lavender", expand = 0.5, r = 1, ticktype = "detailed",
      xlab = "X1", ylab = "X2", zlab = "Probability density",
      main = "Joint probability density function (3D curve)")  
```

<img src="st-normal_distribution_files/figure-gfm/unnamed-chunk-2-2.png" style="display: block; margin: auto;" />

## Multivariate normal distribution

We assume that the response variable in a multiple linear regression
model, commonly denoted as
![{\\boldsymbol{Y}}](https://latex.codecogs.com/png.latex?%7B%5Cboldsymbol%7BY%7D%7D "{\boldsymbol{Y}}"),
has a multivariate normal distribution where
![E({\\boldsymbol{Y}}) = \[\\mu\_1, \\mu\_2, \\cdots, \\mu\_n\]^\\intercal](https://latex.codecogs.com/png.latex?E%28%7B%5Cboldsymbol%7BY%7D%7D%29%20%3D%20%5B%5Cmu_1%2C%20%5Cmu_2%2C%20%5Ccdots%2C%20%5Cmu_n%5D%5E%5Cintercal "E({\boldsymbol{Y}}) = [\mu_1, \mu_2, \cdots, \mu_n]^\intercal")
and is a
![1 \\times n](https://latex.codecogs.com/png.latex?1%20%5Ctimes%20n "1 \times n")
column vector. The joint probability distribution of
![{\\boldsymbol{Y}}](https://latex.codecogs.com/png.latex?%7B%5Cboldsymbol%7BY%7D%7D "{\boldsymbol{Y}}")
is therefore a function which spans the
![n+1](https://latex.codecogs.com/png.latex?n%2B1 "n+1") dimension space
and describes the probability of any combination of
![\[Y\_1, Y\_2. \\cdots, Y\_n\]^\\intercal](https://latex.codecogs.com/png.latex?%5BY_1%2C%20Y_2.%20%5Ccdots%2C%20Y_n%5D%5E%5Cintercal "[Y_1, Y_2. \cdots, Y_n]^\intercal")
values in being observed.

If
![\\boldsymbol{Y} \\sim N(\\boldsymbol{\\mu, \\boldsymbol{\\Sigma}})](https://latex.codecogs.com/png.latex?%5Cboldsymbol%7BY%7D%20%5Csim%20N%28%5Cboldsymbol%7B%5Cmu%2C%20%5Cboldsymbol%7B%5CSigma%7D%7D%29 "\boldsymbol{Y} \sim N(\boldsymbol{\mu, \boldsymbol{\Sigma}})"),
it contains the following properties:

-   ![E(\\boldsymbol{Y}) = \\boldsymbol \\mu = \[\\mu\_1, \\mu\_2, \\cdots, \\mu\_n\]^\\intercal](https://latex.codecogs.com/png.latex?E%28%5Cboldsymbol%7BY%7D%29%20%3D%20%5Cboldsymbol%20%5Cmu%20%3D%20%5B%5Cmu_1%2C%20%5Cmu_2%2C%20%5Ccdots%2C%20%5Cmu_n%5D%5E%5Cintercal "E(\boldsymbol{Y}) = \boldsymbol \mu = [\mu_1, \mu_2, \cdots, \mu_n]^\intercal")  
-   ![Cov(\\boldsymbol{Y}) = \\boldsymbol{\\Sigma} = \\begin{matrix} \\sigma\_{11} & \\cdots & \\sigma\_{1n} \\\\ \\vdots & \\ddots & \\vdots \\\\ \\sigma\_{n1} & \\cdots & \\sigma\_{nn} \\end{matrix}](https://latex.codecogs.com/png.latex?Cov%28%5Cboldsymbol%7BY%7D%29%20%3D%20%5Cboldsymbol%7B%5CSigma%7D%20%3D%20%5Cbegin%7Bmatrix%7D%20%5Csigma_%7B11%7D%20%26%20%5Ccdots%20%26%20%5Csigma_%7B1n%7D%20%5C%5C%20%5Cvdots%20%26%20%5Cddots%20%26%20%5Cvdots%20%5C%5C%20%5Csigma_%7Bn1%7D%20%26%20%5Ccdots%20%26%20%5Csigma_%7Bnn%7D%20%5Cend%7Bmatrix%7D "Cov(\boldsymbol{Y}) = \boldsymbol{\Sigma} = \begin{matrix} \sigma_{11} & \cdots & \sigma_{1n} \\ \vdots & \ddots & \vdots \\ \sigma_{n1} & \cdots & \sigma_{nn} \end{matrix}").  
-   If the
    ![Cov(Y\_1, Y\_2) = 0](https://latex.codecogs.com/png.latex?Cov%28Y_1%2C%20Y_2%29%20%3D%200 "Cov(Y_1, Y_2) = 0")
    where
    ![Y\_1 \\sim N(\\mu, \\sigma^2\_1)](https://latex.codecogs.com/png.latex?Y_1%20%5Csim%20N%28%5Cmu%2C%20%5Csigma%5E2_1%29 "Y_1 \sim N(\mu, \sigma^2_1)")
    and
    ![Y\_2 \\sim N(\\mu, \\sigma^2\_2)](https://latex.codecogs.com/png.latex?Y_2%20%5Csim%20N%28%5Cmu%2C%20%5Csigma%5E2_2%29 "Y_2 \sim N(\mu, \sigma^2_2)"),
    then ![Y\_1](https://latex.codecogs.com/png.latex?Y_1 "Y_1") and
    ![Y\_2](https://latex.codecogs.com/png.latex?Y_2 "Y_2") are
    independent. This inference only holds for normally distributed
    variables. In multiple linear regression, we assume that each
    observed values represent an independent observation.

**Note:** For any constant matrix
![\\boldsymbol{A}^{n\\times n}](https://latex.codecogs.com/png.latex?%5Cboldsymbol%7BA%7D%5E%7Bn%5Ctimes%20n%7D "\boldsymbol{A}^{n\times n}")
and constant vector
![\\boldsymbol{b}^{n\\times 1}](https://latex.codecogs.com/png.latex?%5Cboldsymbol%7Bb%7D%5E%7Bn%5Ctimes%201%7D "\boldsymbol{b}^{n\times 1}"),
if
![\\boldsymbol{Y} \\sim N(\\boldsymbol{\\mu, \\boldsymbol{\\Sigma}})](https://latex.codecogs.com/png.latex?%5Cboldsymbol%7BY%7D%20%5Csim%20N%28%5Cboldsymbol%7B%5Cmu%2C%20%5Cboldsymbol%7B%5CSigma%7D%7D%29 "\boldsymbol{Y} \sim N(\boldsymbol{\mu, \boldsymbol{\Sigma}})"),
then the following linear transformation is also normally distributed
and
![\\boldsymbol{AY} + \\boldsymbol{b} \\sim N(\\boldsymbol{A \\mu} + \\boldsymbol{b}, \\boldsymbol{A\\Sigma A^\\intercal})](https://latex.codecogs.com/png.latex?%5Cboldsymbol%7BAY%7D%20%2B%20%5Cboldsymbol%7Bb%7D%20%5Csim%20N%28%5Cboldsymbol%7BA%20%5Cmu%7D%20%2B%20%5Cboldsymbol%7Bb%7D%2C%20%5Cboldsymbol%7BA%5CSigma%20A%5E%5Cintercal%7D%29 "\boldsymbol{AY} + \boldsymbol{b} \sim N(\boldsymbol{A \mu} + \boldsymbol{b}, \boldsymbol{A\Sigma A^\intercal})").

# Other resources

-   A University of Sydney Mathematics Learning Centre
    [resource](https://www.sydney.edu.au/content/dam/students/documents/mathematics-learning-centre/normal-distribution.pdf)
    on the normal distribution.  
-   A jbstatistics [YouTube
    video](https://www.youtube.com/watch?v=iYiOVISWXS4) on the normal
    distribution.  
-   A jbstatistics [YouTube
    video](https://www.youtube.com/watch?v=4R8xm19DmPM) on how to
    standardise normally distributed variables to obtain the standard
    normal distribution.  
-   A University of British Columbia [statistics
    tutorial](https://ubc-mds.github.io/DSCI_551_stat-prob-dsci/lectures/noteworthy-distribution-families.html#multivariate-gaussiannormal-family-20-min-1)
    about the bivariate and multivariate normal distribution.  
-   A University of British Columbia [statistics
    tutorial](https://ubc-mds.github.io/DSCI_551_stat-prob-dsci/lectures/dependence.html)
    on how to visualise multidimensional functions.
