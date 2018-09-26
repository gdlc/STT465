
## Poisson-Gamma (single-parameter) model

**Topics taht we will cover**:

  - Likelihood function
  - Maximum Likelihood inference (ML estimator, inference under the CLT theorem)
  - Bayesian inference: 
      - Gamma prior
      - Posterior distribution
      - Posterior mean/mode, posterior credibility regions.
      - Predictive distribution (Negative binomial, analythically and using Monte Carlo Methods).
      
 
 **Examples**:
 
 *A small simulated data set*
 
 ```r
  n=50
  x=rpois(n=n,lambda=2)
  
  ML=mean(x)      # the sample mean is also the ML estimator
  SE=sqr(ML/n)    # the sampling variance of the mean is var(xi)/n, var(xi) for x~Poiss(lambda)  is lambda
  CI_95=ML+c(1,-1)*1.96*SE
  
  CI_9=qnorm(mean=ML,sd=SE,p=c(.005,.995))
  
 ```
 *1. Maximum likelihood inference*
