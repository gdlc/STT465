
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
  set.seed(195021)
  n=50
  x=rpois(n=n,lambda=2)
  hist(x) 
 ```
 *1. Maximum likelihood inference*

```r 
  ML=mean(x)      # the sample mean is also the ML estimator
  SE=sqrt(ML/n)    # the sampling variance of the mean is var(xi)/n, var(xi) for x~Poiss(lambda)  is lambda
  CI_95=ML+c(1,-1)*1.96*SE   # a 95% CI
  CI_99=qnorm(mean=ML,sd=SE,p=c(.005,.995)) # a 99% CI
```

*2. The likelihood function*

```r
  lambda=seq(from=1/1000,to=6,by=.01)
  Lik=rep(NA,length(lambda))
  
  for(i in 1:length(lambda)){
    Lik[i]=exp(sum(dpois(x=x,lambda=lambda[i],log=TRUE)))
   }
   
   plot(Lik~lambda,type='l',col=4)
   abline(v=ML,col=2)
   abline(v=CI_95,col=2,lty=2)
```

Note: try changing sample size and evaluate how the likelihood function and CI change.
