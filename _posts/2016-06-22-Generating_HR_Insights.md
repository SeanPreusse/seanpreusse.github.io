---
title: "Generating HR Insights"
header:
excerpt: "At some point you may reach a bottle neck where you have squeezed the sponge dry. Your team will require a few different skills to optimise the impact and frequency of workforce insights."
categories:
  - Business-Case
tags:
  - Data-Science-Tools
  - Behavioural-Analytics
last_modified_at: 2016-06-22T15:11:19-04:00
---
![](/assets/images/generating_insights/Generating%20HR%20Insights.jpg)

## Generating HR Insights

Your team will require a few different skills to optimise the impact and frequency of workforce insights. To start, you may be able to partner an experienced HR business partner with someone who can source and analyse data to question and shape relevant ideas. This is effective as you can quickly identify what is important, build a business case for additional and start off with a quick win that will allow you to spend extra time with key stakeholders.

At some point you may reach a bottleneck where you have squeezed the sponge dry in a sense and it may be difficult to find new insights or your team get stuck into regular reporting around what has previously worked, only to see that the reports are not adding any value further down the track or they do not go deep enough to shed light onto the issues.

Here are my tips for building a excellent analytics pipeline that should start to show value incrementally.

**1. Stop dashboards with typical HR metrics.** Reports or dashboards with measures like attrition rates by line of business or recruitment metrics like time to fill can often lead to misinterpretation or hide core organisational issues as they are either too high in level or are not coupled with the right metrics. If placed out of context, time to fill can have a negative impact on the quality of the hire impacting new starter retention, lowering productivity/revenue or creating higher risk / lowering brand reputation.

Worst case, you create disengagement and a loss of interest to use data to help inform some of the most important decisions an organisation can make around its staff and you usually only get one shot with a stakeholder.

Most HR reporting outputs fall into two categories; reporting or insights with a recommendation to action. You should have the reporting aspect automated and centralised to ensure standard definitions and understanding, within this you should know who your customers are and measure demand. On the other side, an insight is usually temporary and available for a finite period until the organisation misses many opportunities and gets taken over, they do not survive or stagnate. The best way to start creating insights is by working on specific business problems through sprint programs lasting between 4-6 weeks. An executive should sponsor these and involve a technical lead, engagement manager and someone from the business.

**2. Start talking with the business to identify top organisational priorities and risks.** Key priorities could be raising revenue, decreasing operational cost, retention of top talent and building critical future capabilities with new products or operating model. These priorities then need to be broken down, i.e. How much of this can be controlled internally? What proportion of this relates to the workforce? With this information, you can then start to understand critical segments and where this relates to in the employment life cycle.

If you would like to start thinking about important HR metrics differently then, I suggest that you take a look at the <a href="https://www.visier.com/docs/en/Metric-definitions.pdf" target="_blank">metric dictionary</a> by Visier.

**3. Behavioural based analysis.** This may sound difficult to do, but it can be easy. This is about segmenting key workforce metrics against segments that best identify behavioural groups of employees.

The data that you have available should include information on the types of positions. This needs to be taken up a level to identify role and talent segments as supply and demand for particular roles have varied in the past ten years; we have seen a high demand for engineers that is starting to slow down and a high demand for specialist technology professionals. Behaviour by role will vary. Therefore grouping positions by function where statistically significant can help to shed light on retention, productivity and leadership issues and help to develop a strategy to improve. Try planning 2-3 years out at least and identify where there may be future labour shortfall or surplus to close any associated risks.

*<u>Additional thoughts</u>*

Analytics and insights can be personal, i.e. Using metrics to identify who are the talent accelerators in the organisation can be crucial to addressing talent gaps but can be very political and needs to be customised for each organisation with a clear case for why you are measuring it. You will be dealing with many data sources, information will not be properly structured and there will be data integrity issues, and managers will have different interpretations of what should work for them. Data integrity is crucial and presenting a crisp picture with a few data points as possible with a clear call to action will help to ensure the insight is not diluted or missed.

The technology aspect can be straightforward and may require no investment in the early stages, there is no one tool for the job, and there is no one company that has the silver bullet. Below are some great tools that you can use to speed up the delivery of insights and also highlight important data integrity issues as well as providing statistical validation.

**Tools of the trade**

<a href="https://www.postgresql.org/" target="_blank">Data Storage</a> - If you are using excel, reproducibility may be an issue and as you go deeper into a layered insight approach rebuilding the analytical data model many times will be labour intensive. If you don’t have a data warehouse, then I recommend installing a local SQL server until you can prove the business case for server funding. This version is free, and you can quickly load flat files downloaded from your HRIS coupled with various other data sources and join them to create analytical data sets.

<a href="https://www.continuum.io/downloads/" target="_blank">Anaconda, Python</a> - This version of Python is called Anaconda, not the snake. This tool may take a little bit of time to learn, and there are many free tutorials to get you started. What this tool enables you to do is quickly validate the data and perform statistical analysis to create robust and reproducible insights. With a few lines of code, you can call upon many different models using many different packages like <a href="https://www.scipy.org/" target="_blank">SciPy</a> to understand the importance of key variables to inform drivers. You can also go down the predictive model path as you start to develop data maturity.

![](/assets/images/generating_insights/ghr_1.jpg)

<a href="https://www.knime.org/" target="_blank">Knime</a> - This tool can be downloaded through the Anaconda tool. If you are starting in data science activities and want to move beyond descriptive analytics, then give this a go. It is straightforward to use, and you can create models and data visualisations with drag and drop functionality.

![](/assets/images/generating_insights/ghr_2.jpg)

<a href="https://www.r-project.org/" target="_blank">R</a> - if you are new to programming , R may be a little easier to use and has simular functionality to Python. I recommend installing <a href="https://www.rstudio.com/home/" target="_blank">R Studio</a>, this provides an easy to use interface. If you are interested in visualisations then install ggplot within R.

![](/assets/images/generating_insights/ghr_3.jpg)

<a href="http://www.cs.waikato.ac.nz/ml/weka/" target="_blank">Weka</a> - It seems all good things come out of NZ. This is another great data visualisation and machine learning tool. This tool is in between Knime and R/Python concerning ease of use and has been great for me to quickly validate distributions and isolate where data integrity issues exist before shaping the insight.

![](/assets/images/generating_insights/ghr_4.jpg)