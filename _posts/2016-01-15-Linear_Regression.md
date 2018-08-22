---
title: "Learning Linear Regression"
header:
excerpt: "Linear regression is an approach for modelling the relationship between a dependant variable and one or more independent variables. This can be useful to show ROI of a program."
categories:
  - Machine Learning
  - Data Science
tags:
  - Learning
last_modified_at: 2016-01-15T15:11:19-04:00
---
Linear regression is an approach for modelling the relationship between a dependant variable and one or more independent variables.

This type of analysis is great to show the ROI of a program, typically when the dependant variable is related to revenue or cost benefit. You can also use linear regression on time to predict when engagement or high stress may occur.

![](/assets/images/linear_regression/linear_regression.png)

### Data Loading

{% highlight ruby %}
# data source: "http://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data"
library(sqldf)
autos <- read.csv("data/auto.csv", sep=",")
{% endhighlight %}


### Data Cleaning

{% highlight ruby %}
autos$horsepower <- as.numeric(autos$horsepower)
autos$weight <- as.numeric(autos$weight)
autos$year <- factor(autos$year)
autos$origin <- factor(autos$origin)
autos$cylinders <- factor(autos$cylinders)
autos$name <- NULL
{% endhighlight %}

### Creating the Model

Creating a training set and hold out set 'test' is an important part to building a predictive model. This is an easy task for R, only a few lines of code required.

{% highlight ruby %}
train_size <- round(nrow(autos) * 0.7)
test_size <- nrow(autos) - train_size

set.seed(123)
training_indices <- sample(seq_len(nrow(autos)),size=train_size)
train_set <- autos[training_indices,]
test_set <- autos[-training_indices,]
{% endhighlight %}

### Modelling

Lets build our first linear regression model using the train data set.

{% highlight ruby %}
model1 <- lm(formula=train_set$mpg ~ . , data=train_set)
summary(model1)
{% endhighlight %}

#### Output

{% highlight ruby %}
## 
## Call:
## lm(formula = train_set$mpg ~ ., data = train_set)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -7.231 -1.693  0.066  1.641 11.753 
## 
## Coefficients:
##                Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  32.1636689  3.0200512  10.650  < 2e-16 ***
## cylinders4    6.3752363  2.0904858   3.050  0.00254 ** 
## cylinders5    5.1023058  2.9260543   1.744  0.08243 .  
## cylinders6    4.4632375  2.2457692   1.987  0.04797 *  
## cylinders8    6.4701767  2.5396794   2.548  0.01144 *  
## displacement  0.0095497  0.0085302   1.120  0.26399    
## horsepower   -0.0309956  0.0149760  -2.070  0.03951 *  
## weight       -0.0055992  0.0007804  -7.175 8.15e-12 ***
## acceleration  0.0112276  0.0998305   0.112  0.91054    
## year71        0.8237770  1.0026891   0.822  0.41210    
## year72       -0.6630779  0.9845797  -0.673  0.50127    
## year73       -0.5732564  0.9016260  -0.636  0.52548    
## year74        0.8771954  1.0584685   0.829  0.40804    
## year75        0.2441576  1.0276471   0.238  0.81239    
## year76        1.3970878  0.9879225   1.414  0.15855    
## year77        3.2199748  1.0453308   3.080  0.00230 ** 
## year78        2.5545481  0.9585029   2.665  0.00819 ** 
## year79        5.2412950  1.0317058   5.080 7.36e-07 ***
## year80       10.9284434  1.1183704   9.772  < 2e-16 ***
## year81        6.2349352  1.0534448   5.919 1.06e-08 ***
## year82        8.0130985  1.0637229   7.533 8.96e-13 ***
## origin2       1.7545885  0.5798928   3.026  0.00274 ** 
## origin3       2.0559795  0.5820600   3.532  0.00049 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.741 on 251 degrees of freedom
## Multiple R-squared:  0.8912, Adjusted R-squared:  0.8817 
## F-statistic: 93.45 on 22 and 251 DF,  p-value: < 2.2e-16
{% endhighlight %}

Prediction with 95% confidence

{% highlight ruby %}
predictions <- predict(model1, test_set, interval="predict", level=.95)
head(predictions)
{% endhighlight %}

#### Output

{% highlight ruby %}
##        fit       lwr      upr
## 2 16.31326 10.690858 21.93567
## 4 17.80032 12.170447 23.43018
## 5 17.98474 12.313119 23.65637
## 6 12.39972  6.689705 18.10973
## 7 11.87254  6.062164 17.68292
## 8 12.12337  6.363162 17.88359
{% endhighlight %}

### Diagnostic

{% highlight ruby %}
comparison <- cbind(test_set$mpg, predictions[,1])
colnames(comparison) <- c("actual","predicted")
{% endhighlight %}

{% highlight ruby %}
head(comparison)
{% endhighlight %}

#### Output

{% highlight ruby %}
##   actual predicted
## 2     15  16.31326
## 4     16  17.80032
## 5     17  17.98474
## 6     15  12.39972
## 7     14  11.87254
## 8     14  12.12337
{% endhighlight %}

{% highlight ruby %}
summary(comparison)
{% endhighlight %}

#### Output

{% highlight ruby %}
##      actual        predicted     
##  Min.   : 9.00   Min.   : 8.422  
##  1st Qu.:17.00   1st Qu.:18.615  
##  Median :22.00   Median :24.240  
##  Mean   :23.08   Mean   :24.046  
##  3rd Qu.:29.60   3rd Qu.:29.631  
##  Max.   :46.60   Max.   :39.991
{% endhighlight %}

### Error Rate

{% highlight ruby %}
mape <- (sum(abs(comparison[,1]-comparison[,2]) / abs(comparison[,1]))/nrow(comparison))*100
mape
{% endhighlight %}

#### Output
{% highlight ruby %}
## [1] 11.60109
{% endhighlight %}

### Error Rate Table

{% highlight ruby %}
mape_table <- cbind(comparison, abs(comparison[,1]-comparison[,2])/comparison[,1]*100)
colnames(mape_table)[3] <- "absolute percent error"
head(mape_table)
{% endhighlight %}

#### Output

{% highlight ruby %}
##   actual predicted absolute percent error
## 2     15  16.31326               8.755096
## 4     16  17.80032              11.251974
## 5     17  17.98474               5.792613
## 6     15  12.39972              17.335215
## 7     14  11.87254              15.196144
## 8     14  12.12337              13.404470
{% endhighlight %}

{% highlight ruby %}
sum(mape_table[,3])/nrow(comparison)
{% endhighlight %}

#### Output

{% highlight ruby %}
## [1] 11.60109
{% endhighlight %}
 
### Making new predictions

{% highlight ruby %}
new_prediction <- predict(model1, 
                            list(cylinders=factor(4), 
                            displacement=370,
                            horsepower=150, 
                            weight=3904, 
                            acceleration=12, 
                            year=factor(70),
                            origin=factor(1)),
                          interval="predict",
                          level=.95)
new_prediction
{% endhighlight %}

#### Output

{% highlight ruby %}
##        fit      lwr      upr
## 1 15.69844 9.350904 22.04597
{% endhighlight %}
