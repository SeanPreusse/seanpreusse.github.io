---
title: "Learning Regression Trees"
header:
excerpt: "Regresion tree's is a great way to build predictive models that have a visual representation model output"
categories:
  - Machine-Learning
tags:
  - Tutorial
last_modified_at: 2016-01-16T15:11:19-04:00
---
Regression trees through the ‘party’ library in R is a great way to build a visual predictive model. The underlying ‘party’ package for R allows for an easy visual representation for the model output.

The example used in this tutorial is the prediction of a seed type from the seeds dataset; kernels belonging to three different varieties of wheat.

Data is sourced <a href="http://archive.ics.uci.edu/ml/machine-learning-databases/00236/seeds_dataset.txt">from here</a>

### Data Loading

{% highlight ruby %}
# Data Source 
seeds <- read.csv("data/seeds.csv", sep=",")
summary(seeds)
{% endhighlight %}

#### Output

{% highlight ruby %}
##       area         perimeter      compactness         length     
##  Min.   :10.59   Min.   :12.41   Min.   :0.8081   Min.   :4.899  
##  1st Qu.:12.27   1st Qu.:13.45   1st Qu.:0.8569   1st Qu.:5.262  
##  Median :14.36   Median :14.32   Median :0.8734   Median :5.524  
##  Mean   :14.85   Mean   :14.56   Mean   :0.8710   Mean   :5.629  
##  3rd Qu.:17.30   3rd Qu.:15.71   3rd Qu.:0.8878   3rd Qu.:5.980  
##  Max.   :21.18   Max.   :17.25   Max.   :0.9183   Max.   :6.675  
##      width         asymmetry         length2        seed_type
##  Min.   :2.630   Min.   :0.7651   Min.   :4.519   Min.   :1  
##  1st Qu.:2.944   1st Qu.:2.5615   1st Qu.:5.045   1st Qu.:1  
##  Median :3.237   Median :3.5990   Median :5.223   Median :2  
##  Mean   :3.259   Mean   :3.7002   Mean   :5.408   Mean   :2  
##  3rd Qu.:3.562   3rd Qu.:4.7687   3rd Qu.:5.877   3rd Qu.:3  
##  Max.   :4.033   Max.   :8.4560   Max.   :6.550   Max.   :3
{% endhighlight %}

### Preparing the Data

{% highlight ruby %}
str(seeds)
{% endhighlight %}

#### Output

{% highlight ruby %}
## 'data.frame':    210 obs. of  8 variables:
##  $ area       : num  15.3 14.9 14.3 13.8 16.1 ...
##  $ perimeter  : num  14.8 14.6 14.1 13.9 15 ...
##  $ compactness: num  0.871 0.881 0.905 0.895 0.903 ...
##  $ length     : num  5.76 5.55 5.29 5.32 5.66 ...
##  $ width      : num  3.31 3.33 3.34 3.38 3.56 ...
##  $ asymmetry  : num  2.22 1.02 2.7 2.26 1.35 ...
##  $ length2    : num  5.22 4.96 4.83 4.8 5.17 ...
##  $ seed_type  : int  1 1 1 1 1 1 1 1 1 1 ...
{% endhighlight %}


{% highlight ruby %}
colnames(seeds) <- c("area", "perimeter","compactness","length","width","asymmetry","length2","seed_type")
seeds$seed_type <- factor(seeds$seed_type)
{% endhighlight %}

### Creating the Model

{% highlight ruby %}
train_size <- round(nrow(seeds) *0.7)
test_size <- nrow(seeds) - train_size
train_size
{% endhighlight %}

#### Output

{% highlight ruby %}
## [1] 147
{% endhighlight %}

{% highlight ruby %}
set.seed(123)
training_indices <- sample(seq_len(nrow(seeds)), size=train_size)
train_set <- seeds[training_indices,]
test_set <- seeds[-training_indices,]
{% endhighlight %}

### Modelling

{% highlight ruby %}
library(party)
model1 <- ctree(seed_type~., data=train_set)
summary(model1)
{% endhighlight %}

#### Output

{% highlight ruby %}
##     Length      Class       Mode 
##          1 BinaryTree         S4
{% endhighlight %}

{% highlight ruby %}
model1
{% endhighlight %}

#### Output

{% highlight ruby %}
## 
##   Conditional inference tree with 6 terminal nodes
## 
## Response:  seed_type 
## Inputs:  area, perimeter, compactness, length, width, asymmetry, length2 
## Number of observations:  147 
## 
## 1) area <= 16.2; criterion = 1, statistic = 123.423
##   2) area <= 13.37; criterion = 1, statistic = 63.549
##     3) length2 <= 4.914; criterion = 1, statistic = 22.251
##       4)*  weights = 11 
##     3) length2 > 4.914
##       5)*  weights = 45 
##   2) area > 13.37
##     6) length2 <= 5.396; criterion = 1, statistic = 16.31
##       7)*  weights = 33 
##     6) length2 > 5.396
##       8)*  weights = 8 
## 1) area > 16.2
##   9) length2 <= 5.877; criterion = 0.979, statistic = 8.764
##     10)*  weights = 10 
##   9) length2 > 5.877
##     11)*  weights = 40
{% endhighlight %}

{% highlight ruby %}
plot(model1)
{% endhighlight %}

#### Output

![](/assets/images/regression_trees/regression_tree.png)

{% highlight ruby %}
table(predict(model1), train_set$seed_type)
{% endhighlight %}

#### Output

{% highlight ruby %}
##    
##      1  2  3
##   1 45  4  3
##   2  3 47  0
##   3  1  0 44
{% endhighlight %}

{% highlight ruby %}
test_prediction <- predict(model1, newdata=test_set)
table(test_prediction, test_set$seed_type)
{% endhighlight %}

#### Output

{% highlight ruby %}
##                
## test_prediction  1  2  3
##               1 18  4  4
##               2  0 15  0
##               3  3  0 19
{% endhighlight %}

### Making new predictions

{% highlight ruby %}
new_prediction <-
      predict(model1,
      list(area=11,
           perimeter=13,
           compactness=0.855,
           length=5, width=2.8,
           asymmetry=6.5,
           length2=5),
      interval="predict",
      level=.95)
new_prediction
{% endhighlight %}

#### Output

{% highlight ruby %}
## [1] 3
## Levels: 1 2 3
{% endhighlight %}
