#P7: Design an A/B Test
*Submission By: Kellyn Khoo*  
*Submission Date: January 20th, 2017*  
*Submission Purpose: Part-Evaluation for Udacity Data Analyst Nanodegree*  

## Experiment Overview: Free Trial Screener
At the time of this experiment, Udacity courses have two options on the home page: "start free trial", and "access course materials". For students who clicked "start free trial", they will need to enter credit card information, before being enrolled in a 14-day free trial. For students who clicked "access course materials", they will be able to view videos and quizzes for free, but they will not receive coaching support or a verified certificate. 

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time can they devote to the course. If the student indicated >=5 hours per week, they would be taken through the checkout process as usual. Otherwise, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free instead. 

## Experiment Design
### Metric Choice
> #### List which metrics you will use as invariant metrics and evaluation metrics here.

* **Invariant metrics**: Number of cookies, Number of clicks, Click-through-probability
* **Evaluation metrics**: Gross conversion, Retention, Net conversion

> #### For each metric, explain both why you did or did not use it as an invariant metric and why you did or did not use it as an evaluation metric. Also, state what results you will look for in your evaluation metrics in order to launch the experiment.

* **Number of cookies**: *An ideal invariant metric as the cookies load before users see the experiment page.*
* **Number of user-ids**: *Not a good invariant metric because the number of users who enrolled in the free trial is dependent on the experiment. Also, not a good evaluation metric because the number of visitors may be different between the control and experiment groups.*
* **Number of clicks**: *An ideal invariant metric because the clicks happened before the users see the experiment page, and are thus independent of both control and experiment groups.*
* **Click-through-probability**: *Also a good invariant metric because the clicks happened before the users see the experiment page, and are thus independent of both control and experiment groups.*
* **Gross conversion**: *Not a good invariant metric because users who indicate that they will not be able to work more than 5 hours per week, they are suggested not to enroll. Gross conversion is about probability to succeed, and it is a suitable evaluation metric because it is directly dependent on the effect of the experiment.*
* **Retention**: *Not a suitable invariant metric because the number of users who enroll in the free trial is dependent on the experiment. Suitable evaluation metric because it is directly dependent on the effect of the experiment.*
* **Net conversion**: *Not a suitable invariant metric because the number of users who enroll in the free trial is dependent on the experiment. Suitable evaluation metric because it is directly dependent on the effect of the experiment.*

I would launch the experiment only when the number of enrollments are reduced but number of payments do not reduce. In other words, I require **Gross conversion** to have a statistically significant decrease, and **Net conversion** to statistically remain the same (or increase).

