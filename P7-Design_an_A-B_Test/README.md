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

### Measuring Standard Deviation
> #### List the standard deviation of each of your evaluation metrics.  

To evaluate whether the analytical estimates of standard deviation are accurate and match empirical standard deviation, the unit of analysis and unit of diversion are compared for each evaluation metric. Assuming Bernoulli distribution with probability `p` and population `N`, the standard deviation `stddev` is given by `sqrt(p*(1-p)/N)`.

```
Pageviews = 5000
N = Pageviews * Click-through-probability on "Start free trial"
  = 5000 * 0.08
  = 400
```

#### Gross conversion

```
p = Probability of enrolling, given click = 0.20625
stddev = sqrt(0.20625 * (1-0.20625) / 400) = 0.0202
```

#### Net conversion
```
p = Probability of payment, given click = 0.1093125
stddev = sqrt(0.1093125 * (1-0.1093125) / 400) = 0.0156
```

#### Retention
```
p = Probability of payment, given enroll = 0.53
number of enrollment = N * Probability of enrolling, given click
                     = 400 × 0.20625 
                     = 82.5
stddev = sqrt(0.53 * (1-0.53) / 82.5) = 0.0549
```

> #### For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the the empirical variability, or whether you expect them to be different.  

For **Gross conversion** and **Net conversion**, their denominators are number of cookies (unique cookies that click the "start free trial"), which is also unit of diversion, so their analytic variance are likely to match their empirical variance. For **retention**, its denominator is **number of enrollments**, which is not the unit of diversion in the experiment, so its empirical variance might be much higher than analytic variance.

### Sizing
#### Number of Samples vs. Power

> #### Indicate whether you will use the Bonferroni correction during your analysis phase.  

I did not use the Bonferroni correction because I wanted **Gross conversion** to significantly decrease and **Net conversion** to not significantly decrease. Bonferroni correction is suitable when we are dealing with an *or* case but not suitable when use in an *and* case. For this experiment, we have an *and* case for our metrics because we are more concerned with false negatives than false positives (i.e. Bonferroni correction controls for false positives). Furthermore, the use of the Bonfferoni reduces statistical power (i.e. we might miss differences that actually exist).

> #### Give the number of pageviews you will need to power your experiment appropriately.  

alpha = 0.05; beta = 0.20

* **Gross conversion:** base conversion rate = 0.20625, dmin = 1%,
* **Retention:** base conversion rate = 0.53, dmin = 1%,
* **Net conversion:** base conversion rate = 0.1093125, dmin = 0.75%,

where dmin is the minimum Detectable Effect.

Using the [calculator](http://www.evanmiller.org/ab-testing/sample-size.html) referred in the classes, the number of samples:
* **Gross conversion:** 25,835 clicks for each group
* **Retention:** 39,115 enrollments for each group
* **Net conversion:** 27,413 clicks for each group

Hence, the number of pageviews required are:
* **Gross conversion:** 25,835 x 40000 / 3200 = 322,937.5 pageviews for each group
* **Retention:** 39,115 x 40000 x 660 = 237,0606 pageviews for each group
* **Net conversion:** 27,413 x 40000 / 3200 = 342,662.5 pageviews for each group

