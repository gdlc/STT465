### Multiple Linear regression via Ordinary Least Squares (OLS)

The following example illustrates how to implement a multiple linear regression using OLS. We do it using the `lm()` function and with matrix operations.


**Data**
```r
 DATA=read.table('~/GitHub/STT465/gout.txt',header=T)
  
 
 head(DATA) #shows first rows, use tail(DATA) to see the last rows and fix(DATA) to see data as a table
 
 # Transforming categorical predictors into factors
  DATA$sex=factor(DATA$sex,levels=c('M','F'))
  DATA$race=as.factor(DATA$race) 
 ```
 
 **OLS usign `lm()`**
 ```r
 # OLS esitmation
  fm=lm(serum_urate~sex+race+age,data=DATA)
  
  # extracts estimates, SEs and p-values
  summary(fm)
```

**OLS using matrix operations**

```r
 dF=ifelse(DATA$sex=='F',1,0) # a dummy variable for female
 dW=ifelse(DATA$race=='W',1,0) # a dummy variable for male
 
 # Incidence matrix for intercept and effects of sex, race and age
 X=cbind(1,dF,dW,DATA$age) 
 head(X)
 y=DATA$serum_urate

 # OLS equations
 Xy=t(X)%*%y
 XtX=t(X)%*%X
 # Estimates, compare with fm$coef
  bHat=solve(XtX,Xy)
 
 # To get SEs we need an estimate of the error variance
  eHat=y-X%*%bHat
  vEHat=sum(eHat^2)/(nrow(DATA)-ncol(X)) # Sum of squares of errors divide by n-rank(X)
  SE=sqrt(diag(solve(XtX)*vEHat))
  z=bHat/SE
  ## P-values (under normality first, and under t then...)
  pvaluesN=pnorm(abs(z),lower.tail=F)*2
  pvaluesT=pt(abs(z),df=length(y)-ncol(X),lower.tail=F)*2
  
```
