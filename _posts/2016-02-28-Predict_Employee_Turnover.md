---
title: "Predicting Employee Turnover"
header:
excerpt: "Predicting employee turnover and having a greater understanding of key drivers is important for any business. Implementing successful interventions based on early information can help you stay ahead of regretted turnover."
categories:
  - Machine-Learning
tags:
  - Employee-Churn
  - Tutorial
last_modified_at: 2016-01-28T15:11:19-04:00
---

Predicting employee turnover and having a greater understanding of key drivers is starting to emerge in a few organisations. Some people may have a gut feeling when an employee is on the verge of leaving but what if you could create a dataset with many different behaviour identifiers to predict turnover early so that you could intervene? Here we will use Logistic regression to complete this analytical task.

Logistic regression has a place for many practical business problems. My experience with this type of modelling is in the prediction of a binary outcome e.g. predicting employee turnover (yes/no), employees stress, employee performance and employee sentiment.

The example here will look at predicting employee turnover. The accuracy of this model is 86% and can be improved. Data used is supplied from IBM Watson, workforce attrition dataset.

### Load required packages

{% highlight ruby %}
# Required Packages
library(ROCR)
library(caret)
{% endhighlight %}

### Data

This data has been sourced from IBM Watson Analytic. The predictive model that we will build will look at the probability of an employee leaving the organisation.
'https://community.watsonanalytics.com/hr-employee-attrition/'

{% highlight ruby %}
data <- read.table("http://www.infoexcite.com/data/employee_attrition_example.csv", sep=",", header=T)
data$outcome <- data$attrition
data$attrition <- NULL
{% endhighlight %}

### Cleaning the data

We only want to select key variables that make sense i.e. job, environment and relationship satisfaction as well as performance rating.

{% highlight ruby %}
drop <- c("education", "environment_satisfaction", "job_involvement","job_satisfaction", "performance_rating","relationship_satisfaction")
data_model <- data[,!(names(data) %in% drop)]
drop2 <- c("business_travel", "department", "education_field", "gender", "job_role", "marital_status", "over_18", "over_time")
data_model <- data_model[,!(names(data_model) %in% drop2)]
{% endhighlight %}

### Create Training Sets

{% highlight ruby %}
smp_size <- floor(0.75 * nrow(data_model))
set.seed(123)
train_ind <- sample(seq_len(nrow(data_model)), size = smp_size)
train <- data_model[train_ind, ]
test <- data_model[-train_ind, ]
{% endhighlight %}

### Model Build

{% highlight ruby %}
model <- glm(outcome ~., family=binomial(link='logit'),data=train)
summary(model)
{% endhighlight %}

#### Output

 {% highlight ruby %}
# Call:
# glm(formula = outcome ~ ., family = binomial(link = "logit"), 
#     data = train)
#
# Deviance Residuals: 
#     Min       1Q   Median       3Q      Max  
# -1.5474  -0.5881  -0.3947  -0.1955   3.0800  
# 
# Coefficients: (3 not defined because of singularities)
#                                  Estimate Std. Error z value Pr(>|z|)    
# (Intercept)                     4.849e+00  1.321e+00   3.672 0.000241 ***
# age                            -2.545e-02  1.401e-02  -1.816 0.069365 .  
# daily_rate                     -4.281e-04  2.298e-04  -1.863 0.062466 .  
# distance_from_home              3.778e-02  1.075e-02   3.514 0.000441 ***
# education_rank                 -2.407e-02  9.179e-02  -0.262 0.793124    
# employee_count                         NA         NA      NA       NA    
# employee_number                -2.031e-04  1.535e-04  -1.323 0.185748    
# environment_satisfaction_rank  -2.724e-01  8.298e-02  -3.283 0.001028 ** 
# hourlyrate                     -3.298e-03  4.518e-03  -0.730 0.465409    
# job_involvement_rank           -3.273e-01  1.271e-01  -2.575 0.010018 *  
# job_level                      -2.277e-01  2.908e-01  -0.783 0.433639    
# job_satisfaction_rank          -3.114e-01  8.314e-02  -3.746 0.000180 ***
# monthly_income                 -9.502e-06  7.085e-05  -0.134 0.893310    
# monthly_rate                    1.329e-05  1.308e-05   1.017 0.309316    
# num_companies_worked            1.034e-01  3.973e-02   2.604 0.009226 ** 
# percent_salary_hike            -7.484e-03  3.948e-02  -0.190 0.849658    
# performance_rating_rank         4.016e-02  4.034e-01   0.100 0.920688    
# relationship_satisfaction_rank -1.676e-01  8.472e-02  -1.978 0.047892 *  
# standard_hours                         NA         NA      NA       NA    
# stockoption_level              -4.049e-01  1.202e-01  -3.367 0.000759 ***
# total_working_years            -6.271e-02  3.004e-02  -2.088 0.036816 *  
# training_times_last_year       -2.330e-01  7.401e-02  -3.148 0.001642 ** 
# worklife_balanceBest           -5.599e-01  4.376e-01  -1.279 0.200730    
# worklife_balanceBetter         -8.521e-01  3.588e-01  -2.375 0.017547 *  
# worklife_balanceGood           -4.184e-01  3.809e-01  -1.098 0.272004    
# worklife_balance_rank                  NA         NA      NA       NA    
# years_at_company                6.085e-02  4.271e-02   1.425 0.154225    
# years_in_current_role          -1.316e-01  4.883e-02  -2.694 0.007056 ** 
# years_since_last_promotion      1.722e-01  4.438e-02   3.880 0.000105 ***
# years_with_curr_manager        -1.109e-01  4.577e-02  -2.422 0.015444 *  
# ---
# Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
# 
# (Dispersion parameter for binomial family taken to be 1)
# 
#   Null deviance: 964.64  on 1101  degrees of freedom
# Residual deviance: 795.74  on 1075  degrees of freedom
# AIC: 849.74
# Number of Fisher Scoring iterations: 6
{% endhighlight %}

