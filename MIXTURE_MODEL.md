
## Finite Mixutre Model

In a finite mixture model the density function of a RV (say X) is modeled as a weighted sum of a finite number of distributions. Here we we consider mixutres of Gaussians, thus, for a 3-component mixutre model the denisty of X would be given by:


  `p(x|mu,SD)=p1*dnorm(x,mean=mu[1],sd=SD[1])+p2*dnorm(x,mean=mu[2],sd=SD[2]) + (1-p1-p2)*dnorm(x,mean=mu[1],sd=SD[1])`  [1]
     
where: p1 and p2 are proportions (0<p1<1, 0<p2<1, 0<p1+p2<1) and `dnorm()` is the density of a normally distributed random variable.

We can also describe a finite mixture using a Hierarchical model:

Let `zi` be a indicator variable that can take values 1, 2 and 3, then we can write:


	`p(Zi=zi,Yi=yi|mu,SD)=p(yi|Zi=zi,mu,SD)*p(Zi=zi)`			[2]
	
Here, `p(zi)=1(zi=1)*p1 + 1(zi=2)*p2 + 1(zi=3)*(1-p1-p2)` is the prior probability of the components which is given by the mixture proportions (p1, p2 and 1-p1-p2), `mu` and `SD` are vector containing the means and SDs of each of the components of the mixtures and `p(yi|Zi=zi,mu,SD)=dnorm(yi|mu[zi],SD[zi])` is a normal density. The expected value of [2], taken with respecdt to the distribution of Z, is [1]. 


**Simulating data from a 3-component mixture of normals**

```r
n=1000
mu0=1:3
sd0=c(.2,.3,.3)
prob0=c(.3,.4,.3)
group0=sample(1:length(mu0),size=n,replace=T,prob=prob0)
y=rnorm(n=n,mean=mu0[group0],sd=sd0[group0])

# Empirical density
fitDen=density(y)
plot(fitDen,lwd=2,col=4,ylim=c(0,.6))
x=seq(from=0,to=4,length=1000)
for(i in 1:3){
   tmp=dnorm(x=x,mean=mu0[i],sd=sd0[i])
   tmp=tmp/max(tmp)*max(fitDen$y)
   lines(x=x,y=tmp,col=8,lwd=.5)
}


```

**Gibbs Sample**

```r

gibbsMixture=function(y,nComp,nIter=5000,DF0=4,S0=1,priorMean=0,priorVar=1e5){

  n=length(y)
  MU=matrix(nrow=nIter,ncol=nComp,NA)
  SD=MU
  GROUP=matrix(nrow=nIter,ncol=n,NA)

  ## Initial values
  GROUP[1,]=1
  if(nComp==2){
	GROUP[1,y>=median(y)]=2
  }else{
	for(i in 1:(nComp-1)){
		GROUP[1,y>=quantile(y,prob=i/nComp)]=i+1
		#print(i/nComp)
	}
  }

  for(i in 1:nComp){
	MU[1,i]=mean(y[GROUP[1,]==i])
	SD[1,i]=mean(y[GROUP[1,]==i])
  }	

  PROBS=matrix(nrow=n,ncol=nComp)

  for(i in 2:nIter){
	# Sampling groups
	for(j in 1:nComp){
		PROBS[,j]=dnorm(x=y,mean=MU[i-1,j],sd=SD[i-1,j])
	}
	
	GROUP[i,]=apply(X=PROBS,x=1:nComp,MARGIN=1,FUN=sample,size=1,replace=F)
	
	for(j in 1:nComp){
		tmp=GROUP[i,]==j
		nj=sum(tmp)
		
		postPrec= ( 1/priorVar+nj/(SD[i-1,j]^2) )
		postMean=( priorMean/priorVar   + sum(y[tmp])/(SD[i-1,j]^2))/postPrec
		
		MU[i,j]=rnorm(n=1,sd=sqrt(1/postPrec),mean=postMean)
		
		S=S0+sum((y[tmp]-MU[i,j])^2)
		DF=nj+DF0
		V=S/rchisq(n=1,df=DF)
		
		SD[i,j]=sqrt(V)
	}
	print(i)

	return(list(MEAM=MU,SD=SD,GROUP=GROUP))
}
```

**Example**

```r


```
