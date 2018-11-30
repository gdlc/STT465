## Likelihood and Bayesian Analysis of the Logistic Regression


Logistic regression is used to estimate the effects of a set of predictors (e.g., sex, age) on the success probability of a binary outcome (Yi). This is done
by linking the success probability, P(Yi=1|xi,b), with a linear predictor using a non-linear transformation  (the *logit*). 

  * *Success probability* `P(Yi=1)=pi`
  * *Linear predictor* `LPi=xi'b` where `xi` is a vector of covariantes and `b` is  a vector of effects
  * *Odds* `pi/(1-pi)`.
  * *Logit* the logaritm of the odds, `log(pi/(1-pi))=LPi=xi'b`.
  * Solving the Logit for `pi` we get `pi=exp(LPi)/(1+LPi)`.
 
 Using the terms defined above we can evaluate the likelihood functio of a logistic regression using the folowing
 
 
 **Log-likelihood of a Logistic Regression**
 
 ```r
  loglik=function(y,X,b){
     LP=X%*%b
     theta=exp(LP)/(1+exp(LP))
     logLik=ifelse(y==1,log(theta),log(1-theta))
     return(sum(logLik))
  }
 
 ```

<h1 id="likelihood">

### Fitting a logistic regression via maximum likelihood using `glm`

```r
rm(list=ls())
 
DATA=read.table('~/Desktop/gout.txt',header=T)
colnames(DATA)=c('sex','race','age','serumUrate','gout')
DATA$gout=ifelse(DATA$gout=="Y",1,0)
### Logistic Regression
 
# Incidence matrix for effects (not needed for glm because glm accepts formula as an input)
Z=as.matrix(model.matrix(~sex+age+race+serumUrate,data=DATA))[,-1]
Z=scale(Z,center=T,scale=F) # centering to improve mixing and convergence
 
# Maximum Likelihood using glm
fm=glm(DATA$gout~Z,family='binomial')
summary(fm)

```

**Solving the maximum likelihood problem using a general-purpouse optimization function (`optim()`)**

```r
# Maximum Likelihood using optim
  negLogLik=function(y,X,b){
                 Xb=X%*%b
                 theta=exp(Xb)/(1+exp(Xb))
                 logLik=sum( y*log(theta)+(1-y)*log(1-theta))
                 return(-logLik)
  }
 
  iniInt=log(mean(DATA$gout)/(1-mean(DATA$gout)))
 
  fm2=optim(fn=negLogLik,y=DATA$gout,X=cbind(1,Z),par=c(iniInt,rep(0,ncol(Z)))
                                                ,control=list(maxit=10000,reltol=1e-8)) 
  # `control` is used to define convergence and iteration parameters, help(optim) 
  
  fm2$convergence # 0 means it converged
  cbind(fm$coef,fm2$par)

```

<h1 id="bayes">

### Bayesian Analysis

The following code implements the Metropolis algorithm we discuss in class. The Likelihood si bernoully sith subject-specific success probability (see likelihood above). The prior is IID normal with mean b0 and variance varB, that is `bj~N(b0,varB)`. Samples are drawn using a Metropolis algorithm (Chapter 8 of the book) with candidates generated using a normal distribution centered at the current sample of effects and variance `V`, a user-specified parameter. The code includes two functions, the first one evlautes the posterior distribution and is used to evaluate the acceptance ratio, the second function is the sampler.



```r
   # A function to evaluate the log of the posterior density
   logP=function(y,X,b,b0,varB){
     Xb=X%*%b
     theta=exp(Xb)/(1+exp(Xb))
     logLik=sum( dbinom(x=y,p=theta,size=1,log=T)  )
     logPrior=sum(  dnorm(x=b,sd=sqrt(varB),mean=b0,log=T))
     return(logLik+logPrior)
   }


logisticRegressionBayes=function(y,X,nIter=70000,V=.02,varB=rep(10000,ncol(X)),b0=rep(0,ncol(X))){
 
  ####### Arguments #######################
  # y  a vector with 0/1 values
  # X  incidence matrix fo effects
  # b0,varB, the prior mean and prior variance bj~N(b0[j],varB[j])
  # V the variance of the normal distribution used to generate candidates~N(b[i-1],V)
  # nIter: number of iterations of the sampler
  # Details: generates samples from the posterior distribution of a logistic regression using a Metropolis algorithm
  #########################################
    
  # A matrix to store samples
   p=ncol(X)
   B=matrix(nrow=nIter,ncol=p)
   colnames(B)=colnames(X)
 
  # A vector to trace acceptancve
   accept=matrix(nrow=nIter,ncol=p,NA)
   accept[1,]=TRUE 
   
  # Initialize
   B[1,]=0
   B[1,1]=log(mean(y)/(1-mean(y)))
   b=B[1,]
  for(i in 2:nIter){
    
    for(j in 1:p){
      candidate=b
      candidate[j]=rnorm(mean=b[j],sd=sqrt(V),n=1)
 
      logP_current=logP(y,X,b0=b0,varB=varB,b=b)
      logP_candidate=logP(y,X,b0=b0,varB=varB,b=candidate)
      r=min(1,exp(logP_candidate-logP_current))
      delta=rbinom(n=1,size=1,p=r)
   
      accept[i,j]=delta
   
     if(delta==1){ b[j]=candidate[j] }
    }
    B[i,]=b
    if(i%%1000==0){
      message(" Iteration ",i)
    }
 
  }
  
  return(list(B=B,accept=accept))
}
 

```

**Use**


```r
  fm=glm(gout~sex+race+age,data=DATA,family='binomial')
  samples=logisticRegressionBayes(y=DATA$gout,X=cbind(1,Z),nIter=50000)
  cbind(fm$coef,colMeans(samples$B[-(1:1000),]))
  
  
```

**Suggestion:** (i) Try reducing the prior variance and changing the prior mean, (ii) Inspect the trace plot for different coefficients, (iii) Change `V` and assess the impact on the mixing of the algorithm, (iv) Estimate auto-correlations, number of independent samples, monte carlo error, etc.