### Model Reduce

{% highlight ruby %}
model_reduced <- step(model, trace=0)
summary(model_reduced)
# Reduction of one variable, great!
{% endhighlight %}

### Model Prediction

{% highlight ruby %}
test$predict <- predict(model_reduced, newdata=test, type="response")
summary(test$predict)
{% endhighlight %}

#### Output

{% highlight ruby %}
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
## 0.001922 0.049840 0.124300 0.170500 0.252400 0.744200
{% endhighlight %}

### Model Effectiveness, Chart 1 - ROC Curve

{% highlight ruby %}
# First Step
pred <- prediction(test$predict, test$outcome)
# ROC CUrve Chart 1
perf = performance(pred, measure = "tpr", x.measure = "fpr")
plot(perf, colorize=T,lwd=3)
abline(a=0, b= 1)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-7-1.png)

{% highlight ruby %}
# ROC CUrve Chart 2
perf <- performance(pred, "prec", "rec")
plot(perf, colorize=T,lwd=3)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-7-2.png)

{% highlight ruby %}
# ROC CUrve Chart 3
perf <- performance(pred, "tpr", "fpr")
plot(perf, avg='threshold', spread.estimate='stddev', colorize=T,lwd=3)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-7-3.png)

{% highlight ruby %}
# ROC CUrve Chart 4
perf <- performance(pred, "cal",window.size=50)
plot(perf)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-7-4.png)

{% highlight ruby %}
# ROC CUrve Chart 5
perf <- performance(pred, "acc")
plot(perf, avg= "vertical", spread.estimate="boxplot", show.spread.at= seq(0.1, 0.9, by=0.1),lwd=2)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-7-5.png)

{% highlight ruby %}
# ROC CUrve Chart 6
perf <- performance(pred,"pcmiss","lift")
plot(perf, colorize=T, print.cutoffs.at=seq(0,1,by=0.1), text.adj=c(1.2,1.2), avg="threshold", lwd=3)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-7-6.png)

### Optimal cutoff

{% highlight ruby %}
acc.perf = performance(pred, measure = "acc")
plot(acc.perf)
{% endhighlight %}

![](/assets/images/logistic_regression/unnamed-chunk-9-1.png)

### Matrix and Plot with cut-off

{% highlight ruby %}
# Test will full
pred_cutoff <- factor(ifelse(test$predict > .1, "Yes", "No"))
confusionMatrix(pred_cutoff, test$outcome)
{% endhighlight %}

#### Output

{% highlight ruby %}
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction  No Yes
##        No  148   8
##        Yes 158  54
##                                           
##                Accuracy : 0.5489          
##                  95% CI : (0.4965, 0.6005)
##     No Information Rate : 0.8315          
##     P-Value [Acc > NIR] : 1               
##                                           
##                   Kappa : 0.1805          
##  Mcnemar's Test P-Value : <2e-16          
##                                           
##             Sensitivity : 0.4837          
##             Specificity : 0.8710          
##          Pos Pred Value : 0.9487          
##          Neg Pred Value : 0.2547          
##              Prevalence : 0.8315          
##          Detection Rate : 0.4022          
##    Detection Prevalence : 0.4239          
##       Balanced Accuracy : 0.6773          
##                                           
##        'Positive' Class : No              
## 
{% endhighlight %}

### Cutoff Test

{% highlight ruby %}
pred_cutoff <- factor(ifelse(test$predict > 0.4, "Yes", "No"))
confusionMatrix(pred_cutoff, test$outcome)
{% endhighlight %}

#### Output

{% highlight ruby %}
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction  No Yes
##        No  293  39
##        Yes  13  23
##                                           
##                Accuracy : 0.8587          
##                  95% CI : (0.8189, 0.8926)
##     No Information Rate : 0.8315          
##     P-Value [Acc > NIR] : 0.0907709       
##                                           
##                   Kappa : 0.3944          
##  Mcnemar's Test P-Value : 0.0005265       
##                                           
##             Sensitivity : 0.9575          
##             Specificity : 0.3710          
##          Pos Pred Value : 0.8825          
##          Neg Pred Value : 0.6389          
##              Prevalence : 0.8315          
##          Detection Rate : 0.7962          
##    Detection Prevalence : 0.9022          
##       Balanced Accuracy : 0.6642          
##                                           
##        'Positive' Class : No              
## 
{% endhighlight %}


#### Plot results

{% highlight ruby %}
pROC = function(pred, fpr.stop){
    perf <- performance(pred,"tpr","fpr")
    for (iperf in seq_along(perf@x.values)){
        ind = which(perf@x.values[[iperf]] <= fpr.stop)
        perf@y.values[[iperf]] = perf@y.values[[iperf]][ind]
        perf@x.values[[iperf]] = perf@x.values[[iperf]][ind]
    }
    return(perf)
}
proc.perf = pROC(pred, fpr.stop=0.4)
plot(proc.perf, colorize=T,lwd=3, main="ROC Curve - True Positive Rate")
abline(a=0, b= 1)
{% endhighlight %}

For a cut-off of .4 (we only want to target the highest probability) we get an accuracy of 85%. The coloured line represents much better than average 'the black line'

![](/assets/images/logistic_regression/unnamed-chunk-10-1.png)
