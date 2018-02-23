# Chapter 6 Linear Model Selection and Regularization 

Purpose: Extend the linear model to accommodate non-linear, but still additive, relationships  

Caveat: The linear model is quite good usually; instead of extension, we could also replace plain least squares fitting with alternative methods.  
  Alternative fitting methods can yield better prediction accuracy and model interpretability 
  
If n (samples) is not way larger than p (parameters), then there can be a lot of variability in the least squares fit.  
  This leads to overfitting.
  By constraining / shrinking estimated coefficients, we can reduce the variance for a small cost of bias. 
  
*Model interpretability*: Often, many of the variables used in a multiple regression model have no relationship to the response.  
  Including irrelevant variables leads to unnecsesary complexity in the resulting model.  
  Feature selection / variable selection helps us set irrelevant coefficients to 0
  
*Subset selection* : Find a subset of the predictors that you believe are related, and fit the least squares model on these reduced variables. 
*Shrinkage*: Fit the model to all P predictors, but the coefs are shrunken toward zero relative to the least squares estimates. 
  Also known as *regularization*. This reduces variance 
*Dimension Reduction* : Project the p parameters into a M-dimensional subspace, where M < p.  
  How? Compute M different linear combinations (projections) of the variables. Then, these M projections are used as predictors for linear regression. 
  

## Subset Selection

### 6.1.1 Best Subset Selection
    Fit a separate least squares regression for every possible combination of the p predictors. 
    Difficulty: Selecting the best model out of p^2 possibilities gets computationally expensive as p > 20 or 40
    
    #### Algorithm 6.1  
      1. Null model: No predictors  
      2. Choose the best subset of parameters for EACH set of quantity of parameters using smallest RSS or highest R^2  
      3. Select a single best model (the best number of parameters) using cv prediction error, AIC BIC, or adjusted R^2  
  
  Splitting it into steps like above is necessary because higher numbers of parameters will always have the least RSS on the training data due to overfitting. 
  Visualize the improvement in RSS over number of parameters added 
  
    - Statistical and computation problems when p is large
    - The larger the search space, the easier to find models that are good on training data but not test data -> overfitting  
  
  ### 6.1.2 Stepwise Selection  
    + Explores a far more restricted set of models than Best Subset Selection  
    + WAY faster
    - might not find the absolute best model in certain scenarios  
    Starts with the null model and adds the predictor with the greatest impact 
    
    #### Algorithm 6.2 Forward Stepwise Selection 
      1. Null model: No predictors
      2. Add 1 parameter at a time via RSS / R^2
      3. Decide on the best number of parameters using cv, AIC, BIC, or adjusted R^2
      + Is the only subset model that can be used even when n < p 
      
    #### Algorithm 6.3 Backward Stepwise Selection
    Same as 6.2 but starting with all parameters fit, and removing one at a time. 
  
    Also, hybrid approaches are available but not discussed. Concept: Add new variables, but also remove old ones that don't fit anymore. 
    
    
### 6.1.3 Choosing the best model / best subset method 
  1. Indirectly estimate the test error by making an *adjustment* to the training error due to bias from overfitting  
  2. Directly estimate the test error using a validation set or cross-validation approach from Ch 5  
  
#### Cp 
  Adds a penalty of `2 * #predictors * variance of error` to the training RSS to adjust for training error < test error 
  Penalty increases as # predictors increases 
  Takes on a small value for model with a low test error
  
#### AIC criterion
  With Gaussian errors, maximum likelihood and least squares are the same. AIC is proportional to Cp for least squares models 
  
#### BIC 
  Is derived from Bayesian logic, but looks similar to AIC / Cp 
  Replaces the 2 * d * var penalty from Cp with `log(n) * d * var`
  Since log(n) > 2 for any n > 7, BIC places a heavier penalty on having more variables and generally chooses smaller models 
  
  Adjusted R^2 is valid but not as widely used as the other 3.  
  Maximizing R^2 means minimizing RSS 
  
#### Cross-Validation
  + Makes fewer assumptions about the underlying model
  + Applicable in a wider range of selection tasks, even when it's hard to pinpoint the # of predictors, or hard to estimate the error variance
  - Used to be computationally prohibitive for large p and/or large n, BUT
  + Nowadays, easy to compute and a very good approach relative to indirect estimates 
  
  *one standard error rule* calculate the standard error of the estimated test MSE for each model size
    Select the smallest model that falls within one standard error of the lowest point on the curve
    Rationale: If the models are really similar / more or less the same, then we might as well pick the simplest model 
    
## 6.2 Shrinkage Methods 
  Generally, constrain / regularize the coefficient estimates so that they're shrunk toward zero.
    Shrinking the coefficient estimates can significantly reduce their variance. 
  
#### 6.2.1 Ridge Regression 
  