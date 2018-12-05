
## Finite Mixutre Model


** Simulating data from a 3-component mixture of normals**

```r
n=1000
mu0=1:3
sd0=c(.2,.3,.3)
prob0=c(.3,.4,.3)
group0=sample(1:length(mu0),size=n,replace=T,prob=prob0)
y=rnorm(n=n,mean=mu0[group0],sd=sd0[group0])
hist(y,30)
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
