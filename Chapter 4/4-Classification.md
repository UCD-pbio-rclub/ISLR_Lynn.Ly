# Chapter 4 Classification (logistic regression)

Response you're trying to predict is qualitative instead of quantitative 
Often, we're more interested in estimating the probabilities that X belongs to each category in C, rather than just finding the most likely answer

Why not linear regression to solve classification problems? 
  Say, Y = 0 if No, Y = 1 if Yes. 
  For binary outcomes, it's decent. (Linear discriminant analysis)

However, linear regression might produce probabilities less than 0 or bigger than 1. 
Logistic regression is more appropriate 

With multiple possible response variables, assigning numbers to the categories is dangerous. 
We should use *multiclass logistic regression* or *discriminant analysis*

# Logistic Regression 
Rather than modelling YES vs NO directly, logistic regression (logit) models the probability that Y belongs to a particular category. 

p(X) = Pr(Y = 1 | X)
p(X) = e^(B0 + B1X) / (1 + e^B0 + B1X)

e = Euler's number. We raise e to the power or a linear model. p(X) will always be between 0 and 1
When B0 + B1X is very large, p(X) approaches 1. 

Some rearrangement gives log(p(X) / 1 - p(X)) = B0 + B1X. This model is the log odds, or logit transformation of p(X)

How do we estimate the model from data? 
Maximum Likelihood: Gives the probability of the observed zeroes and ones in the data. 
  We pick B0 and B1 to maximize the likelihood of the observed data
  use `glm()`
  z = standardized slope. Don't care about the p value of the intercept 
  
# 4.3.2 Estimating the Regression Coefficients 
Use maximum likelihood. Want to find intercept and coefficients such that everyone who defaulted is valued near 1, else 0. Least squares is a method of maximum likelihood. The details aren't important since R / packages will do it for us. 

Coefficient meaning: 0.0055 for balance means a one-unit increase in balance increases the log odds of default by 0.0055

# 4.3.4 Multiple Logistic Regression
Be aware that variables may be correlated / confounding

# 4.4 Linear Discriminant Analysis
Used for classifying a response variable with more than 2 options

Use normal distributions for each option

Why discriminant analysis? 
1. When the classes are well-separated, logistic regression is v unstable. (coefficients of infinity)
2. If n is small and distribution of predictors X is ~normal, linear discriminant is more stable again. 
3. Also provides low dimensional views of the data


# 4.4.1 Bayes theorem for classification
Pr(k|x) can be flipped to P(x|k) * P(k) / P(x)
Written slightly differently for discriminant analysis.
Change the "threshold" for classification based on the highest density. Based on priors

(pi)k is the prior probability that a randomly chosen observation comes from the kth class

Take logs and discard terms that don't depend on k
Don't need to calculate discriminant scores, just see which are the largest
pk(x) = p(Y = k|x)
fk(x) = density function of X for a certain class k


### Problem: Classification gave many errors for true Yeses, but classified most "No"s correctly
Solution: Vary the threshold. As you increase the threshold, false negatives increase quickly but false positives do not increase substantially. So you should have some lower threshold. 

To visualize the threshold, use an ROC curve. We want the false positive rate to be low, and the true positive rate to be high. 45 degree line = no information. Ideally, we want the curve to be very close to the top left hand corner. 

You can compare different classifiers by comparing the ROC curves. Sometimes we use "AUC" area under curve to measure the ROC curve. Higher AUC is good

Quadratic Discriminant Analysis - When the variances are different between classes. The quadratic terms matter, and don't get canceled out. Usable when the number of variables is small. 

If many variables though, have to make big matrices and it breaks down. 
Naive Bayes - When you have tons 4000) variables, but they are all independent 
  Assumes that the __ are different, but diagonal. 
  Can use mixed feature vectors (qualitative and quantitative)
  
How do logistic regression and LDA differ? (in practice they are very similar)
  If you have only two classes, they're pretty much the same. They both give you linear logistic models
  The difference is in how the parametres are estimated 
    Logistic regression uses a conditional likelihood based on Pr(Y|X) *discriminative learning* Only uses the distribution of Y
    LDA uses the full likelihood based on Pr(X,Y) *generative learning* Uses the distribution of X and Y
