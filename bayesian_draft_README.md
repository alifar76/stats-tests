#Using a Bayesian approach: Are female crowdfunding profiles funded faster than male profiles?

A Bayesian approach allows us to make direct probability statements. The interpretation of its results is simpler than interpreting p-values or confidence intervals. 

The main drawback to a Bayesian approach is that we must specify our prior belief about the situation, and this inserts some subjectiveness into our calculation. Bayesians respond that we can create very weak priors that don't much affect the results, which will allow the data to dominate the final result.

In this case, our priors are a) the average number of hours to fund a male patient, and b) the average number of hours to fund a female patient. We will look at the data to inform our 'prior' belief; this method is called Empirical Bayes. We will also test various priors to see if they affect our final result much.

##Viewing our data as a Poisson process

The data we have is the amount of time it takes to fund a profile. For each profile, we can think of the time it takes to be funded as an Exponential random variable. The story for the Exponential, as taken from [the notes for](https://bayesrule.files.wordpress.com/2014/07/probability_cheatsheet_140718.pdf) Joe Blitzstein's [Intro to Probability course](http://www.stat110.net), is:

> You are in a grassy field at night, looking at shooting stars. The time in between each shooting star can be modeled as $Exponential(\beta)$, where $\beta$ is the expected amount of time until a shooting star passes.

We can think of our data, the number of hours to fund a profile, as coming out of a random number generator. This number generator is given the expected number of hours for a profile to be funded, and it gives us sample numbers. In this sense, we view the data as a random variable. (In the classical context, the data is not random. It either is a certain figure, or it isn't.)

##Assumption violations of the Exponential distribution

In the example, we are measuring the time between each shooting star. But when we measure time-to-fund, it's not that one profile is posted onto the website, we measure how long it takes to be funded, then after it's funded, another profile is posted, and we measure the time for that to be funded. Instead, there are multiple profiles posted to the site, and we are starting our timer from when the profile is posted to when it is funded. In the lingo, we are not strictly looking at inter-arrival times.

An important property of the Exponential distribution is that it is memoryless. The time it took for one shooting star doesn't affect the time until the next shooting star. But for us, a donor might fully fund multiple profiles in a session, causing the latter profiles' time-to-fund to be affected by its predecessors'. This violation of the memoryless property stems from the fact that we're not strictly measuring interarrival times.

Another assumption we violate is that the times-to-fund are identically distributed. To us sitting in a grassy field, all the shooting stars are the same. A certain kind of shooting star does not occur more frequently than another. But our profiles can more easily be categorized into various groups. For example, profiles that request more money take longer to fund.

Even though our data violates the assumptions of the Exponential distribution, and more broadly, the Poisson process, it is close enough that the results we get are applicable to our problem.

![scatter](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/1-scatter.png "")

The correlation between the number of hours to fund a profile, and its requested funding amount, is 30%--not very strong.

If the correlation were 100%, for every extra $100 a profile requests, its time-to-fund would be an extra 0.7 hours.

##Fitting the exponential distribution to our data

![hist](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/2-histogram.png "")

#Using conjugate priors, Gamma distribution

If our data fits an Exponential distribution, what distribution should we choose to state our prior belief of the number of hours to fund a male profile and a female profile? If we use a conjugate prior, then our final result will be analytically tractable, and we will be able to come to a mathematically clean solution.

The conjugate prior here is the $Gamma(\alpha,\beta)$ distribution, where $\beta$ is the same as in the $Exponential(\beta)$ distribution.

The inutitive story for the Gamma distribution is:

> We are again looking at shooting stars. We want to wait to see $\alpha$ stars before we go home. The amount of time between each shooting star can be modeled as $Exponential(\beta)$, where $\beta$ is the expected amount of time until a shooting star passes. The probability that all $\alpha$ shooting stars will occur by $X$ total time is modeled by $Gamma(\alpha, \beta)$.

Let's model the time to fund a single profile via the $Gamma(\alpha, \beta)$ distribution, where $\alpha=1$. If we want to see how long it takes for one profile to be funded, that is the same story as for the $Exponential(\beta)$ distribution. $Gamma(1, \beta) = Exponential(\beta)$.

![gamma_single](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/3-gamma_single.png "")

![expon](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/4-expon.png "")

Let's input our actual data. Below shows the probability that all 1108 profiles will be funded within $X$ hours.

![expon](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/5-gamma_all_data.png "")

As our number of profiles, or sample size, increases, the probability that all profiles will be funded within $X$ hours becomes a Normal distribution, as we expect via the Central Limit Theorem.

#Choosing our prior parameter values via Empirical Bayes

For both our prior belief for the number of hours to fund a male profile, and the number of hours to fund a female profile, we will say that we know about 1 profile, and we will set the expected time for that that profile to be funded to be the median (or mean?) of all the profiles. We will choose:

$$Gamma(\alpha = 1, \beta = 46.36)$$

The fact that we are looking at our post-experiment data to choose our 'prior' belief as to the number of hours to fund a profile is an example of the Empirical Bayes technique. Traditionally, we should not use post-experiment data to choose our priors.

![expon](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/6-posterior_example_1.png "")

#Our updated beliefs after we see the data, the posterior distribution

Our prior belief was that it would take 46 hours to fund a profile, for both males and females, having 'seen' 1 profile. The data we've collected shows us how long it took to fund each male profile, and each female profile. Now, we can combine our prior belief with the data to get the probability that it would take $X$ hours to fund all our 524 male profiles, and similarly for all our 584 female profiles. This probability distribution is called the posterior distribution.

Our posterior distribution here, because I used the $Gamma(\alpha, \beta)$ conjugate prior, is $Gamma(\alpha + 1, \frac{\beta + x}{2})$ for one $Exponential(\beta)$ experiment when $\alpha = 1$ and $n = 1$, or we started with one new profile that I've learned about and we've learned about one additional profile.

So, let's say that we have a new $Exponential(\beta)$ profile, and it took 75 hours to fund it.

My updated posterior distribution is $Gamma(1 + 1, \frac{46 + 75}{2})$.

![expon](https://github.com/aok1425/stats-tests/raw/master/bayesian_imgs/7-male_female_dists.png "")