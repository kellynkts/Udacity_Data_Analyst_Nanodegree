#P6: Make Effective Data Visualization
*Submission By: Kellyn Khoo*  
*Submission Date: January 12th, 2017*  
*Submission Purpose: Part-Evaluation for Udacity Data Analyst Nanodegree*  

###1. Summary 

The objective of this project is to select a dataset and create an explanatory data visualization using either dimple.js or d3.js., thereafter document the findings.  

I have chosen one of the beginner dataset i.e. "Titanic". Variables available for data analysis include passenger class, name, gender, age, number of siblings/spouses aboard, number of parents/children aboard, ticket number, passenger fare, cabin, port of embarkation and survival indicator.  

I have created three data visualization for my analysis:  
(a) Survival Rate by Passenger Class  
(b) Survival Rate by Age Group  
(c) Survival Rate by Passenger Class and Gender  

###2. Design  

* In view that passengers' information are classified into categories, bar charts would be best to visualize Titanic dataset.  
* As the survival status comprises only two values, I have selected stacked bar charts for my analysis.
* Prior to feedbacks, data on y-axis is represented by number of passengers. Post feedbacks, I have improved my analysis by using ratios.
* For chart legend, I have placed this on the top right corner of each chart.  

[Initial Visualization](https://bl.ocks.org/kellynkts/raw/5b6a8852c02947eb71efe895a258bee3/)

###3. Feedback  

I gathered feedback from 3 different people, where I referred to Udacity questions' guideline and here are the abridged responses:

####Feeback #1 - from my husband:  
> You should use ratios. Currently it is difficult to judge which category has better survival rate. You should consider renaming "Non-Survivors" as "Casualties" or "Victims". The name "Non-Survivors" is too long especially when it is depicted in the legend section of the third chart.  

####Feedback #2 - from my colleague: 
> You should use standardized colours for "Survivors" and "Non-Survivors", but perhaps in different shades for each chart. You should rename "Non-Survivors", it is too long. This long name makes the third chart looks messy.  

####Feedback #3 - from my supervisor:
> To show survival rate, you should first calculate the ratio of "Survivors" vs. "Non-Survivors". By showing the number of passengers who survived and did not survived is meaningless. For example, in the second chart, you mentioned that age group < 18 has the highest survival rate, but this is not clearly shown. From my perspective, it looks like age group 25-59 has the highest survival rate, since it has the highest number of survivors.  

[Final Visualization](https://bl.ocks.org/kellynkts/raw/f594dc4340abb08c821fa5bc0a9a560c/)

###4. Resources  

* [mbostock](https://bl.ocks.org/mbostock)
* [d3 documentation's](https://github.com/d3/d3/blob/master/API.md)
* [DashingD3js](https://www.dashingd3js.com/table-of-contents)
* [Color Brewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3)

  

