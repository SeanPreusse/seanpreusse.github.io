---
title: "Is HR Analytics worth the investment?
"
header:
excerpt: "Is HR Analytics worth the investment? Many find it difficult to find the necessary resourcing or funding start tackling big HR problems with data, is there a stepping stone that you can leverage to start the journey?"
categories:
  - Practical_Action_Plans
tags:
  - Data_Science_Tools
  - Business_Case
last_modified_at: 2016-08-12T15:11:19-04:00
---
![](/assets/images/Is%20HR%20Analytics%20worth%20the%20investment.jpg)

## Is HR Analytics worth the investment?

Is HR Analytics worth the investment? I have talked to a few HR professionals from varying organisations and many find it difficult to find the necessary resourcing or funding start tackling big HR problems with data. This article focuses on retention as a possible business case and hopefully provides you with some ideas to tackle this problem in a cost-effective way immediately.

We all know that retention of emerging leadership, high performing employees and critical roles is a no brainer but we often do not have enough information on this group and we tend to rely on gut feeling when creating strategies to retain or throw everything at the wall to convert them from an external offer only to have lower productivity or eventual loss further down.

The cost of doing nothing can be significant and in the millions and for many organisations people analytics is usually staffed by a part-time resource and it can be difficult to justify the additional $100k investment to your director to increase resourcing and start answer business problems to really give it a shot.

Here is a quick business case to consider, that may help get the investment needed. You are an averaged sized organisation with five thousand employees and attrition is normal at 12%. Not all roles are replaced, let’s say 80% and given the below model the total cost of replacement is ~$80m. Factors for replacement cost will vary by organisation and may consider advertising, productivity, knowledge loss and time spent with key individuals involved with the process.

![](/assets/images/hr_invest3.jpg)

What if we could use people analytics to understand the key drivers of employee turnover or perhaps develop a predictive model to understand high-risk employees to allow for early intervention? This analysis and early intervention may form a part of a sprint program at a cost of 20-60k and if we were able to prevent just 1% of unwanted/regretted turnover, the total benefit could be around $1m. Not a bad ROI, justifying the cost of the sprint program and implementation.

There are additional benefits to this, as you start to intervene on validated drivers there may be improvements in productivity, engagement, and collaboration to name a few. Not all predictor variables will apply to the attrition model and if you would like to validate these side benefits why not use this process and data model to compare these outputs directly to validate, score and improve.

**Data and Variables to consider**

Below are some examples of the types of variables that may shed light into drivers of attrition. You will likely have this data available in some format and remember that you can beg borrow and steal to start building your analytical data model if you need data outside of HR i.e. business performance, workload, collaboration or work flexibility data.

What is an analytical data model? This can be as simple as a range of key employee metrics by employee with an outcome column (yes they left or no they stayed). Key metrics can be built in excel with monthly snapshot data to build key employee events.

![](/assets/images/hr_invest1.jpg)

**Analysis**

In my <a href="https://www.linkedin.com/pulse/generating-hr-insights-sean-preusse?lipi=urn%3Ali%3Apage%3Ad_flagship3_pulse_read%3Bg62HFiFERECKUKRFCzk4Kg%3D%3D" target="_blank">last article</a> I mentioned a range of free tools that you can use to generate insights. Once you have created your analytical dataset, you can use R to complete a decision tree of key variables to produce the below chart. This will highlight natural segments where employee attrition may be very high and it will start to shed light on important drivers or where further investigation may be required.

* In the below example, we can see that Gen Y employees who have been recently on-boarded and who travel frequently are at high risk of leaving. This may indicate poor role fit for business-related travel for recent hires.
* The second highest attrition probability are employees who have been working with the organisation for over a year but have worked for more than 6 companies in their 10 year working life. The changing attitudes of work for this generation may be a key issue for most but if we look at the number of employees impacted, the majority '419' have a much lower probability - further investigate what makes this group different and apply learnings.

R makes decision trees really easy to complete but you do need sensible variables (you should not throw 50+ variables and hope that it makes sense). Here is the code that you will need, simple right?

*library(party)
plot(ctree(Attrition ~ ., data = data), main="Employee Attrition - Decision Tree")*

![](/assets/images/hr_invest2.jpg)


As you start to get comfortable with the results of the modelling i.e. the variables make sense and you are starting to identify natural segments with high probability, why not compare these results to a predictive logistic regression model - A tutorial on this can be <a href="http://infoexcite.com/machine%20learning/2016/01/28/Predict_Employee_Turnover.html" target="_blank">found here</a>.

With this type of model you will need to decide your level of appetite, if this is a new model you will want to start small and identify employees with a high probability of attrition and then start to understand the highest drivers via the coefficient and then segment to understand what role and line of business they exist in to start creating interventions to improve.