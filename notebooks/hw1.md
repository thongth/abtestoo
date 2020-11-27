# Homework #1 - A/B Testing in the Wild

1. Which of the following use cases can you reliably conduct an A/B test? (True/False)

* [**True**] Frontend person wants to change color of the 'Go' button on a search bar. Will it increase conversion rate?

* [**True**] The data team created four versions of machine learning model for product recommendations to new users of an app. Which one is the best?

* [**False**] Two managers from different factions have Layout A and Layout B for a physical convenience store. Which one should we use?

* [**False**] Mr. Rabbito thinks offline stores are the best channel to distribute our products, whereas Ms. Rakko thinks online websites are the way to go. Who is right?

* [**True**] Your boss wants to add a premium version to your freemium service. Is it a good idea?

* [**True**] The backend team came up with a new setup that they think will speed up the website load time. Should we implement this change?

* [**False**] Kuruma Inc., a car dealer, wants to change the banner on their homepage to see if it will attract more repeated customers. Average time between purchase of the car company is 5 years. How do you know if the banner change has an effect? 

* [**False**] Your company undergoes a total revamp of its corporate identity. Is it the right call?

* [**True**] Elastic ninja at your company wants to show 15 products on the first page of search results instead of 20 products. Should you allow them?

* [**False**] Marketing person wants to know who respond better to our ads campaigns between iOS users and Android users. How to tell?

2. What are the metrics you should use for the following A/B tests? Assume that the granularities are: page views and unique visitors.

* Which button colors will make customers find it more easily? clicks / **page views**

* Which sets of products on a landing page will make customers more likely to buy? purchases / **page views**

* Which types of promotion coupons will be more effective? purchases / **unique visitors**

* Which website layouts will attract more customers to click on sign up button? clicks / **unique visitors**

3. Based on the transaction table below, what are the event-based conversion rate and cohort-based conversion rate of 2020-11? Assume 7-day attribution period. Conversion rate is calculated by purchases / unique users.

| date       | user | event    |
|------------|------|----------|
| 2020-11-01 | A    | visit    |
| 2020-11-01 | A    | purchase |
| 2020-11-05 | B    | visit    |
| 2020-11-13 | B    | visit    |
| 2020-11-30 | C    | visit    |
| 2020-12-05 | C    | purchase |

**ANS** event-based conversion rate of 2020-11 is 1/3 and cohort-based conversion rate is 2/3

4. Give 3 examples of values that are usually distributed in the following manner (do not use examples from class):

* Bernoulli/Binomial distributions: **Gender of newborn child, Head/Tail, Win/Lose**

* Normal/Student t's distribution: **Height, IQ, Shoe size**

* Exponential distribution: **time until a radioactive particle decays, time it takes before your next telephone call, distance between mutations on a DNA strand**

* Poisson distribution: **photons arriving at a telescope, customers arriving at a counter or call centre, number of decays in a given time interval in a radioactive sample**

5. Which variables should you control for in an A/B test of the following cases?

* We want to test if SMOKING -> CANCER (Smoking causes cancer) and we know that AGE -> SMOKING and AGE -> CANCER. We should control for ______

* We want to test if GUN OWNERSHIP -> CRIMES and we know that GUN OWNERSHIP -> GUN SALES and CRIMES -> GUN SALES. We should control for ______

* We want to test if CROP BURNING -> LUNG DISEASES and we know that CROP BURNING -> PM2.5 and PM2.5 -> LUNG DISEASES. We should control for ______

# Homework #2 - A/B Testing in the Wild

1. The Law of Large Numbers (LLN) says that sample mean will converge to expectation as sample size grows. Assuming that this is true, prove that sample variance will converge to variance as sample size grows. 

2. What is p-value? (Choose one or more)

* [x] Assuming that the null hypothesis is true, what is the probability of observing the current data.

* [ ] Based on the observed data, what is the probability of the null hypothesis being true.

* [ ] Based on the observed data, what is the probability of the null hypothesis being false.

* [ ] Assuming that our hypothesis is true, what is the chance that we reject the null hypothesis.

3. If we conduct a frequentist statistical test at 5% significance level repeatedly for 4,000 times, how many times can we expect to have statistically significant results even if group A and B are exactly the same?

**(5 / 100) * 4000 = 200 times**

4. Hamster Inc. once again wants to test the conversion rates between package colors of its sunflower seeds; this time it is Red Package vs Gold Package. The Red Package is the existing group with average conversion rate of 11%. If they think the minimum detectable effect is 1% and want to make a 80/20 control/test split, how many unique users should see each package color before we decide which one performs better? Assume that they are testing at significance level of 15%. Show your work.

**ANS** 

        m = 1/4
        From the formula: n = (m+1)/m * (Za*sigma/MDE)^2
        Find the sigma: Since the conversion distributed as bernoulli distribution, we know that p = mean. And we can find the variance from p(1-p) = 0.11(0.89) = 0.0979
        Z value at significant value 0.15 = 1.04
        Substitute into the formula: n = 5 * (1.04^2*0.0979) / 0.01^2 = 5294 and m = 1/4 * 5294 = 1323

5. Let us say Hamster Inc. ran the experiment and got the following results. 

| campaign_id | clicks | conv_cnt | conv_per |
|------------:|-------:|---------:|---------:|
|         Red |  59504 |     5901 | 0.099170 |
|        Gold |  58944 |     6012 | 0.101995 |

5.1 At significance level of 7%, which variation should be chosen to run at 100% traffic? Show your work.

**ANS** 

       Each one is Bernoulli distributed. Assumed that we are using red and deciding to change to gold.
       H0: mean_red - mean_gold = 0
       Ha: mean_red - mean_gold < 0
       The pooled variance for this data is [(n-1)*sr^2 + (m-1)*sg^2] / (n+m-2) * (1/n + 1/m) 
                                          = [59503*0.099170(1-0.099170) + 58943*0.101995(1-0.101995)] / (59504+58944-2) * (1/59504 + 1/58944)
                                          = 3.0549*10^-6
       t-value = (red_mean - gold_mean) / pooled s.d.
               = (0.099170 - 0.101995) / sqrt(3.0549*10^-6)
               = -1.616
       P(T > |-1.616|) = P(T > 1.616) = 0.053
       Since o.o53 < 0.07, we reject H0 in favor of Ha at significant level 0.07. So we will choose the gold one.

5.2 What are the confidence intervals at 7% significance of conversion rates for Red and Gold? Show your work.

**ANS** 

       for red: interval = 0.09917 + 1.812*sqrt(0.08933/59504) = 0.09917 +or- 0.0022 
       for gold: interval = 0.101995 + 1.812*sqrt(0.09159/58944) = 0.101995 +or- 0.0026 

6. Which of the following are true about frequentist A/B tests? (True/False)

* [**True**] It does not tell us the magnitude of the difference between control and test groups.

* [**False**] We can never know when to stop the experiments.

* [**True**] We can never determine if the null hypothesis being true.

* [**False**] We can run one or as many experiments as we want using the same significance level.

* [**True**] If we have too many samples in each group, the validity of the test can be jeopardized.

* [**False**] If you have set up the experiment based on desired minimum detectable effect and significance level, statististical significance is the only factor in determining which group is the better one.

* [**False**] We can only test difference between two proportions.

* [**False**] More samples in control and test groups are always better.

