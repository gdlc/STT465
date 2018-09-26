

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

a) Assume that each of the four sex-by-race group has it's own prevalence (i.e., probability of developing gout). Under this assumption, write the likelihood function for the entire sample. (Hint: your likelihood function must involve distinct success probabilities for each group).

b) Derive the maximum likelihood estimator for each of the success probabilitites.


b) Provide maximum liklihood estimates and confidence intervals (both 95% and 99%) of the prevalence of gout in each of the four sex-by-race groups.

c) Are the differences betwen groups statistically significant? Explain.


**Question2: Bayesian inference**

As in Question assume that each group has its own probability of developing gout.

a) Asuming a uniform prior (i.e., Beta with shape parameters equal to one) derive and present the joint posterior distribution (show explicitly each of the steps you follow to derive the posterior distribution) for the probability of developing gout of each of the groups.

b) Present 95% posterior credibility regions for each of the parameters.

c) Plot the posterior distribution for each of the groups (all in the same plot.

d) Summarize your findings.


**Question 3**

The data set in the following [link](https://stats.idre.ucla.edu/stat/data/fish.csv) contains data on the # of fish caugth by campers in a park.

a) Present a table with observed  frequencies for x (x=# of fish caughted)=0,1,2,.....

b) Assuming a Poisson model, provide the maximum likelihood estimate of the Poisson parameter ("lambda") and an approximate 95% CI.

c) Present in a bar-plot the observed frequencies (from question a) and the predicted frequencies accoridn to a poisson model with a rate parameter equal to the maximum likelihood estimate that you reported in b.

d) Comment the results that you obained in c. Does the Poisson model fits well this data?



