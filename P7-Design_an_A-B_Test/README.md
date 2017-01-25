#P7: Design an A/B Test
*Submission By: Kellyn Khoo*  
*Submission Date: January 25th, 2017*  
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

* **Number of cookies**: *An ideal invariant metric as the cookies load before visitors see the experiment page, hence number of visitors should not vary between control and experiment groups.*
* **Number of user-ids**: *Not a good invariant metric because the number of visitors who enrolled in the free trial is dependent on the experiment. Also, not a good evaluation metric since this metric does not have a denominator hence it cannot be normalized for differences that exist between control and experiment groups.*
* **Number of clicks**: *Similar to* **Number of cookies** *, this is an ideal invariant metric because the clicks happened before visitors see the experiment page, and are thus independent of both control and experiment groups.*
* **Click-through-probability**: *Similar to* **Number of cookies** and **Number of clicks** *, this is an ideal invariant metric because the clicks happened before visitors see the experiment page, and are thus independent of both control and experiment groups.*
* **Gross conversion**: *Not a good invariant metric because the number of visitors who enrolled in the free trial is dependent on the experiment i.e. visitors who cannot agree to the "at least 5-hours per week" recommendation are suggested not to enroll. But it is a good evalution metric since it is dependent on the results of the experiment.*
* **Retention**: *Not a good invariant metric because the probability of payment after completing the 14-day free trial among number of user-ids who completed check-out is affected by the experiment. But it is a good evaluation metric since it is dependent on the results of the experiment.*   
* **Net conversion**: *Not a good invariant metric because the probability of payment after completing the 14-day free trial among number of unique cookies to click the "start free trial" button is affected by the experiment. But it is a good evaluation metric since it is dependent on the results of the experiment.*  

In the experiment, a "minimum 5-hour per week" screener is introduced with the objective to reduce non-committed enrollments i.e. reduce the number of non-payments after completing the 14-day free trial. Hence, in order to launch this experiment, I would require the evaluation metric **Gross conversion** to have a statistically significant decrease but not the evaluation metrics **Retention** and **Net conversion** i.e. to have no significant change or an increase. 

### Measuring Standard Deviation
> #### List the standard deviation of each of your evaluation metrics.  

