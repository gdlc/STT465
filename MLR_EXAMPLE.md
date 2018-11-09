
### Example of Multiple Linear Regression Analysis


 1) Download the [wage data set](https://github.com/gdlc/STAT_COMP/blob/master/wage.txt) and read it into R.
 2) Conduct basic descriptive analysis
 3) Regress wage on education, race, experience, region, sex and marital status via OLS using `lm()`.
 4) What of the factors/variables have a significant effect on wages? How much do you expect wage to increase per year of
 education? What is the average wage difference between male and female in the sample? Is it statistically different than zero?
 5) Load the [gibbs sampler](https://github.com/gdlc/STT465/blob/master/gibbsMLR.md) into your R-session,.
 6) Collect, for the same model above-specified, 10,000 samples.
 7) Conduct post-gibbs analysis (trace plot, auto-correlation, decide on burn-in and thinning, provide posterior means, posterior SDs and posterior
 credibility regions, estimate and report MC error).
