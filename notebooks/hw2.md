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

       Each one is Bernoulli distributed.
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

       Each one is Bernoulli distributed.
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

6. Which of the following are true about frequentist A/B tests? (True/False)

* [**True**] It does not tell us the magnitude of the difference between control and test groups.

* [**False**] We can never know when to stop the experiments.

* [**True**] We can never determine if the null hypothesis being true.

* [**False**] We can run one or as many experiments as we want using the same significance level.

* [**True**] If we have too many samples in each group, the validity of the test can be jeopardized.

* [**False**] If you have set up the experiment based on desired minimum detectable effect and significance level, statististical significance is the only factor in determining which group is the better one.

* [**False**] We can only test difference between two proportions.

* [**False**] More samples in control and test groups are always better.
