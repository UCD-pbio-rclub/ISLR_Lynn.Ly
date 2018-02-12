Lynn

# R for Statistical Learning

# Chapter 3 Intro to Linear Regression

RSS = Squared distance of each real data point from the prediction line

How accurate is the sample mean u^ as an estimate of u?
  The average of u^s over many data sets will be very close to u, but a single estimate may be a substantial underestimate or overestimate. How far off is that single estimate?
    We answer this by computing the standard error of u^, SE(u^)
    
  Standard errors can be used to compute confidence intervals. A 95% CI is defined as:
  the range of values such that with 95% probability, the range will contain the true value of the parameter.
  
RSE = Residual standard error - estimate of the std of the error term.
  Aka the average amount that the response will deviate from the true regression line
  A very large RSE may indicate that the model doesn't fit the data well. 
  
R^2
  RSE is measured in units of Y, so it's not always clear what makes a good RSE.
  R2 measures a proportion of variability in Y that can be explained in X
  R2 = (TSS - RSS) / TSS
  R2 near 0 means that the regression did not explain very much of the variability in the response

Multiple Regression Questions
  Is there a relationship between the response and predictor? 
  Compute F-statistic for hypothesis test 
    If no relationship, F-statistic close to 1. F-statistic is 570, so TV is definitely important
    If n is very large, an F-statistic that is just over 1 might still provide evidence against H0
    A larger F-statistic is needed to reject H0 if n is small
    
How do you decide on the important variables? 
  We need an automated approach to pick the most important predictors. 
  
  Forward Selection
    Starting with the null model - has intercept but no predictors. 
    Fit p simple linear regressions and add the variable that results in the lowest RSS 
      (Aka picking the best model)
      
  Backward Selection
    Include all the predictors, remove the least useful one. 
    
Qualitative predictors - like sex, race, gender...
  Use a new variable, like xi = 1 if female, xi = 0 if male
  For p with multiple discrete categories, have a new dummy variable for each possible one 
  
Extensions of the Linear Model 
  
  Interactions - Suppose that multiple variables are related, either synergistically or not
    
3.3.3 Potential Problems
  1. *Non-linearity*. Use residual plots to identify non-linearity 
    You want there to be no patterns in the residuals - a flat line. 
    If non-linear, a simple approach is to use a transformation, like logX, sqrt, and ^2
  2. *Correlation of Error Terms*
    Importantly, you assume that the error terms are uncorrelated, so there is no bias in the error
      If there is correlation, then you underestimate the standard errors, and have CI narrower than they should be. Can lead to false significance in parameters 
    Correlations occur frequently in time series data - data obtained at adjacent times will be correlated positively often
      You should plot the residuals as a function of time and look for patterns / tracking. 
    Another potential thing is if people are from the same family / have the same diet / environment
  3. Non-constant Variance of Error Terms 
    We want error terms to have a constant variance. 
    *Heteroscedasticity* = non-constant variance, identifiable by a *funnel shaped residual plot*
    Example: Error terms increase as values increase 
    Solution: Transform using a concave function, like log or sqrt. Shrinks the large values
  4. Outliers
    Can be identified with the residual plot
    *Studentized residual* - divide each residual by its estimated standard error.
      If > 3, the observation is a possible outlier. 
  5. High Leverage Points 
    Outlier = Unusual value in Y. HLP = Unusual value in the predictor Xi
    Difficult to identify with multiple predictors, since the specific combination may be what's weird
    *Leverage statistic* hi increases with the distance from xi from the mean x
    A high leverage statistic indicates a likely high leverage point 
  6. Collinearity - When two or more predictor variables are closely related 
    Example: Credit card rating and limit are very close related when determining balance 
    *Contour plot of the RSS* If narrow, a small change in the data could cause the pair of coefficients that has the least RSS to shift along the valley -> uncertainty in the estimates 
      Collinearity reduces the power of the hypothesis test 
    Two-factor collinearity can be detected by looking at the *correlation matrix* 
    Doesn't work for *Multicollinearity* though. Instead, need to compute the *variance inflation factor.* The VIF is the ratio of the variance of a parameter with the full model vs. the parameter by itself. The smallest possible value for VIF is 1, which is the compete absence of collinearity. *VIF > 5 or 10 is problematic*
    Solutions: Drop the problematic variable, or combine them into a single predictor. 
    
3.5 Comparison of Linear Regression with K-Nearest Neighbors
    Linear regression is parametric - assumes a linear functional form for f(X)
    Non-parametric methods are alternative and more flexible approach for regression 
    KNN regression identifies the K training observations that are closest to a prediction point x0.(N0)
      Then estimates f(x0) using the average of all the training responses in N0
    Optimal K depends on *bias-variance tradeoff* from Chapter 2. 
    Small K = most flexible, low bias, but high variance. 
    Large K = smoother and less variable, but may have some bias due to masking the structure
    
    Parametric approach > non-parametric when the parametric form is close to the true form of f 
    
    Spreading 100 observations over p = 20 dimensions -> a given observation has no nearby neighbors 
      Aka *curse of dimensionality*. So generally, parametric > non when there are few observations per predictor 

    