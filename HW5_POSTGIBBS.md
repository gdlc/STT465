## HW 4 SST 465

**Due**: Wednesday Nov 22.


We have used the Gibbs sampler to implement a Bayesian multiple linear regression model. In this HW you will work on analyzing samples collected with a Gibbs sampler.

A classical paper [Casella & George, 1992](http://www.stat.ufl.edu/archived/casella/OlderPapers/ExpGibbs.pdf)



### (1) Post-Gibbs Analyses


Using the [gout](https://github.com/gdlc/STT465/blob/master/gout.txt) data set fit a linear regression model with sex, age and race as effects and serum urate
as response. The gibbs sampler for fitting thid model is   available in out GitHub repository (`gibbsMLR.md`). Collect a total of 20,000 samples.

   1.1. For each parameter in the model provide a **trace** plot and decide upon burn-in.
   Carry out the analyses requested below only based on samples that were not discarded as burn-in.
   
   
   1.2. Provide, for each parameter an estimate of the **posterior mean, posterior standard deviation and Monte-Carlo error**
   (Hint: try `summary()` on an `mcmc` object, created using the coda package).
   
   
   1.3. Provide, for each parameter, **auto-correlation plots** from lags 0 to lag 100.

   1.4. For each parameter provide an **estimate of the number of independent samples** that you have collected (hint: try `effectiveSize()`)

   1.5. For each parameter in the model provide an estimate of the **posterior density** (try `plot(density())`). 
   Superimpose in each plot vertical lines indicating the estimated posterior mean and estimated **95% high-posterior density intervals** (hint try `HPDintervals()`)
