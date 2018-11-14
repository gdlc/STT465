TThe following entry provides a Gibbs sampler for a Muliple Linear Regression Model (MLR). The derivation of the fully conditionals needed
to implement the Gibbs sampler is available in the following [handout (MLR.pdf)](https://github.com/gdlc/STT465/blob/master/MLR.pdf).



The function that implements the Gibbs sampler takes as **inputs**:

  - y (nx1) a vector with the response.
  - X (nxp) the incidence matrix for effects,
  - nIter, the number of samples to be collected, set by default to 10,000.
  - Hyper-parameters: df0 and S0 (the scale and degree of freedom for the scaled-inverse chi-square prior assigned to the 
  error variance) and b0 and varB (the mean and variance of the normal prior assigned to effects). All these have default values that will give
  a relatively un-informative prior.
  
 The function returns the samples collected by the Gibbs sampler, the **Gibbs sampler algorithm** is schematically summarized here:
  - Initialize parameters (e.g., initialize all reg. coefficients to zero, and the intercept to mean(y), initialize the error variance to be equal to the variance of y).
  - Loop over iterations (1:nIter)
     - Sample the error variance from its fully conditional distribution (see [9] in [MLR.pdf](https://github.com/gdlc/STT465/blob/master/MLR.pdf).
     - Loop ovrer predictors (1:ncol(X)), at each iteration sample each coefficient from its fully conditional distribution (see expression [8] in [MLR.pdf](https://github.com/gdlc/STT465/blob/master/MLR.pdf).
 


 
  
 **Example of use**
 
 ```r
  SAMPLES=gibbsMLR=function(y,X)  
```

To use the function we need data and we need to prepare the incidicence matrix (X). The following code provides an example based on the gout data set.

## Reading data to test the algorithm
```r
DATA=read.table('~/Desktop/gout.txt',header=F)
colnames(DATA)=c('sex','race','age','serum_urate','gout')

DATA$sex=factor(DATA$sex,levels=c('M','F'))
DATA$race=as.factor(DATA$race) 
```

#### Preparing incidence matrix 

```r
dF=ifelse(DATA$sex=='F',1,0) # a dummy variable for female
dW=ifelse(DATA$race=='W',1,0) # a dummy variable for male
 
# Incidence matrix for intercept and effects of sex, race and age
 X=cbind(1,dF,dW,DATA$age) 
 head(X)
 y=DATA$serum_urate
```

#### Gibbs

I wrapped up our Gibbs sampler into a function

```r
gibbsMLR=function(y,X,nIter=10000,df0=4,S0=var(y)*0.8*(df0-2),b0=0,varB=1e12,verbose=500){

 ## Objects to store samples
  p=ncol(X); n=nrow(X)
  B=matrix(nrow=nIter,ncol=p,0)
  varE=rep(NA,nIter)

 ## Initialize
  B[1,]=0
  B[1,1]=mean(y)
  b=B[1,]
  varE[1]=var(y)
  resid=y-B[1,1]

 ## Computing sum x'x for each column
  SSx=colSums(X^2)

  for(i in 2:nIter){
    # Sampling regression coefficients
    for(j in 1:p){
      C=SSx[j]/varE[i-1]+1/varB
      Xy= sum(X[,j]*resid)+SSx[j]*b[j]  # equivalent to X[,j]'(y-X[,-j]%*%b[-j])
      rhs=Xy/varE[i-1]  + b0/varB
      condMean=rhs/C
      condVar=1/C
      b_old=b[j]
      b[j]=rnorm(n=1,mean=condMean,sd=sqrt(condVar))
      B[i,j]=b[j]  
      resid=resid-X[,j]*(b[j]-b_old) # updating residuals
    }
    # Sampling the error variance  
    RSS=sum(resid^2)
    DF=n+df0
    S=RSS+S0
    varE[i]=S/rchisq(df=DF,n=1)

    if(i%%verbose==0){ cat(' Iter ', i, '\n') }
  }

  out=list(effects=B,varE=varE)
  return(out)
 }
```

**Use:**

```r
 samples=gibbsMLR(y,X,nIter=10000,varB=1000) #large varB should give estimates close to OLS
 head(samples$varE) # samples for the error variance
 head(samples$effects)    # samples for the effects
 
 lm(y~X-1) # OLS
 colMeans(samples$effects[-(1:1000),]) # our estimates are very similar to OLS, except for the intercept 
                                       # because internally we centered all the columns of X, except the 1st one.
                                       
 lm(y~scale(X,center=T,scale=F))
 
```
