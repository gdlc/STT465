
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
  
 ```
 *1. Maximum likelihood inference*
