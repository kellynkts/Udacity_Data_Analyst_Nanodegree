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
Pageviews = 5,000
N = Pageviews * Click-through-probability on "Start free trial"
  = 5,000 * 0.08
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
* **Gross conversion:** 25,835 x 40,000 / 3,200 = 322,937.5 pageviews for each group
* **Retention:** 39,115 x 40,000 x 660 = 2,370,606 pageviews for each group
* **Net conversion:** 27,413 x 40,000 / 3,200 = 342,662.5 pageviews for each group

*where:   
40,000 = number of unique cookies to view page per day  
3,200 = number of unique cookies to click "Start free trial" per day  
660 = number of enrollments per day*  

In view that the pageviews required for evaluation metric **Retention** is huge, I will no longer consider this metric for my further analysis. Hence, based on **Gross conversion** and **Net conversion**, I would require 342,662.5 x 2 = 685,325 pageviews to power this experiment.

#### Duration vs. Exposure

> #### Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment.  

The calculations below consider that 100% of the daily traffic of 40,000 is diverted to the experiment. Based on the evalution metric **Gross conversion** and **Net conversion**:

* Number of pageviews: 685,325
* Duration of Experiment = 685,325 / 40,000 = 17.1 days 

An experiment that spans for 17 days seems adequate. 

## Experiment Analysis

### Sanity Checks

> #### For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check.

| Metrics                    | Lower bound  | Upper bound | Observed | Sanity check |
| ----------------------------- |:--------:| :-----------: | :-------: | :----------: |
| Number of cookies       		  | 0.4988   | 0.5012   | 0.5006 | Yes |
| Number of clicks             | 0.4959   | 0.5041   | 0.5005 | Yes |  
| Click-through-probability     | 0.0812   | 0.0830   | 0.0822 | Yes |

##### **Number of cookies**

```
Control group: number of pageviews = 345,543
Experiment group: number of pageviews = 344,660
stddev = sqrt(0.5 x 0.5 / (345,543 + 344,660)) = 0.000602
m, margin of error = stddev x 1.96 = 0.00118 ;  where the z-score for alpha 0.05 is 1.96
cinterval = (0.5 - 0.00118, 0.5 + 0.00118) = (0.4988, 0.5012)
p = 345,543 / (345,543 + 344,660) = 0.5006

As the observed value, p, is within the confidence interval, cinterval, hence it passed the sanity check.
```
##### **Number of clicks:**

```
Control group: number of clicks = 28,378
Experiment group: number of clicks = 28,325
stddev = sqrt(0.5 x 0.5 / (28,378 + 28,325)) = 0.0021
m, margin of error = stddev x 1.96 = 0.004116 ;  where the z-score for alpha 0.05 is 1.96
cinterval = (0.5 - 0.004116, 0.5 + 0.004116) = (0.4959, 0.5041)
p = 28,378 / (28,378 + 28,325) = 0.5005

As the observed value, p, is within the confidence interval, cinterval, hence it passed the sanity check.
```
##### **Click-through-probability:**

```
Control group: click-through-probability = 28,378 / 345,543 = 0.0821
Experiment group: click-through-probability = 28,325 / 344,660 = 0.0822
stddev = sqrt(0.0821 x (1-0.0821) / 345,543) = 0.0005
m, margin of error = stddev x 1.96 = 0.0009 ;  where the z-score for alpha 0.05 is 1.96
cinterval = (0.0821 - 0.0009, 0.0821 + 0.0009) = (0.0812, 0.0830)
p = 28,325 / 344,660 = 0.0822

As the observed value, p, is within the confidence interval, cinterval, hence it passed the sanity check.
```

### Result Analysis

#### Effect Size Tests

> #### For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant.

| Metrics                    | Lower bound  | Upper bound | Statistically significant | Practically significant|
| ----------------------------- |:--------:| :-----------: | :-------: | :----------: |
| Gross conversion       		  | -0.0291   | -0.0120   | Yes | Yes |
| Net conversion            | -0.0116   | 0.0019   | No | No |  

##### **Gross conversion:**

```
Control group: number of enrollment = 3,785
Control group: number of clicks = 17,293
Experiment group: number of enrollment = 3,423
Experiment group: number of clicks = 17,260

p = (3,423 + 3,785) / (17,260 + 17,293)
  = 0.2086

SE = sqrt(p x (1-p) x (1/N1 + 1/N2))
   = sqrt(0.2086 x (1 - 0.2086) x (1/17,293 + 1/17,260))
   = 0.00437

d = (3,423 / 17,260) - (3,785 / 17,293)
  = −0.02055

m = SE * 1.96     ;  where the z-score for alpha 0.05 is 1.96
  = 0.00857

cinterval = (d - m, d + m)
          = (−0.0291, −0.0120)
          
Since 0 is not within the (−0.0291, −0.0120) range, *Gross conversion* in the experiment group is both 
statistically significant and practically significant different than *Gross conversion* in the control group.           
```

##### **Net conversion:**

```
Control group: number of payments = 2,033
Control group: number of clicks = 17,293
Experiment group: number of payments = 1,945
Experiment group: number of clicks = 17,260

p = (1,945 + 2,033) / (17,260 + 17,293)
  = 0.1151

SE = sqrt(p x (1-p) x (1/N1 + 1/N2))
   = sqrt(0.1151 x (1 - 0.1151) x (1/17,293 + 1/17,260))
   = 0.00343

d = (1,945 / 17,260) - (2,033 / 17,293)
  = = −0.00487

m = SE * 1.96     ;  where the z-score for alpha 0.05 is 1.96
  = 0.00673

cinterval = (d - m, d + m)
          = (−0.0116, 0.0019)
          
Since 0 is within the (−0.0116, 0.0019) range, *Net conversion* in the experiment group is neither 
statistically significant nor practically significant different from *Net conversion* in the control group.          
```

#### Sign Tests

> #### For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant

| Metrics                    | p-value | Statistically significant |
| ----------------------------- |:--------:| :-----------: | :-------: | :----------: |
| Gross conversion       		  |  0.0026   | Yes |
| Net conversion            |  0.6776   | No  | 

The results are according to the following [calculator](http://graphpad.com/quickcalcs/binomial1.cfm).

##### **Gross conversion:**
```
Number of “successes” you observed = 4
Number of trials or experiments = 23
Probability = 0.5

The two-tailed p-value is 0.0026. As p-value 0.0026 < alpha 0.05, the *Gross conversion*
in the experiment group is significantly lesser than the control group.
```

##### **Net conversion:**
```
Number of “successes” you observed = 10
Number of trials or experiments = 23
Probability = 0.5

The two-tailed p value is 0.6776. As p-value 0.6776 > alpha 0.05, the *Net conversion*
in the experiment group is not significantly different than the control group.
```

#### Summary

> #### State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.

I did not use Bonferroni correction because I wanted **Gross conversion** to significantly decrease whilst **Net conversion** to not significantly decrease. For both effect size tests and sign tests, the **Gross conversion** in the experiment group is significantly lesser than the control group, whilst **Net conversion** in both control and experiment groups are not significantly different.

