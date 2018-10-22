
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
 priorS=4  # this gives a prior mean of 4/2=2

## Gibbs Sampler
 nIter=100
 sMean=rep(NA,nIter)
 sVar=rep(NA,nIter)

 # Initialization
 sMean[1]=mean(y)
 sVar[1]=var(y)

 yBar=mean(y)
 
 # Sampling
 for(i in 2:nIter){
	LHS=(n/sVar[i-1]+1/priorVar)
	rhs=(n*yBar/sVar[i-1]+ priorMean/priorVar)
	sMean[i]=rnorm(mean=rhs/LHS,sd=sqrt(1/LHS),n=1)
	
    S=sum((y-sMean[i])^2)+priorS
    DF=priorDF+n
    
    sVar[i]=S/rchisq(df=DF,n=1)
    
}



```
