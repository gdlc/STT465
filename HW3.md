
## HW3: Poisson & Normal models


Due Oct 9th in D2L.

#### Q1: Bayesian Inference in the Poisson model

The [crab](https://github.com/gdlc/STT465/blob/master/crab.txt) data set contains information on the number of "satellites" per female crab. The goal of this question is to infer, using a Bayesian model, the Poisson parameter.

The data set can read into R using the following code

```r
   DATA=read.table("~/Desktop/crab.txt",header=TRUE)
   y=DATA$nSatellites
```

  a) Provide summary statistics  (mean, variance, range, histogram).
  
  b) Write the likelihood function

  c) Derive the posterior distribution assuming a Gamma prior with rate=20 and shape=3
  
  d) Provide the posterior mean, posterior SD and 95 and 99% posterior credibility region (hint: you can get a formula for the variance from the Gamma distribution, credibility regions can be derived using `qgamma`).

  e) Plot the prior and posterior distribution of lambda, in the same plot.
  
 
#### Q2: Maximum Likelihood Inference of the mean in a Gaussian Model

For this question we will use the variable `DATA$width`.

   a) Provide summary statistics (mean, variance, range, histogram).
   
   b) Write the likelihood function under Gaussian assumptions.
   
   c) Derive the Maximum Likelihood estimator of the mean parameter (recall the steps: write the likelihood, simplify as much as possible, take the log, take derivative with respect to the mean, set the derivative equal to zero, solve for the mean).
   
   d) Provide the Max. Lileihood esitmate of the mean for this data set together with an approximate 95% CI (assume Central Limit Theorem).
