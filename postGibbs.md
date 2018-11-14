## Post-Gibbs Analysis


In most Bayesian models the posterior distribution does not have a closed form. In those cases, we typically use Monte Carlo Methods
to draw samples from the posterior distribution and use those samples to make inferences about model's uknowns. In this entry
we discuss basic steps we should follow to make inferences from samples collected using Monte Carlo methods.

  -**Convergence**. We first need to assess whetehr our algorithm has converged to the posterior distribution. The very first thinge we need to do are:
    - *Trace plots*: We plot, for each parameters the chain containing the samples collected. We used trace plots to visually asses convergence mixing and to visually determined a save burn-in period 
(later on we will discuss more formal approaches, but the trace plot is essential).

  -**Discard burn-in**
  
  -**Mixing**: a more quantitative measure of mixing can be done using an aut-correlation plot, and by estimating the effective sample size.
  
  -**Thinning** retain 1 out of k samples.
  
  -**Do we have enough samples?** (based on the samples after discarding burn-in and after thinning): Typically we will require at least 100 effective samples. Additionally we can copute the
  Monte Carlo Error (MC-SE). We often want to have MC SE that are smaller than 1% of the estomated posterior mean.
  
  -**Compute posterior means, posterior standard deviations and hig-postrior credibility regions**.
  
  #### EXAMPLE 
  
 ```r
  library(coda)
  SAMPLES=as.mcmc(SAMPLES) # converting your samples into an MCMC object
  plot(SAMPLES[,2]) # this produces both density and trace plot
  plot(as.vector(SAMPLES[,2]),type='o',cex=.5,col=4)
  
  burnIn=1:100
  SAMPLES=SAMPLES[-burnIn,] # removing burn-in
  summary(SAMPLES) # Discuss Monte Carlo Error
  HPDinterval(SAMPLES,prob=c(.025,.975)) 
 ```
 
 **[Wages Data Set](https://github.com/gdlc/STAT_COMP/blob/master/wage.txt)**
 
 
 [Home](https://github.com/gdlc/STT465)
