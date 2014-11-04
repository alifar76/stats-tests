#Are female crowdfunding profiles funded faster than male profiles?

My eventual goal is to create a machine learning model to predict how long it takes to fully fund a profile. But first, let's do some exploratory data analysis and basic inferential statistics to see if there are differences in the time it takes to fund a profile by sex.

##Exploratory data analysis

Both distributions are heavily right-skewed.

![histogram](https://raw.githubusercontent.com/aok1425/stats-tests/master/imgs/histogram.png "")

![boxplot](https://github.com/aok1425/stats-tests/raw/master/imgs/boxplot.png "")

The difference in the average number of hours to fund a male profile and a female profile is 23 hours. 

The difference in the median number of hours is 11 hours.

##Why is there a difference?

My first guess is that male profiles may have higher amount requests than female profiles. Let's look at the distribution of funding amounts by sex.

![target_amt1](https://github.com/aok1425/stats-tests/raw/master/imgs/target_amt1.png "")

I know that females often have a profile that always has the same request amount, and is unique only to female profiles. Let's remove all profiles with that request amount of $215.

![target_amt2](https://github.com/aok1425/stats-tests/raw/master/imgs/target_amt2.png "")

Now, the request amounts are more similar than before.

After correcting for request amount, the difference between the averages is 20.5 hours. The difference between the medians is 10.5 hours. So those deleted profiles didn't account for much of a difference.

[show updated box-and-whiskers and histogram?]

##Is the difference signficant?

Before we determine whether the two distributions are statistically significantly different, we need to choose what measure we will use to define the difference. A popular choice is the mean. But because of the large amount of variance and the right-skewness, the mean is very sensitive to the few outlying large values.

A better choice is the median. It is less sensitive to outliers.

Let's perform a significance test based on the medians, a Wilcoxon Rank Sum Test. It is the non-parametric equivalent of the two-sample t-test, and it does not require a Normal distribution.

###Wilcoxon Rank Sum Test

The Wilcoxon Rank Sum Test tests whether two groups have indentical distributions.

Our null hypothesis is that the two sets of measurements are drawn from the same distribution. Our alternative hypothesis is that the values in one sample are more likely to be larger than the values in the other sample.

According to the Wilcoxon Rank Sum test, if we compare the distribution of the number of hours to fund a male profile and the number of hours to fund a female profile, there is a statistically signficant difference between the two. 

However, if we remove the pregnancies from the female distribution, we can't reject the null hypothesis, and we can't say that it takes more hours to fund a male profile than a female, or vice-versa. If we assume that the male distribution and the female distribution minus the pregnancies are each drawn from the same distribution, the chance that we would get the Rank Sum result that we did is 11%. This result is not unlikely enough that we would reject our initial assumption.

##Conclusion

Our parametric tests, which looked at the difference between the averages, stated that there is about a 20-hour, statistically significant difference between the average time to fund a male profile and the average time to fund a female profile. 

When we use our non-parametric tests, which look at the median instead of the average, we find that there is not a statistically significant difference between the two groups after we remove the pregnancies.

####Is it valid to remove the pregnancies?

Ideally, the two groups would have profiles with the same distibution of funding request amounts, and each pair of  male-female profiles with the same funding request amount would be presented to the user at the same time. This is the ideal of the A/B test. Because we can't achieve this ideal, we want the two distributions to be on as much of an 'equal playing field' as possible. Removing the pregnancies makes the distributions of the male and female funding request amounts more similar.

On the other hand, by simply removing a subset of profiles, we've removed the instances in which users would have donated to other profiles, and we'll never know how that would have affected the male and female distributions.

So while there is a downside to removing a subset of profiles, I think the fact that the fact that the funding request amount distributions are more equal justifies the action.

####Which test to give more weight to, the one that focuses on the mean, or the one that focuses on the median?

If we look back to the distributions of the two groups, the variance is large and right-skewed. That means that when a new profile is funded, the amount of time it took could be close to the mean, but it could also be far away from the mean, because of the large variance.

For that reason, when trying to decide which metric to use to define whether there is a difference between these two distribtions, I think the median is a more representative metric than the mean. The median is less affected by a large variance than the mean.

####In conclusion

The median is a better indicator of the difference between these two distributions because of the large variance they each have. If we use a median-based nonparametric signficance test, we find that there is a statistically significant difference between the distribution of the number of hours to fund a male profile and the number of hours to fund a female profile.

However, after we account for pregnancy profiles, which all have the same cost and which only females have, we find that there is not a significant difference between the two distributions. We also know that nonparametric tests are less sensitive to detect differences than parametric tests.

In conclusion, we can't say that the number of hours to fully fund a male profile is more likely to be larger than the number of hours to fund a female profile, or vice-versa.