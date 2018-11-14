
### Example of Multiple Linear Regression Analysis


 1) Download the [wage data set](https://github.com/gdlc/STAT_COMP/blob/master/wage.txt) and read it into R.
 2) Conduct basic descriptive analysis
 3) Regress wage on education, race, experience, region, sex and marital status via OLS using `lm()` (see [OLS example](https://github.com/gdlc/STT465/blob/master/OLS.md)).
 4) Ussing the output of `summary(lm(y~....))` answer these questions:
    - What of the factors/variables have a significant effect on wages? 
    - How much do you expect wage to increase per year of education? 
    - What is the average wage difference between male and female in the sample? 
    - Is the difference statistically different from zero?
 5) Load the [gibbs sampler](https://github.com/gdlc/STT465/blob/master/gibbsMLR.md) into your R-session,.
 6) Collect, for the same model above-specified, 10,000 samples. **Hint**:  `gibbsMLR(y,X,...)` takes as inputs a numeric vector with the response (y, wages) and an incidence matrix for effects (sex, race, education, experience, ...). `lm()` creates the incidence matrix internally. `gibbsMLR()` does not, so you will need to create it. For this you can use
 `X=model.matrix(~ a+b+...,data=DATA)` where a, b are the predictors that are included in DATA. This will create your incidence matrix and then you will use it in `gibbsMLR(y,X)`.
 7) Conduct post-gibbs analysis (trace plot, auto-correlation, decide on burn-in and thinning, provide posterior means, posterior SDs and posterior
 credibility regions, estimate and report MC error). (see example of [post gibbs analyses](https://github.com/gdlc/STT465/blob/master/postGibbs.md))


### Least Squares Fit & Frequentist Inference

```r
 DATA=read.table("~/Desktop/wage.txt",header=TRUE)
 head(DATA)
 dim(DATA)
 
 # OLS fit
 fm=lm(Wage~Education+South+Black+Hispanic+Sex+Married+Experience,data=DATA)
 summary(fm)
 
 ## Prediction equations
 bHat=coef(fm)
 
 plot(Wage~Education,data=DATA,cex=.7,col=8,ylim=c(0,45))
 
 # Pred Eq for: female, hispanic, from the north, 10 yr of Exp, Married
 Int= bHat[1]+bHat[5]+bHat[6]+bHat[7]+10*bHat[8]
 Slope=bHat[2]
 Edu=seq(from=5,to=18,by=1)
 lines(x=Edu,y=Int+Edu*Slope,col=4)

 # Pred Eq for: male, white, from the north, 10 yr of Exp, Married
 Int= bHat[1]+bHat[7]+10*bHat[8]
 Slope=bHat[2]
 Edu=seq(from=5,to=18,by=1)
 lines(x=Edu,y=Int+Edu*Slope,col=2)
```

### Bayesian Analysis

```r
  wage=DATA$Wage
  X=model.matrix(~Education+South+Black+Hispanic+Sex+Married+Experience,data=DATA)
  SAMPLES=gibbsMLR(y=wage,X=X,nIter=13000)  
  dim(SAMPLES$effects)# cols are effects, rows are samples
  head(SAMPLES$varE)
   
```
### Post Gibbs

```r
 # trace plots of a few parameters....
  plot(SAMPLES$varE[1:500],type='o') # 1st 100 interations should be discared as burn in
  plot(SAMPLES$effects[1:500,1],type='o') #Intercept, probably 1st 200 needs to be discarded
  plot(SAMPLES$effects[1:5000,2],type='o') # effect of education. Discard 2000?
  
  ## Discarding 3K iterations as burn-in
  B=SAMPLES$effects[-(1:3000),];colnames(B)=colnames(X)
  varE=SAMPLES$varE[-(1:3000)]
  
  summary(B)
  
  # Converting to MCMC object 
  library(coda)
  B=as.mcmc(B)
  varE=as.mcmc(varE)
  
  
  autocorr.plot(B[,1],lag.max=100)
  autocorr.plot(B[,2],lag.max=100)
  
  summary(B)
  summary(varE)

  HPDinterval(B,prob=.95)

```
### Bayesian Analysis of Linear Hypothesis

A linear hypothesis takes the form **t'b**=0. **t** is called a contrats. 

For instance you may want to test wehther the ethnic disparity can be fully described as a difference between whites versus either hispanic or black. This can be formalized as `b[4]=b[5]` or `b[4]-b[5]=0`. The contrast for this is:
`contrast=c(0,0,0,0,1,-1,0,0)`. Thus, H0: `**contrast**'**b**=0`. The contrast is a deterministic function of the parameters (**b**). 


```r
 contrast=c(0,0,0,0,1,-1,0,0)
 tmp=B%*%contrast
 plot(density(tmp));abline(v=0)
 abline(v=HPDinterval(as.mcmc(tmp),p=.95),col=2,lty=2)
 mean(tmp>0)
 mean(tmp<0)
```
We conclude that the posterior probability that Black people have a wage smaller than Hispanics, holding evereything else constant, is ~0.97.

