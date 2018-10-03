## Normal Model

  1) The normal distribution (mean, variance, quantiles) and its properites
  2) Normal Likelihood
  3) Maximum Likelihood estimation and inference 
  4) Normal prior for a model where the variance is assumed to be known.
  5) Posterior distribution
  6) Posterior inference, and comparison with ML inference.
  
 ##### 1) The Normal Distribution
 
 The normal distribution is the most commonly used distribution for quantitative random variables. The normal distribution has support on the
 real line, its indexed by two parameters the mean and the variance, and it is symmetric around the mean.
 
 The normal distribution is closed under linear operartions meaning that if `x~N(mu,V)`, then `y=a+bx` also follows a normal distribution.
 The mean and variance of `y` are: `E[y]=E[a+bx]=a+bE[x]=a+b*mu`, and the variance is `Var(y)=Var(a+bx)=b^2V(x). Therfore, `y~N(a+b*mu,b^2*V)`.
 
 
 In R we can use `rnorm`, `dnorm`, `pnorm` and `qnorm` to draw samples (`rnorm`), evaluate the density (`dnorm`),
 the CDF (`pnorm`) and quantiles (`qnorm`).
 
 
 **Examples**
 
 ```r
  mu=4
  V=2
  
  x=rnorm(10000,mean=mu,sd=sqrt(V))
  
  mean(x)
  var(x)
  
  hist(x,50)
  
  y=100+2*x
  
  var(y)
  mean(y)
  
  hist(y,50)
  
  density(y)
  
  qnorm(p=.01,mean=mu,sd=sqrt(V))

  quantile(x,p=0.01)
  
 ```
 
 
 ##### 2) The Normal Likelihood
 
 The likelihood is the probability of the data given the parameters, viewed as a function of the parameters.
 
 ```r
   mu=-2
   V=2
   x=rnorm(30,mean=mu,sd=sqrt(V))
   
   logLik=function(x,mu,V){
      logL=sum(dnorm(x=x,mean=mu,sd=sqrt(V),log=TRUE))
      return(logL)
   }
   
   logLik(x=x,mu=-3,V=2)
   logLik(x=x,mu=1,V=2)
   logLik(x=x,mu=mean(x),V=2)
   
   # evaluating the likelihood
   y=rep(NA,length(myGrid))
   for(i in 1:length(y)){ y[i]=logLik(x=x,mu=myGrid[i],V=V) }
   
   # A plot of the liklelihood function
    plot(exp(y),x=myGrid,type='l')
    abline(v=mean(x) ,col=2)

   
 ```