As provided by [Wikipedia](https://en.wikipedia.org/wiki/Binomial_distribution), "in probability theory and statistics, the Binomial distribution with parameters n and p is the discrete probability distribution of the number of successes in a sequence of n independent yes/no experiments, each of which yields success with probability p. A success/failure experiment is also called a Bernoulli experiment or Bernoulli trial; when n = 1, the binomial distribution is a Bernoulli distribution. The binomial distribution is the basis for the popular binomial test of statistical significance."

In this experiment, our evaluation metrics **Gross conversion**, **Retention** and **Net conversion** are measures of probability to succeed, hence we can assume Bernoulli distribution with probability `p` and population `N`, where the standard deviation `stddev` is given as `sqrt(p*(1-p)/N)`:

| Evaluation Metrics    | N     | p          | stddev |
| --------------------- |:-----:| :--------: | :----: |
| Gross conversion      | 400   | 0.20625    | 0.0202 |
| Retention             | 82.5  | 0.53       | 0.0549 |
| Net conversion        | 400   | 0.1093125  | 0.0156 |

```
where:
Sample size of pageviews = 5,000
Click-through-probability on "Start free trial" = 0.08
Probability of enrolling, given click = 0.20625
Probability of payment, given enroll = 0.53
Probability of payment, given click = 0.1093125

hence:
N for Gross conversion = 5,000 x 0.08 = 400
N for Retention = 5,000 x 0.08 x 0.20625 = 82.5
N for Net conversion = 5,000 x 0.08 = 400

and:
stddev for Gross conversion = sqrt(0.20625 * (1-0.20625) / 400) = 0.0202
stddev for Retention = sqrt(0.53 * (1-0.53) / 82.5) = 0.0549
stddev for Net conversion = sqrt(0.1093125 * (1-0.1093125) / 400) = 0.0156
```

> #### For each of your evaluation metrics, indicate whether you think the analytic estimate would be comparable to the the empirical variability, or whether you expect them to be different.  

* For **Gross conversion**, the unit of analysis is the visitor who clicked the "start free trial" button, whilst the unit of diversion is the corresponding cookie. Generally, both of these are highly correlated unless the same visitor used different devices to visit the page. Hence, the analytic estimate for **Gross conversion** is likely to match its empirical variability.

* For **Retention**, the unit of analysis is the visitor who enrolled in the free 14-day trial, whilst the unit of diversion is the corresponding user-id. However, the **Number of user-ids** is not a unit of diversion in this experiment. Hence, the analytic estimate for **Retention** is likely to be different to its empirical variability.

* For **Net conversion**, this is similar to **Gross conversion** i.e. the unit of analysis is the visitor who clicked the "start free trial" button, whilst the unit of diversion is the corresponding cookie. Hence, the analytic estimate for **Net conversion** is likely to match its empirical variability.


### Sizing
#### Number of Samples vs. Power

> #### Indicate whether you will use the Bonferroni correction during your analysis phase.  

As provided by [Wikipedia](https://en.wikipedia.org/wiki/Bonferroni_correction), "... the Bonferroni correction can be conservative if there are a large number of tests and/or the test statistics are positively correlated."

In order to launch this experiment, I would require the evaluation metric **Gross conversion** to have a statistically significant decrease but not the evaluation metrics **Retention** and **Net conversion** i.e. test statistics are **not** positively correlated. So, I decided not to use the Bonferroni correction for my analysis as it would be too conservative.

> #### Give the number of pageviews you will need to power your experiment appropriately.  

Based on the [online calculator](http://www.evanmiller.org/ab-testing/sample-size.html) introduced in the classroom, the number of pageviews required for my experiment with alpha = 0.05 and beta = 0.20:

| Evaluation Metrics  | Baseline Conversion Rate  | Minimum Detectable Effect | Sample Size Needed | Number of Pageviews Needed|
| ------------------- |:-------------------------:| :-----------------------: | :----------------: | :-----------------------: |
| Gross conversion    | 20.625%    | 1%    | 25,835 per group  | 322,937.5 per group  |
| Retention           | 53%        | 1%    | 39,115 per group  | 2,370,606 per group  |  
| Net conversion      | 10.93125%  | 0.75% | 27,413 per group  | 342,662.5 per group  | 

```
where:
Number of unique cookies to view page per day = 40,000
Number of unique cookies to click "start free trial" per day = 3,200
Number of enrollments per day = 660

hence:
Number of pageviews needed for Gross conversion = 25,835 x 40,000 / 3,200 = 322,937.5 pageviews for each group
Number of pageviews needed for Retention = 39,115 x 40,000 / 660 = 2,370,606 pageviews for each group
Number of pageviews needed for Net conversion = 27,413 x 40,000 / 3,200 = 342,662.5 pageviews for each group
```
In view that the pageviews required for evaluation metric **Retention** is huge, I will no longer consider this metric for my further analysis. Hence, based on the evaluation metrics **Gross conversion** and **Net conversion**, I would require 342,662.5 x 2 = 685,325 pageviews to power this experiment.

#### Duration vs. Exposure

> #### Indicate what fraction of traffic you would divert to this experiment and, given this, how many days you would need to run the experiment.  

The calculations below consider that 100% of the daily traffic of 40,000 is diverted to the experiment. Based on the evalution metrics **Gross conversion** and **Net conversion**:

* Number of pageviews needed: 685,325
* Duration of Experiment = 685,325 / 40,000 = 17.1 days

From the above, noted that the duration of experiment is 17.1 days, which we would need to round-up to 18 days. If we round-down to 17 days, then we might not achieve the number of pageviews required. In conclusion, an experiment that requires 18 days would be deemed satisfactory.

> #### Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?

In this experiment, potential students are screened for their level of commitment i.e. if students can devote at least 5-hours per week in their choice of course. If potential students can agree to the recommended commitment, then students proceed to check-out as normal. However, potential students who hesitate to agree to the recommended commitment are not rejected, they are instead suggested not to enroll but still able to view videos and quizzes for free, without coaching support or a verified certificate. Hence, potential students do not suffer from physical, psychological or financial harm. During check-out process, potential students are asked for their credit card information, but this is independent of our experiment. The introduction of a screener does not burden Udacity's website and genuine potential students would still continue to enroll. In fact, the screener would encourage more potential students to enroll since the recommended commitment is reasonable i.e. minimum of 5-hours per week (it would be considered demanding if the recommended commitment is minimum 5-hours per day). Hence, in view that this experiment presents a low-risk for both Udacity and its potential students, I would prefer to divert 100% of traffic and complete the experiment within 18 days. Reducing the diverted traffic would require a longer experiment duration, which may increase some risks.

## Experiment Analysis

### Sanity Checks

> #### For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check.

| Invariant Metrics           | Lower 95% CI bound  | Upper 95% CI bound   | Observed Value | Sanity Check |
| --------------------------- |:-------------------:| :------------------: | :------------: | :----------: |
| Number of cookies       		| 0.4988    | 0.5012   | 0.5006 | Yes |
| Number of clicks            | 0.4959    | 0.5041   | 0.5005 | Yes |  
| Click-through-probability   | 0.0812    | 0.0830   | 0.0822 | Yes |

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

| Evaluation Metrics        | Lower 95% CI Bound  | Upper 95% CI Bound | Statistically Significant? | Practically Significant?|
| --------------------------|:-------------------:| :----------------: | :------------------------: | :---------------------: |
| Gross conversion       		| -0.0291   | -0.0120  | Yes  | Yes |
| Net conversion            | -0.0116   | 0.0019   | No   | No  |  

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

| Evaluation Metrics      | p-value  | Statistically Significant? |
| ----------------------- |:--------:| :------------------------: |
| Gross conversion    	  |  0.0026  | Yes |
| Net conversion          |  0.6776  | No  | 

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

I did not use Bonferroni correction because I wanted **Gross conversion** to significantly decrease whilst **Net conversion** to not significantly change (or increase). As mentioned earlier, since test statistics are **not** positively correlated, using the Bonferroni correction would be too conservative. 

Bonferroni correction would be used if it is expected that not all of the evaluation metrics would behave in the ways we expected. So, one false positive can affect our decision. In this case, the risk of type I error (i.e. when null hypothesis is true, but is rejected) would be very high and the Bonferroni correction can be used to reduce this risk by reducing alpha.

However, in this experiment, it is expected that all of the evaluation metrics would behave in the ways we expected. So, one false negative can affect our decision. Hence, the risk of type II error (i.e. when null hypothesis is false, but failed to be rejected) would be very high but we do not want to reduce the alpha. So, we do not want to use the Bonferroni correction.

Noted that for both effect size tests and sign tests, the **Gross conversion** in the experiment group is significantly lesser than the control group, whilst **Net conversion** in both control and experiment groups are not significantly different.

### Recommendation

The experiment significantly decreases **Gross conversion** but do not significantly decrease **Net conversion** i.e. a trial screener could significantly reduce non-paid enrollments, but not significantly reduce paid enrollments. However, the confidence interval for **Net conversion** includes negative numbers hence, there is a risk that the introduction of the trial screener may lead to a decrease in revenue. Thus, I do not recommend to launch this experiment.

## Follow-Up Experiment

> #### Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.

From my experience as a first-time Udacity student, the obvious gap between physical classroom learnings and online learnings is the lack of real-time support. Students can easily walk to their lecturers' room for real-time, face-to-face interactions. However, for online programs, students may email or post questions in forums and thereafter wait for responses, which may be delayed especially if there are time-zone differences. Hence, I suggest Udacity to consider setting up real-time support during the 14 days trial period i.e. send alerts to the students' respective facilitator that the student is currently online, hence the facilitator can be on standby for real-time interaction thru a chatbox.

My **null hypothesis** is that setting up real-time support for new trial signups will not increase **Net conversion** by a practically significant amount.

New free trial signups will randomly be assigned to a Control and an Experiment group. The experience for users in the Control group will remain unchanged. Users in the Experiment group will be assigned a random member of the Udacity team and receive real-time onboarding message thru a chatbox.

The **unit of diversion** will be the **user-id**, as this change only impacts what happens after a free trial account is created.

The **invariant metric** is **Number of user-ids**, because users signed up for free trials even before they are assigned a point of contact and are exposed to the new onboarding messages.

The **evaluation metric** is **Net conversion**, whether real-time support helps increase the ratio of users who make payment over those who decided only to try the program.

If the **Net conversion** is positive and practically significant at the end of the experiment, then we can implement real-time support to all Udacity's nanodegree programs.
