

## Homework 2 

Due Sept. 30th in D2L.



The `gout.txt` data set contains information on n=400 subjects. Fre ach sbject we have the follwoing information:
  - sex (M=male/F=female)
  - race (B=African American/W=White European)
  - Age (years)
  - Serum urate
  - Gout (a type of inflammatory arthritis, Y=yes/N=no)
  
The following code illustrates how to read the data into R.
```r
  DATA=read.table('~/Desktop/gout.txt',header=T)
```

**Question 1: Maximum likleihood**

a) Assume that each of the four sex-by-race group has it's own prevalence (probability of gout). Under this assumption write down the likelihood function for the entire sample. (Hint: your likelihood function must involve distinct success probabilities for each group).

b) Derive the maximum likelihood estimator for each of the success probabilitites.


b) Provide maximum liklihood estimates and confidence intervals (both 95% and 99%) of the prevlance of gout in each of the four sex-by-race groups.

c) Are the differences betwen groups statistically significant? Explain.


**Question2: Bayesian inference**

As in Question uming a uniform prior (i.e., Beta with shape parameters equal to one) derive the joint posterior distribution 
