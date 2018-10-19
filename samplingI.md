We will describe two sampling methods (composition sampling and Gibbs sampling) using two Bernoulli random variables.

The joint probability (object PROBS below) is given by the following table

| 			   | Y=0      | Y=0  |
| ------------- |:-------------:| -----:|
| X=0     | 0.30 | 0.10 |
| X=1      | 0.15     |   0.45 |


```r
 PROBS=rbind(c(.3,.1),
			c(.15,.45))
			
rownames(PROBS)=c("p(X=0)","P(X=1)")
colnames(PROBS)=c("p(Y=0)","P(Y=1)")

## Marginals
 pX=rowSums(PROBS)
 pY=colSums(PROBS)

 #Are X and Y independent (please check it!)
 
```


## How do we sample from p(X,Y)?

We will consider thre methods. The first one is a very simple one. T

**(0) Sample directly from the joint distribution**

Let's 1, 2, 3 and 4 represent each of the possible events, (X=0,Y=0), (X=1,Y=0), (X=0,Y=1) and
(X=1,Y=1). We can sample numbers 1 to 4 with the probabilities in PROBS. Here is an example


```r
  B=1000000 # of Monte Carlo Samples
  theta=as.vector(t(PROBS))
  z=sample(1:4,prob=theta,size=B,replace=T)
  X=ifelse(z<=2,0,1)
  Y=ifelse(z==1|z==3,0,1)
  xtabs(~X+Y)/B   
```
**(1) Composition Sampling**

Composition sampling uses the fact that p(X,Y)=p(X)p(Y|X), which suggests that we can
sample from the joint distribution by sampling one of the random variables from its marginal
distribution and then the next one from the conditional distribution. The options are


   i) Sample X from p(X) 
   ii) Sample Y from p(Y|X)
   
or

   i) Sample Y from p(Y) 
   ii) Sample X from p(X|Y)


The following R-code illustrates a sampler that samples X from its marginal and Y from p(Y|XA)


```r
  # Conditional Distributions  		
  X=rbinom(prob=pX[2],size=1,n=B)
  pYGX=ifelse(X==0,PROBS[1,2]/pX[1],PROBS[2,2]/pX[2])
  Y=rbinom(prob=pYGX,n=B,size=1)
```
		
