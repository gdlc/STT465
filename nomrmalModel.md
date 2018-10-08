
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
#### Need to update what follows below....

### Normal Model

We will discuss liklelihood and Bayesian inference for the normal model, p(y|mu,V)=N(mu,V). We will begin with the case where the variance is known, 
then relax that assumption and discuss the model for the mean and variance. Then we will extend the model to the case where the mean
is parametrically modeled using a simple linear regression, so that yi~N(mui,V) with mui=mu+xib. Finally we will extend this to the case
where the mean is modeled using a multiple linear regression.

The first 2 cases are discussed in the book in chapter 5. Here I reproduce some of the scripts we develop in class. This is by no means
a substitute for the notes you take in class and the book.

#### (1) Bayesian Inference of the mean with known variance

In the following example we examine the ML estimator and the mean and variance of the posterior density for data with the same mean
and three different sample sizes.

```r
## Data (same mean, varying sample size)
 yBar=15
 n=c(10,20,50)
 Vy=10 # variance of the data

## prior
 mu0=4 # prior mean
 Vmu=5  # prior variance

 ## MLE
  varMLE=Vy/n
  # Approx. 95% CI for the the Max. Likelihod estimator.
  CI=rbind(15+c(-1.96,1.96)*sqrt(varMLE[1]),
	   15+c(-1.96,1.96)*sqrt(varMLE[2]),
 	   15+c(-1.96,1.96)*sqrt(varMLE[3])
 	)
  show(CI)
  
 ## Posterior density
  tau2_data=1/Vy  # presicion of a single data-point
  tau2_mu=1/Vmu   # prior precision
 
  postMean=c(  (n[1]*yBar*tau2_data +tau2_mu*mu0)/( n[1]*tau2_data+tau2_mu) ,
 			  (n[2]*yBar*tau2_data +tau2_mu*mu0)/( n[2]*tau2_data+tau2_mu),
 			  (n[3]*yBar*tau2_data +tau2_mu*mu0)/( n[3]*tau2_data+tau2_mu)
 			)
 			
 			
   postVar=1/c(	n[1]*tau2_data+tau2_mu ,
 			n[2]*tau2_data+tau2_mu  ,
 			n[3]*tau2_data+tau2_mu
 			)
 			
    show(postMean)
    
    ## Posterior 95% credictility regions
    CR=cbind(qnorm(mean=postMean,sd=sqrt(postVar),p=.025),
          qnorm(mean=postMean,sd=sqrt(postVar),p=.975))
    show(CR)
    
    ## In this case, sample size affects the witdh of the frequentist CI but not the midpoint
     CI[,1]+(CI[,2]-CI[,1])/2
     
    ## However, because the prior mean is not equal to the data mean, in the Bayesian CR, the midpoint also changes.
     CR[,1]+(CR[,2]-CR[,1])/2
 ```
 	
