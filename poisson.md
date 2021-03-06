
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
  
Consider the joint posterior distribution of future data and the uknownparameter, `p(xNew,lambda|xPast)`. Using rules of probability discussed in the class we have
 
 
 `p(xNew,lambda|xPast)=p(xNew|lambda,xPast)p(lambda|xPast)`.
 

Conditional on `lambda` the future data does not depend on the past data, then


 `p(xNew,lambda|xPast)=p(xNew|lambda)p(lambda|xPast)`.
 
The first term in the right-hand side is the Poisson likelihood, and the second term is the posterior distribution which we showed in this model is Gamma. To arrive at the predictive distribution we need to integrate lambda out from `p(xNew,lambda|xPast)`. We can do this analythically (see pp 47 in the book) or numerically, using Monte Carlo methods.
 
*Monte Carlo Estimation of the Predictive Distribution*

```r
 nMCSamples=10000
 
 x=seq(from=0,to=10,by=1)
 
 PRED.DIST=matrix(nrow=nMCSamples,ncol=length(x))
 
 for(i in 1:nMCSamples){
    tmpLambda=rgamma(n=1,shape=posteriorShape,rate=posteriorRate)
    PRED.DIST[i,]=dpois(x=x,lambda=tmpLambda)
 }

 MC.EST.PRED.DIST=colMeans(PRED.DIST)
 
 barplot(MC.EST.PRED.DIST)
 
```

How good is the Monte Carlo estimate? We will show in class (and also see pp 47 in the book) that the predictive distribution is Negative Binomial, thus, the quality of the MC estimate can be evaluated by comparing the MC estimated predictive distribution with the Negative Bionmial.


*Comparsion with the Naive plug-in estimator*

A naive estimator of the preditive distribution would be a Poisson distribution with `lambda=ML estimate` or `lambda=Posterior mean`. This predictive distribution does not account for the uncertainty about lambda. Let's compare it with the correct predictive distribution the one derived in the previous block of code).

```r
  y=dpois(x=x,lambda=posteriorShape/posteriorRate)
  points(x=x,y=y,pch=19,col='darkred')
  lines(x=x,y=y,col='darkred') #strictly speaking we should not add lines because x is discrete...

```
