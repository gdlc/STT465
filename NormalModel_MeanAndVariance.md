
## Infering the Mean and Variance in the Normal Model (Ch. 5)



```r


mu=10
V=3
n=1000
y=rnorm(n=n,sd=sqrt(V),mean=mu)


## Hyper-parameters (i.e., parameters of the prior distribution(s))
 priorMean=20
 priorVar=100  # very large=>weakly informative prior

 priorDF=4
 priorS=2  # using E[var]=df*S/(df-2), this gives a prior mean of (1)/(4-2)=0.5 
 		   # likewise the mode of the prior is df*X/(df+2)
 
## Gibbs Sampler

 nIter=1000            # number of samples we will collect
 sMean=rep(NA,nIter)  # here we will store the samples for mu
 sVar=rep(NA,nIter)   # here we will store the samples for the variance

 # Initialization
  sMean[1]=mean(y)
  sVar[1]=var(y)

  yBar=mean(y)
 
 # Sampling
 for(i in 2:nIter){
	LHS=(n/sVar[i-1]+1/priorVar)
	rhs=(n*yBar/sVar[i-1]+ priorMean/priorVar)
	sMean[i]=rnorm(mean=rhs/LHS,sd=sqrt(1/LHS),n=1)
	
    SS=sum((y-sMean[i])^2)
    
    S=SS+priorS*priorDF
    DF=priorDF+n
    sVar[i]=S/rchisq(df=DF,n=1)   
 }

plot(sVar,type='o',ylim=c(1.5,4.5),col=4)
abline(h=var(y),col=2)
abline(h=priorDF*priorS/(priorDF-2),col='green')
abline(h=mean(y),col=4)

```

For convinience we may create an R-function. The following example illustrates how to create a function that takes as arguments `x` and `msg` (the latest has a default value) and prints the message and returns x-squared.

```r
 # Function definition
 getSq=function(x,msg="hello, here is X-squared"){
 	print(msg)
	return(x^2)
 }

 # Use
 getSq(10)
 getSq(10,"not a very interesting function, use x^2")

```

Now, let's wrap-up our code into a function


```r
  gibbsNormal=function(y,priorMean,priorVar,priorDF,priorS){
 
	## Gibbs Sampler
 	nIter=1000            # number of samples we will collect
 	sMean=rep(NA,nIter)  # here we will store the samples for mu
 	sVar=rep(NA,nIter)   # here we will store the samples for the variance

 	# Initialization
  	sMean[1]=mean(y)
  	sVar[1]=var(y)

  	yBar=mean(y)
 
 	# Sampling
 	for(i in 2:nIter){
		LHS=(n/sVar[i-1]+1/priorVar)
		rhs=(n*yBar/sVar[i-1]+ priorMean/priorVar)
		sMean[i]=rnorm(mean=rhs/LHS,sd=sqrt(1/LHS),n=1)
    		SS=sum((y-sMean[i])^2)
    
    		S=SS+priorS*priorDF
    		DF=priorDF+n
    		sVar[i]=S/rchisq(df=DF,n=1)   
 	}

  	out=cbind(sMean,sVar)
	colnames(out)=c("mean","var")
	return(out)
  }

  # USE
   samples=gibbsNormal(y,0,1000,priorDF=5,priorS=3)
   plot(samples[,"mean"],type='o',cex=.5,col=4)
   

```
