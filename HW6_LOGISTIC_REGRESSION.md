### HW 6: Bayesian Analysis of the Logistic Regression

**Due**: Monday Dec. 3rd (printed copy in class)

**Topics**:
  - Logistic Regression
  - Metropolis Algorithm
  - Analyses of Monte Carlo Samples
  
 
 **(1) Maximum Likelihood Estimaton and Interpretation of Parameter Estimates**
 
Using the [titanic](https://github.com/gdlc/STT465/blob/master/titanic.csv) data set fit a logistic regression with `survived` as response, `sex`, `class` and `age` as predictors using glm. Note: sex and class should be treated as goruping factors, and age as a contionous predictor.

**Note**: Some entries have missing values. Be sure to remove all the rows of the data set that contain missing values @ sex, class, age or survived. Hint: you can find missing values using `is.na(DATA$survivied)` or non-missing using `!is.na(DATA$survived)`. 


1.1. Report parameter estimates, SEs and p-values


1.2. Summarize your findings


1.3. Report estimated probabilities for male and female in each class (set age to be 30).

**(2) Bayesian Analysis**

Use the Metropolis sampler developed in class ([logisticRegressionBayes](https://github.com/gdlc/STT465/blob/master/logisticRegression.md#bayes)) to fit the logistic regression. Collect 55,000 samples, discard the frist 5,000 for burn in.

**Note**: to avoid confusion when comparing results from the Bayesian and ML analysis, do not center the predictors.

2.1. Report parameter estimates, posterior standard deviation and 95% posterior credibility regions for each of the regression coefficients.



2.2. Report, for each coefficient, the trace plot and estimates of the numbrer of effective samples and the MC standard error.

2.3. Use the samples collected to estimate the posterior distribution of the survival probability for male and female in each of the classes (fix age to 30). For each of the groups report a histogram of the posterior desnity of the survival probability with vertical read lines indicating 95% posterior credibility regions.


3. The  function `logisticRegressionBayes` implements a Metropolis algorithm. With candidates generated from normal distribution with mean equal to the current sample and variance `V`. Small values of lambda lead to high rates of acceptance but high correlation between samples. Fit the logistic regression of question 2 using `V=.5`, `V=.1`,`V=.001`,`V=.0001`, and `V=.00005`.

3.1. Report the average acceptance rate and the lag-50 correlation and effective number of samples for the effect of age.

3.3. What value of V would you recommend? Why?

