
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
