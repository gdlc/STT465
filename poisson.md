
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
  lambda=seq(from=1/1000,to=8,by=.01)
  Lik=rep(NA,length(lambda))
  
  for(i in 1:length(lambda)){
    Lik[i]=exp(sum(dpois(x=x,lambda=lambda[i],log=TRUE)))
   }
   
   plot(Lik~lambda,type='l',col=4)
   abline(v=ML,col=2)
   abline(v=CI_95,col=2,lty=2)
```

Note: try changing sample size and evaluate how the likelihood function and CI change.

*3. Bayesian Inference*

We showed in class that the conjugate prior for the Poisson likelihood is Gamma. The Gamma prior is indexed by two hyper-parameters, the `shape` and `rate`. The expected value of the Gamma distribution is `E[lambda]=shape/rate` and the variance is `Var[lambda]=shape/(rate^2)`. How to choose these parameters? We can think of the rate as a prior `sample size`; thus, if we want a weakly informative prior we can set `rate=2` or a similar small value. Then, based on our prior expected value for the varaible (remember that in the poisson model `E[x]=lambda`) we can solve for the shape parameter. This is illustrated in the following example.

```r
 ## Prior distribution
  rate=2
  K=6 # our prior expected value
  shape=K*rate

 # Posterior distribution
  posteriorShape=shape+sum(x)
  posteriorRate=rate+n
  
  postDensity=dgamma(x=lambda,rate=posteriorRate,shape=posteriorShape)
  plot(postDensity~lambda,type='l',col=4,main='Posterior Density')
  abline(v=ML,col='green') # adding a green line at the Max. Lik. estimator
  abline(v=posteriorShape/posteriorRate,col=2) # adding a red vertical line @ the posterior mean
  BayesianCR=qgamma(shape=posteriorShape,rate=posteriorRate,p=c(.025,.975))
```

Let's compare the prior, likelihood and posterior distribution (I scale all of them to have a maximum=1).

```r
 priorDensity=dgamma(rate=rate,shape=shape,x=lambda)
 
 Lik=Lik/max(Lik)
 postDensity=postDensity/max(postDensity)
 priorDensity=priorDensity/max(priorDensity)

 plot(Lik~lambda)
 plot(Lik~lambda,type='l',col=1,ylim=c(0,1)) 
 lines(x=lambda,y=priorDensity,col=4)
 lines(x=lambda,y=postDensity,col=2)

```

*4. The Predictive Distribution**


The predictive distribution is `p(xNew|xPast)`. To arrive at the predictive distribution we:

  - Augment the distribution by introducing the uknownparameter
  - Integrate the uknown parameter out.
  
 `p(xNew|xPast)=Int( p(xNew,lambda|xPast)   ,with respect to lambda)`.
```r


```
