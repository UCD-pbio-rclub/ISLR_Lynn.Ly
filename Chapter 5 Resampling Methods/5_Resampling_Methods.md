# 5-Resampling Methods 

## Cross Validation  
Concept: Hold out a subset of the training observations from the fitting process, as the test subset.   
Caveats: Validation estimate of the test-error rate can be variable, due to randomness in selecting the test and training subsets  
         Overestimates the test-error rates since we're only training on half the data, losing power  
         
### 5.1.2 Leave-One-Out Cross-Validation  
Uses only a single observation for the test set. Repeat this n times, each time leaving out one sample.  
+ No randomness   
+ Does not overestimate test-error rate  
- Could be expensive to implement, since you have to fit a model n times.   
  + With least squares linear or polynomial regression, there is a great shortcut that relies on leverage  
    Divide the ith residual by (1 - leveragei)  
    The leverage reflects the amount that an observation influences its own fit.   
    
### 5.1.3 k-Fold Cross-Validation  
Randomly divide the observations into k groups (folds) of equal size  
Each fold acts as a validation set  
+ less expensive than LOOCV
+ Much less variation than simple CV

Important to find the minimum point in the estimated test MSE curve when trying to compare methods  


### 5.1.4 Bias-Variance Trade-Off for k-Fold Cross-Validation  
k-fold CV > LOOCV because it gives more accurate estimates of the test error rate than LOOCV  
LOOCV is much less biased than k-fold but has higher variance. Why?  
  LOOCV = averaging the outputs of n fitted models, each one almost exactly the same. So the outputs are highly correlated with each other.  
  k-fold = the overlap between training sets in each model is smaller.  
    Concept: The mean of many highly correlated qualities has higher variance than the mean of many non-correlated ones
*Typically, one performs k-fold CV using k = 5 or k = 10*  

### 5.1.5 Cross-Validation on Classification Problems  
Instead of using MSE to quantify test error, use the number of misclassified observations.  

## 5.2 The Bootstrap
Concept: Quantify the uncertainty associated with a given estimator or statistical learning method  
Example: Can be used to estimate the standard errors of the coefficients from a linear regression fit  

Toy example: Use bootstrap to look at variability associated with regression coefficients 
  Two companies: Yield returns of X and Y. Invest a in X and 1-a in Y 
  Goal: Minimize the total risk (variance) of the investment 
  Approach: Simulated 100 pairs of returns to estimate the variances
    To quantify the accuracy of our estimate, repeat the simulation process multiple times. 100 samples x 1000 times 
  Caveat: In real data, we cannot generate new samples from the original population. 
    BUT, bootstrap lets us emulate the process of obtaining new sample sets, so we can estimate the variability of a^ without generating additional samples. Obtain distinct data sets by repeatedly sampling observations from the original data set
  Sampling may occur with replacement 
  
  