#P6: Make Effective Data Visualization
*Submission By: Kellyn Khoo*  
*Submission Date: January 15th, 2017*  
*Submission Purpose: Part-Evaluation for Udacity Data Analyst Nanodegree*  

###1. Summary 

The objective of this project is to select a dataset and create an explanatory data visualization using either dimple.js or d3.js., thereafter document the findings.  

I have chosen one of the beginner dataset i.e. "Titanic". Variables available for data analysis include passenger class, name, gender, age, number of siblings/spouses aboard, number of parents/children aboard, ticket number, passenger fare, cabin, port of embarkation and survival indicator.  

*Background*  
The sinking of the RMS Titanic in April 1912 is one of the deadliest peacetime maritime disasters in history. From the estimated 2,224 passengers onboard, more than 1,500 died when the RMS Titanic struck an iceberg and sank in the North Atlantic Ocean.  

*Purpose of Analysis*  
To study the survival rate in the Titanic's disaster, I have referred to the provided data file, where information was collected from a sample of 891 passengers.Information available include passenger class, name, gender, age, number of siblings/spouses aboard, number of parents/children aboard, ticket number, passenger fare, cabin, port of embarkation and survival indicator.  

My first experience with this disaster is from the movie "Titanic" that was released in 1997. From the movie, I learnt that survival priority is given to passenger class, children and women.Hence, I expect that my analysis should support my prior knowledge:  

Chart 1 - To ascertain my prior knowledge that first class passengers have better chance of survival  
Chart 2 - To ascertain my prior knowledge that children passengers have better chance of survival  
Chart 3 - To ascertain my prior knowledge that female passengers have better chance of survival  

*Conclusion*  
From the three charts created, I could conclude that indeed a passenger's survival onboard Titanic is affected by three factors i.e. passenger class, age and gender.

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

[Final Visualization](https://bl.ocks.org/kellynkts/raw/fb24e060f0b3f1d89634eab359c06340/)

###4. Resources  

* [mbostock](https://bl.ocks.org/mbostock)
* [d3 documentation's](https://github.com/d3/d3/blob/master/API.md)
* [DashingD3js](https://www.dashingd3js.com/table-of-contents)
* [Color Brewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3)

  

