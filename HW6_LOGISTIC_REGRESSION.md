### HW 6: Bayesian Analysis of the Logistic Regression

**Due**: Monday Dec. 3rd (@ D2L and printed copy in class)

**Topics**:
  - Logistic Regression
  - Metropolis Algorithm
  - Analyses of Monte Carlo Samples
  
 
 **(1) Maximum Likelihood Estimaton and Interpretation of Parameter Estimates**
 
Using the [gout](https://github.com/gdlc/STT465/blob/master/gout.txt) data set fit a logistic regression with `gout` as response, `sex`, `race` and `age` as predictors using the `glm()` function.


1.1. Report parameter estimates, SEs and p-values


1.2. Summarize your findings


1.3. What is the estimated probability of developing gout for a male, white, 65 years old person?



**(2) Bayesian Analysis**

Use the Metropolis sampler developed in class ([logisticRegressionBayes](https://github.com/gdlc/STT465/blob/master/logisticRegression.md#bayes)) to fit the logistic regression. Collect 55,000 samples, discard the frist 5,000 for burn in.

**Note**: to imrpove mixing, I suggest you center your predictors.

2.1. Report parameter estimates, posterior standard deviation and 95% posterior credibility regions for each of the regression coefficients.



2.2. Report, for each coefficient, the trace plot and estimates of the numbrer of effective samples and the MC standard error.

2.3. Use the samples collected to estimate the posterior distribution of the probability of developing gout for each of these cases listed below (report one plot with the estimated posterior desnity of each of these probabilities, add for each density vertical lines for 95% credibility regions).


| Sex    |  Race | Age |
|--------|-------|-----|
| F   |  W | 55 |
| F    |  W | 65 |
| F    |  B | 55 |
| F    |  B | 65 |
| M   |  W | 55 |
| M    |  W | 65 |
| M    |  B | 55 |
| M    |  B | 65 |



3. Collect samples for the model of question 2 using `V=.4`, `V=.2`, `V=.05` and `V=.001`.

3.1. Report the average acceptance rate and the lag-100 correlation and effective number of samples for the effect of age.

3.3. What value of V would you recommend? Why?

