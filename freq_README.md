#Are female crowdfunding profiles funded faster than male profiles?

My eventual goal is to create a machine learning model to predict how long it takes to fully fund a profile. But first, let's do some exploratory data analysis and basic inferential statistics to see if there are differences in the time it takes to fund a profile by sex.

##Exploratory data analysis

Both distributions are heavily right-skewed.

![histogram](https://raw.githubusercontent.com/aok1425/stats-tests/master/imgs/histogram.png "")

![boxplot](https://github.com/aok1425/stats-tests/raw/master/imgs/boxplot.png "")

The difference in the average number of hours to fund a male profile and a female profile is 23.2 hours. The difference in the median number of hours is 11.0 hours.

##Why is there a difference?

My first guess is that male profiles may have higher amount requests than female profiles. Let's look at the distribution of funding amounts by sex.

![target_amt1](https://github.com/aok1425/stats-tests/raw/master/imgs/target_amt1.png "")

I know that females often have a profile that always has the same request amount, and is unique only to female profiles. Let's remove all profiles with that request amount of $215.

![target_amt2](https://github.com/aok1425/stats-tests/raw/master/imgs/target_amt2.png "")

Now, the request amounts are more similar than before.

After correcting for request amount, the difference between the averages is 20.5 hours. The difference between the medians is 10.5 hours. So those deleted profiles didn't account for much of a difference, at least regarding these two measures.

##Is the difference statistically signficant?

Before we determine whether the two distributions are statistically significantly different, we need to choose what measure we will use to define the difference. A popular choice is the mean. But because of the large amount of variance and the right-skewness, the mean is very sensitive to the few outlying large values.

A better choice is the median. It is less sensitive to outliers.

Let's perform a significance test based on the medians, a Wilcoxon Rank Sum Test. It is the non-parametric equivalent of the two-sample t-test, and it does not require a Normal distribution.

###Wilcoxon Rank Sum Test

The Wilcoxon Rank Sum Test tests whether two groups have indentical distributions.

Our null hypothesis is that the two sets of measurements are drawn from the same distribution. Our alternative hypothesis is that the values in one sample are more likely to be larger than the values in the other sample.

According to the Wilcoxon Rank Sum test, if we compare the distributions of the number of hours to fund a male profile and the number of hours to fund a female profile, there is a statistically signficant difference between the two, with `p = 0.03`. 

However, if we remove the $215 profiles from the female distribution, we can't reject the null hypothesis, and we can't say that it takes more hours to fund a male profile than a female, or vice-versa. 

If we assume that the male distribution and the female distribution minus the $215 profiles are each drawn from the same distribution, the chance that we would get the Rank Sum result that we did is 11% (`p = 0.11`). This result is not unlikely enough that we would reject our initial assumption.

##Conclusion

We decided that a median-based, non-parametric significance test would be the most appropriate way to compare the two distributions, because of the large amount of variance in the distributions.

When we use our Wilcoxon Rank Sum Test, we don't have enough evidence to say that the two distributions are different after we remove the $215 profiles.

####Is it valid to remove the $215 profiles?

Ideally, the two groups would have profiles with the same distibution of funding request amounts, and each pair of male-female profiles with the same funding request amount would be presented to the user at the same time. This is the ideal of the A/B test. Because we can't achieve this ideal, we want the two distributions to be on as much of an 'equal playing field' as possible. Removing the $215 profiles makes the distributions of the male and female funding request amounts more similar.

On the other hand, by simply removing a subset of profiles, we've removed the instances in which users would have donated to other profiles. We'll never know how that would have affected the male and female distributions.

So while there is a downside to removing a subset of profiles, I think the fact that the fact that the funding request amount distributions are more equal justifies the action.

##Next steps

Via the methods we used above, we answered the question:

> If we replicated this experiment many times, with different results each time, what is the likelihood that we would get the female distribution (minus the $215 profiles) to be as dissimilar from the male distribution as we did?

We looked at long-run frequency. We did not answer the question:

> What is the probability that a male profile takes longer to fund than a female profile, knowing what we know from this single experiment?

because we defined 'likelihood' as the number of experiements where the two distributions would be similar, divided by the total number of experiments.

With Bayesian methods, we can answer the probability question. The interpretation of its results is more intuitive, and we can focus on the probability of *this* experiment, as opposed to many trials of the experiment. The downside to the Bayesian approach is that we have to state our subjective belief that the distributions were similar or different before the experiment took place.

So in the next notebook, I will try a Bayesian approach to find if female profiles are funded faster than male profiles.

If you would like to see a more comprehensive assessment of the methods we used above, [view the full iPython notebook](http://nbviewer.ipython.org/github/aok1425/stats-tests/blob/master/frequentist.ipynb).