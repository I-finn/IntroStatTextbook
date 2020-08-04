# Inference for numerical data {#inference-num}

\BeginKnitrBlock{uptohere}<div class="uptohere">The content in this chapter is currently just placeholder. We will remove this banner once the chapter content has been updated and ready for review.</div>\EndKnitrBlock{uptohere}

\BeginKnitrBlock{chapterintro}<div class="chapterintro">Focusing now on Statistical Inference for **numerical data**, again, we will revisit and expand upon the foundational aspects of hypothesis testing from Chapter \@ref(inference-foundations).

The important data structure for this chapter is a numeric response variable (that is, the outcome is quantitative).
The four data structures we detail are one numeric response variable, one numeric response variable which is a difference across a pair of observations, a numeric response variable broken down by a binary explanatory variable, and a numeric response variable broken down by an explanatory variable that has two or more levels.
When appropriate, each of the data structures will be analyzed using the three methods from Chapter \@ref(inference-foundations): randomization test, bootstrapping, and mathematical models.

As we build on the inferential ideas, we will visit new foundational concepts in statistical inference.  One key new idea rests in estimating how the sample mean (as opposed to the sample proportion) varies from sample to sample; the resulting value is referred to as the standard error of the mean.  We will also introduce a new important mathematical model, the $t$-distribution (as the foundation for the $t$-test).  </div>\EndKnitrBlock{chapterintro}








In this chapter, we focus on the sample mean (instead of, for example, the sample median or the range of the observations) because of the well-studied mathematical model which describes the behavior of the sample mean.
We will not cover mathematical models which describe other statistics, but the bootstrap and randomization techniques described below are immediately extendable to any function of the observed data.
The sample mean will be calculated in one group, two paired groups, two independent groups, and many groups settings.
The techniques described for each setting will vary slightly, but you will be well served to find the structural similarities across the different settings.


## One mean {#one-mean}

Similar to how we can model the behavior of the
sample proportion $\hat{p}$ using a normal distribution,
the sample mean $\bar{x}$ can also be modeled using
a normal distribution when certain conditions are met.
\index{point estimate!single mean}
However, we'll soon learn that a new distribution,
called the $t$-distribution,
tends to be more useful when working with the sample mean.
We'll first learn about this new distribution,
then we'll use it to construct confidence intervals
and conduct hypothesis tests for the mean.


### Bootstrap confidence interval for $\mu$

As an employer who subsidizes housing for your employees, you need to know the average month rental price for a three bedroom flat in Edinburgh.
In order to walk through the example more clearly, let's say that you are only able to randomly sample five Edinburgh flats (if this were a real example, you would surely be able to take a much larger sample size, possibly even being able to measure the entire population!).


#### Observed data {-}

Figure \@ref(fig:5flats) presents the details of the random sample of observations where the monthly rent of five flats has been recorded.

<div class="figure" style="text-align: center">
<img src="06/figures/5flats.png" alt="5 flats" width="75%" />
<p class="caption">(\#fig:5flats)5 flats</p>
</div>



The sample average monthly rent of £ 1648 is a first guess at the price of three bedroom flats.  However, as a student of statistics, you understand that one sample mean based on a sample of five observations will not necessarily equal the true population average rent for all three bedroom flats in Edinburgh.
Indeed, you can see that the observed rent prices vary with a standard deviation of 340.232, and surely the average monthly rent would be different if a different sample of size five had been taken from the population.
Fortunately, as it did in previous chapters for the sample proportion, bootstrapping will approximate the variability of the sample mean from sample to sample.


#### Variability of the statistic {-}

As with the inferential ideas covered in Chapter \@ref(inference-foundations), the inferential analysis methods in this chapter are grounded in quantifying how one dataset differs from another when they are both taken from the same population.
Again, it still doesn't make sense to to take repeated samples from the same population (same reasoning:  if you have the means to take more samples, a larger sample size will benefit you more than then exact same sample twice).
Just like with proportions, we are going to use the observed data to 

Most of the inferential procedures covered in this text are grounded in quantifying how one data set would differ from another when they are both taken from the same population.
It doesn't make sense to take repeated samples from the same population because if you have the means to take more samples, a larger sample size will benefit you more than the exact same sample twice.
Instead, we measure how the samples behave under an estimate of the population.  Figure \@ref(fig:bootquant1) shows how the unknown original population can be estimated by using the sample to approximate the distribution of 

\BeginKnitrBlock{todo}<div class="todo">need to fill in the example here</div>\EndKnitrBlock{todo}

<div class="figure" style="text-align: center">
<img src="06/figures/bootquant1.png" alt="first figure with the ? pop, then sample, then estimate of the pop." width="75%" />
<p class="caption">(\#fig:bootquant1)first figure with the ? pop, then sample, then estimate of the pop.</p>
</div>


By taking repeated samples from the estimated population, the variability from sample to sample can be observed.  In Figure \@ref(fig:boot2) the repeated bootstrap samples are obviously different both from each other and from the original population.
Recall that the bootstrap samples were taken from the same (estimated) population, and so the differences are due entirely to natural variability in the sampling procedure.

<div class="figure" style="text-align: center">
<img src="06/figures/bootquant2.png" alt="next fig, has the bootstrap samples" width="75%" />
<p class="caption">(\#fig:bootquant2)next fig, has the bootstrap samples</p>
</div>

By summarizing each of the bootstrap samples (here, using the sample mean), we see, directly, the variability of the sample mean, $\bar{x}$, from sample to sample.
The distribution of $\hat{x}_{bs}$ for the Edinburgh flats is shown in Figure \@ref(fig:bootquant3).

\BeginKnitrBlock{todo}<div class="todo">after the plot is made, describe the actual BS samples</div>\EndKnitrBlock{todo}


<div class="figure" style="text-align: center">
<img src="06/figures/bootquant3.png" alt="WITH ADDED HISTOGRAM... boot samples, arrow, histogram of all of them" width="75%" />
<p class="caption">(\#fig:bootquant3)WITH ADDED HISTOGRAM... boot samples, arrow, histogram of all of them</p>
</div>




\BeginKnitrBlock{todo}<div class="todo">add the sampling with replacement part (????)</div>\EndKnitrBlock{todo}


Figure \@ref(fig:flatsbsmean) summarizes one thousand bootstrap samples in a histogram of the bootstrap sample means.
The bootstrapped average rent prices vary from £ 1250 to £ 1995 (with a small observed sample of size 5, a bootstrap resample can sometimes, although rarely, include only repeated measurements of the same observation).
The bootstrap confidence interval is found by locating the middle 90% (for a 90% confidence interval) or a 95% (for a 95% confidence interval) of the bootstrapped statistics.

\BeginKnitrBlock{example}<div class="example">Using Figure \@ref(fig:flatsbsmean), find the 90% and 95% confidence intervals for the true mean monthly rental price of a three bedroom flat in Edinburgh.

---
A 90% confidence interval is given by £ 1429 to £ 1876.  The conclusion is that we are 90% confident that the true average rental price for three bedroom flats in Edinburgh lies somewhere between £ 1429 and £ 1876.


A 95% confidence interval is given by £ 1389.75 to £ 1916.  The conclusion is that we are 90% confident that the true average rental price for three bedroom flats in Edinburgh lies somewhere between £ 1389.75 and £ 1916.</div>\EndKnitrBlock{example}



<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/flatsbsmean-1.png" alt="The original Edinburgh data is bootstrapped 1,000 times. The histogram provides a sense for the variability of the average rent values from sample to sample." width="70%" />
<p class="caption">(\#fig:flatsbsmean)The original Edinburgh data is bootstrapped 1,000 times. The histogram provides a sense for the variability of the average rent values from sample to sample.</p>
</div>

#### Bootstrap SE confidence interval {-}

Another method for creating bootstrap intervals is built on first calculating the variability of the bootstrap statistics (here, the bootstrap means).  If the bootstrap distribution is relatively symmetric and bell-shaped, then the bootstrap 95% confidence interval can be constructed with the formula familiar from the mathematical models in previous chapters:

$$\mbox{point estimate} \pm 2 \cdot SE_{BS}$$
The number 2 is an approximation connected to the "95%" part of the confidence interval (remember the 68-95-99.7 rule) which will be made more detailed in Section \@ref(one-mean-math).

\BeginKnitrBlock{example}<div class="example">Explain how the SE of the bootstrapped means is calculated and what it is measuring.

---

The SE of the bootstrapped means measures how variable the means are from resample to resample.  The bootstrap SE is a good approximation to the SE of means as if we had taken repeated samples from the original population (which we agreed isn't something we would do because of wasted resources).

Logistically, we can find the standard deviation of the bootstrapped means using the same calculations from Chapter \@ref(eda).  That is, the bootstrapped means are the individual observations about which we measure the variability.</div>\EndKnitrBlock{example}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">It turns out that the standard deviation of the bootstrapped means from Figure \@ref(fig:flatsbsmean) is £ 136.9.  [Note: in R the calculation was done using the function `sd()`.]  The average of the observed prices, the best guess point estimate for $\mu$, is  £ 1648.

Find and interpret the confidence interval for $\mu$ (the true average rental price of flats in Edinbugh) using the Bootstrap SE inverval formula.^[Using the formula for the boostrap SE interval, we find the 95% confidence interval for $\mu$ is: 
                                                                                                                                                      $1648 \pm 2 \cdot 136.9 \rightarrow$ (£ 1374.2, £ 1921.8)
                                                                                                                                                      
We are 95% confident that the true average rent price for a three bedroom flat in Edinburgh is somewhere between £ 1374.2 and £ 1921.8.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{example}<div class="example">Compare and contrast the two different 95% confidence intervals for $\mu$ created by finding the percentiles of the bootstrapped means and created by finding the SE of the bootstrapped means.  Do you think the intervals *should* be identical?
  
---
  
* Percentile interval: (£ 1389.75, £ 1916)
* SE interval: (£ 1374.2, £ 1921.8)

The intervals were created using different methods, so it is not surprising that they are not identical.  However, we are pleased to see that the two methods provide very similar interval approximations. 

The technical details surrounding which data structures are best for percentile intervals and which are best for SE intervals is beyond the scope of this text.  However, the larger the samples are, the better the interval estimates will be.</div>\EndKnitrBlock{example}

#### Bootstrap confidence interval for $\sigma$ {-}

Suppose that the research question at hand seeks to understand how variable the rental price of the flats are in Edinburgh.
That is, your interest is no longer in the average rental price of the flats but in the *standard deviation* of the rental prices of all three bedroom flats in Edinburgh, $\sigma$.
You may have already realized that the sample standard deviation, $s$, will work as a good **point estimate** for the parameter of interest: the population standard deviation, $\sigma$.
The point estimate of the five observations is calculated to be $s =$ £  340.23.
While $s =$ £  340.23 might be a good guess for $\sigma$, we prefer to have an interval 
Although there is a mathematical model which describes how $s$ varies from sample to sample, the mathematical model will not be presented in this text.
But even without the mathematical model, bootstrapping can be used to find a confidence interval for the parameter $\sigma$.



\BeginKnitrBlock{example}<div class="example">Describe the bootstrap distribution for the standard deviation shown in Figure \@ref(fig:flatsbssd).

---
  
The distribution is skewed left and centered near £ 340.23, which is the point estimate from the original data. Most observations in this distribution lie between £ 0 and £ 408.1.</div>\EndKnitrBlock{example}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Using Figure \@ref(fig:flatsbssd), find *and interpret* a 90% confidence interval for the population standard deviation for three bedroom flat prices in Edinburgh.^[By looking at the percentile values in Figure \@ref(fig:flatsbssd), the middle 90% of the bootstrap standard deviations are given by the 5 percentile (£ 153.9) and 95 percentile (£ 385.6).  That is, we are 90% confident that the true standard deviation of rent prices is between £ 153.9 and £ 385.6.]</div>\EndKnitrBlock{guidedpractice}

<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/flatsbssd-1.png" alt="The original Edinburgh data is bootstrapped 1,000 times. The histogram provides a sense for the variability of the standard deviation of the rent values from sample to sample." width="70%" />
<p class="caption">(\#fig:flatsbssd)The original Edinburgh data is bootstrapped 1,000 times. The histogram provides a sense for the variability of the standard deviation of the rent values from sample to sample.</p>
</div>


#### Bootstrapping is not a solution to small sample sizes! {-}

The example presented above is done for a sample with only five observations. 
As with analysis techniques that build on mathematical models, bootstrapping works best when a large random sample has been taken from the population.
Bootstrapping is a method for capturing the variability of a statistic when the mathematical model is unknown (it is not a method for navigating small samples).
As you might guess, the larger the random sample, the more accurately that sample will represent the population of interest.

### Mathematical model {#one-mean-math}


As with the sample proportion, the variability of the sample mean is well described by the mathematical theory given by the Central Limit Theorem.  However, because of missing information about the inherent variability in the population, a $t$-distribution is used in place of the standard normal when performing hypothesis test or confidence interval analyses.

#### A mathematical distribution of $\bar{x}$

The sample mean tends to follow
a normal distribution centered at the population mean, $\mu$,
when certain conditions are met.
Additionally, we can compute a standard error for the sample
mean using the population standard deviation $\sigma$
and the sample size $n$.

\BeginKnitrBlock{onebox}<div class="onebox">**Central Limit Theorem for the sample mean**  
  When we collect a sufficiently large sample of
  $n$ independent observations from a population with
  mean $\mu$ and standard deviation $\sigma$,
  the sampling distribution of $\bar{x}$ will be nearly
  normal with
  \begin{align*}
  &\text{Mean}=\mu
  &&\text{Standard Error }(SE) = \frac{\sigma}{\sqrt{n}}
  \end{align*}</div>\EndKnitrBlock{onebox}


Before diving into confidence intervals and hypothesis
tests using $\bar{x}$, we first need to cover two topics:

* When we modeled $\hat{p}$ using the normal distribution,
    certain conditions had to be satisfied.
    The conditions for working with $\bar{x}$
    are a little more complex, and below, we will discuss
    how to check conditions for inference using a mathematical model.
* The standard error is dependent on the population
    standard deviation, $\sigma$.
    However, we rarely know $\sigma$, and instead
    we must estimate it.
    Because this estimation is itself imperfect,
    we use a new distribution called the
    **$t$-distribution**\index{t-distribution@$t$-distribution}
    to fix this problem, which we discuss in




#### Evaluating the two conditions required for modeling $\bar{x}$ {-}


Two conditions are required to apply the
Central Limit Theorem\index{Central Limit Theorem}
for a sample mean $\bar{x}$:  
* **Independence.** The sample observations must be independent,
    The most common way to satisfy this condition is
    when the sample is a simple random sample from the
    population.
    If the data come from a random process,
    analogous to rolling a die,
    this would also satisfy the independence condition.  
* **Normality.** When a sample is small,
    we also require that the sample observations
    come from a normally distributed population.
    We can relax this condition more and more
    for larger and larger sample sizes.
    This condition is obviously vague,
    making it difficult to evaluate,
    so next we introduce a couple rules of thumb
    to make checking this condition easier.





\BeginKnitrBlock{onebox}<div class="onebox">**General rule: how to perform the normality check**
  
  There is no perfect way to check the normality condition,
  so instead we use two general rules: 

* $\mathbf{n < 30}$: If the sample size $n$ is less than 30
      and there are no clear outliers in the data,
      then we typically assume the data come from
      a nearly normal distribution to satisfy the
      condition.  
* $\mathbf{n \geq 30}$: If the sample size $n$ is at least 30
      and there are no *particularly extreme* outliers,
      then we typically assume the sampling distribution
      of $\bar{x}$ is nearly normal, even if the underlying
      distribution of individual observations is not.</div>\EndKnitrBlock{onebox}

In this first course in statistics, you aren't expected
to develop perfect judgement on the normality condition.
However, you are expected to be able to handle
clear cut cases based on the rules of thumb.^[More
  nuanced guidelines would consider further relaxing
  the *particularly extreme outlier* check when the
  sample size is very large.
  However, we'll leave further discussion here to a future course.]

\BeginKnitrBlock{example}<div class="example">Consider the following two plots
    that come from simple random samples from
    different populations.
    Their sample sizes are $n_1 = 15$ and $n_2 = 50$.

Are the independence and normality conditions met
    in each case?

---
      
Each samples is from a simple random sample of its
  respective population, so the independence condition
  is satisfied.
  Let's next check the normality condition for
  each using the rule of thumb.
  
  The first sample has fewer than 30 observations,
  so we are watching for any clear outliers.
  None are present; while there is a small gap in the
  histogram on the right, this gap is small and
  20% of the observations in this small sample
  are represented in that far right bar of the histogram,
  so we can hardly call these clear outliers.
  With no clear outliers, the normality condition
  is reasonably met.

  The second sample has a sample size greater than 30 and
  includes an outlier that appears to be roughly 5 times
  further from the center of the distribution than the
  next furthest observation.
  This is an example of a particularly extreme outlier,
  so the normality condition would not be satisfied.</div>\EndKnitrBlock{example}

<img src="06-inference-num_files/figure-html/outliersandsscondition-1.png" width="70%" style="display: block; margin: auto;" /><img src="06-inference-num_files/figure-html/outliersandsscondition-2.png" width="70%" style="display: block; margin: auto;" />



In practice, it's typical to also do a mental check to evaluate
whether we have reason to believe the underlying population
would have moderate skew (if $n < 30$)
or have particularly extreme outliers ($n \geq 30$)
beyond what we observe in the data.
For example, consider the number of followers
for each individual account on Twitter,
and then imagine this distribution.
The large majority of accounts have built up
a couple thousand followers or fewer,
while a relatively tiny fraction have amassed
tens of millions of followers,
meaning the distribution is extremely skewed.
When we know the data come from such an extremely
skewed distribution,
it takes some effort to understand what sample
size is large enough for the normality condition
to be satisfied.



\index{Central Limit Theorem!normal data|)}



#### Introducing the $t$-distribution {-}

\index{t-distribution@$t$-distribution|(}
\index{distribution!t@$t$|(}

In practice, we cannot directly calculate the standard error
for $\bar{x}$ since we do not know the population standard
deviation, $\sigma$.
We encountered a similar issue when computing the standard
error for a sample proportion, which relied on the population
proportion, $p$.
Our solution in the proportion context was to use sample
value in place
of the population value when computing the standard error.
We'll employ a similar strategy for computing the standard
error of $\bar{x}$, using the sample
standard deviation $s$ in place of $\sigma$:
\begin{align*}
SE = \frac{\sigma}{\sqrt{n}} \approx \frac{s}{\sqrt{n}}
\end{align*}
This strategy tends to work well when we have
a lot of data and can estimate $\sigma$ using $s$ accurately.
However, the estimate is less precise with smaller samples,
and this leads to problems when using the normal
distribution to model $\bar{x}$.



We'll find it useful to use a new distribution for
inference calculations called the **$t$-distribution**.
A $t$-distribution, shown as a solid line in
Figure \@ref(fig:tDistCompareToNormalDist), has a bell shape.
However, its tails are thicker than the normal distribution's,
meaning observations are more likely to fall beyond two
standard deviations from the mean than under the normal
distribution. 

The extra thick tails of the $t$-distribution are exactly
the correction needed to resolve the problem of using $s$
in place of $\sigma$ in the $SE$ calculation.


<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/tDistCompareToNormalDist-1.png" alt="Comparison of a $t$-distribution and a normal distribution." width="70%" />
<p class="caption">(\#fig:tDistCompareToNormalDist)Comparison of a $t$-distribution and a normal distribution.</p>
</div>


The $t$-distribution is always centered at zero and
has a single parameter: degrees of freedom.
The **degrees of freedom** \termsub{degrees of freedom ($\pmb{df}$)}
    {degrees of freedom ($df$)!$t$-distribution}
describes the precise form of the bell-shaped $t$-distribution.
Several $t$-distributions are shown in
Figure \@ref(fig:tDistConvergeToNormalDist)
in comparison to the normal distribution.

In general, we'll use a $t$-distribution
with $df = n - 1$ to model the sample mean
when the sample size is $n$.
That is, when we have more observations,
the degrees of freedom will be larger and
the $t$-distribution will look more like the
standard normal distribution;
when the degrees of freedom is about 30 or more,
the $t$-distribution is nearly indistinguishable
from the normal distribution.




<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/tDistConvergeToNormalDist-1.png" alt="The larger the degrees of freedom, the more closely the $t$-distribution resembles the standard normal distribution." width="70%" />
<p class="caption">(\#fig:tDistConvergeToNormalDist)The larger the degrees of freedom, the more closely the $t$-distribution resembles the standard normal distribution.</p>
</div>



\BeginKnitrBlock{onebox}<div class="onebox">**Degrees of freedom: df**
  
  The degrees of freedom describes the shape of the
  $t$-distribution.
  The larger the degrees of freedom, the more closely
  the distribution approximates the normal model. 
  
  When modeling $\bar{x}$ using the $t$-distribution,
  use $df = n - 1$.</div>\EndKnitrBlock{onebox}


The $t$-distribution allows us greater flexibility than
the normal distribution when analyzing numerical data.
In practice, it's common to use statistical software,
such as R, Python, or SAS for these analyses.
In R, the function used for calculating probabilities under a $t$-distribution is `pt()` (which should seem similar to previous R functions, `pnorm()` and `pchisq()`).
Don't forget that with the $t$-distribution, the degrees of freedom must always be specified!

<!--
Alternatively, a graphing calculator or a
\termsub{$\pmb{t}$-table}{t-table@$t$-table} may be used;
the $t$-table is similar to the normal distribution table,
and it may be found in Appendix \ref{tDistributionTable},
which includes usage instructions and examples
for those who wish to use this option.
-->
No matter the approach you choose, apply your method
using the examples below to confirm your working
understanding of the $t$-distribution.

\BeginKnitrBlock{example}<div class="example">What proportion of the $t$-distribution
    with 18 degrees of freedom falls below -2.10?
      
---
      
Just like a normal probability problem, we first draw
  the picture in Figure \@ref(fig:tDistDF18LeftTail2Point10)
  and shade the area below -2.10.


  Using statistical software, we can obtain a precise
  value: 0.0250.</div>\EndKnitrBlock{example}



```r
# using pt() to find probability under the $t$-distribution
pt(-2.10, df = 18)
#> [1] 0.025
```

<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/tDistDF18LeftTail2Point10-1.png" alt="The $t$-distribution with 18 degrees of freedom. The area below -2.10 has been shaded." width="70%" />
<p class="caption">(\#fig:tDistDF18LeftTail2Point10)The $t$-distribution with 18 degrees of freedom. The area below -2.10 has been shaded.</p>
</div>



\BeginKnitrBlock{example}<div class="example">A $t$-distribution with 20 degrees of freedom
    is shown in the top panel of
    Figure \@ref(fig:tDistDF20RightTail1Point65).
    Estimate the proportion of the distribution falling
    above 1.65.
    
---

With a normal distribution, this would correspond to
  about 0.05, so we should expect the $t$-distribution
  to give us a value in this neighborhood.
  Using statistical software: 0.0573.</div>\EndKnitrBlock{example}



<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/tDistDF20RightTail1Point65-1.png" alt="Top: The $t$-distribution with 20 degrees of freedom, with the area above 1.65 shaded. Bottom: The $t$-distribution with 2 degrees of freedom, with the area further than 3 units from 0 shaded." width="70%" /><img src="06-inference-num_files/figure-html/tDistDF20RightTail1Point65-2.png" alt="Top: The $t$-distribution with 20 degrees of freedom, with the area above 1.65 shaded. Bottom: The $t$-distribution with 2 degrees of freedom, with the area further than 3 units from 0 shaded." width="70%" />
<p class="caption">(\#fig:tDistDF20RightTail1Point65)Top: The $t$-distribution with 20 degrees of freedom, with the area above 1.65 shaded. Bottom: The $t$-distribution with 2 degrees of freedom, with the area further than 3 units from 0 shaded.</p>
</div>

\BeginKnitrBlock{example}<div class="example">A $t$-distribution with 2 degrees of freedom
    is shown in the bottom panel of
    Figure \@ref(fig:tDistDF20RightTail1Point65).
    Estimate the proportion of the distribution falling more
    than 3 units from the mean (above or below).
    
---

With so few degrees of freedom, the $t$-distribution will
  give a more notably different value than the normal
  distribution.
  Under a normal distribution, the area would be about
  0.003 using the 68-95-99.7 rule.
  For a $t$-distribution with $df = 2$, the area in both
  tails beyond 3 units totals 0.0955.
  This area is dramatically different than what
  we obtain from the normal distribution.</div>\EndKnitrBlock{example}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What proportion of the $t$-distribution with 19 degrees
of freedom falls above -1.79 units?
Use your preferred method for finding tail areas.^[We want to find the shaded area *above* -1.79 (we leave the picture to you).
  The lower tail area has an area of 0.0447,
  so the upper area would have an area of $1 - 0.0447 = 0.9553$.]</div>\EndKnitrBlock{guidedpractice}

\index{distribution!t@$t$|)}
\index{t-distribution@$t$-distribution|)}



#### One sample $t$-confidence intervals {-}

\index{data!dolphins and mercury|(}

Let's get our first taste of applying the $t$-distribution
in the context of an example about the mercury content
of dolphin muscle.
Elevated mercury concentrations are an important problem
for both dolphins
and other animals, like humans, who occasionally eat them.


<div class="figure" style="text-align: center">
<img src="06/figures/rissosDolphin.jpg" alt="A Risso's dolphin. Photo by Mike Baird, www.bairdphotos.com" width="75%" />
<p class="caption">(\#fig:rissosDolphin)A Risso's dolphin. Photo by Mike Baird, www.bairdphotos.com</p>
</div>

#### Observed data {-}

We will identify a confidence interval for the average mercury content in dolphin muscle using a sample of 19 Risso's dolphins from the Taiji area in Japan. The data are summarized in Table \@ref(tab:summaryStatsOfHgInMuscleOfRissosDolphins). The minimum and maximum observed values can be used to evaluate whether or not there are clear outliers.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:summaryStatsOfHgInMuscleOfRissosDolphins)Summary of mercury content in the muscle of 19 Risso's dolphins from the Taiji area. Measurements are in micrograms of mercury per wet gram
    of muscle ($\mu$g/wet g).</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> $n$ </th>
   <th style="text-align:right;"> $\bar{x}$ </th>
   <th style="text-align:right;"> s </th>
   <th style="text-align:right;"> minimum </th>
   <th style="text-align:right;"> maximum </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 19 </td>
   <td style="text-align:right;"> 4.4 </td>
   <td style="text-align:right;"> 2.3 </td>
   <td style="text-align:right;"> 1.7 </td>
   <td style="text-align:right;"> 9.2 </td>
  </tr>
</tbody>
</table>

\BeginKnitrBlock{example}<div class="example">Are the independence and
    normality conditions satisfied for this data set?
      
---
      
The observations are a simple random sample,
  therefore independence is reasonable.
  The summary statistics in
  Table \@ref(tab:summaryStatsOfHgInMuscleOfRissosDolphins)
  do not suggest any clear outliers, with
  all observations are within 2.5 standard deviations
  of the mean.
  Based on this evidence, the normality condition
  seems reasonable.</div>\EndKnitrBlock{example}

In the normal model, we used $z^{\star}$ and the standard error to determine the width of a confidence interval. We revise the confidence interval formula slightly when using the $t$-distribution:
\begin{align*}
&\text{point estimate} \ \pm\  t^{\star}_{df} \times SE
&&\to
&&\bar{x} \ \pm\  t^{\star}_{df} \times \frac{s}{\sqrt{n}}
\end{align*}

\BeginKnitrBlock{example}<div class="example">Using the summary statistics in
    Table \@ref(tab:summaryStatsOfHgInMuscleOfRissosDolphins),
    compute the standard error for the average
    mercury content in the $n = 19$ dolphins.
    
---
      
We plug in $s$ and $n$ into the formula:
  $SE
    = s / \sqrt{n}
    = 2.3 / \sqrt{19}
    = 0.528$.</div>\EndKnitrBlock{example}

The value $t^{\star}_{df}$ is a cutoff we obtain based on the
confidence level and the $t$-distribution with $df$ degrees
of freedom.
That cutoff is found in the same way as with a normal
distribution: we find $t^{\star}_{df}$ such that
the fraction of the $t$-distribution with $df$ degrees
of freedom within a distance $t^{\star}_{df}$
of 0 matches the confidence level of interest.

\BeginKnitrBlock{example}<div class="example">
When $n = 19$, what is the appropriate
    degrees of freedom?
    Find $t^{\star}_{df}$ for this degrees of freedom
    and the confidence level of 95%
    
---
  
The degrees of freedom is easy to calculate:
  $df = n - 1 = 18$.
  
  Using statistical software, we find the cutoff where
  the upper tail is equal to 2.5%:
  $t^{\star}_{18} = 2.10$.
  The area below -2.10 will also be equal to 2.5%.
  That is, 95% of the $t$-distribution with $df = 18$
  lies within 2.10 units of 0.</div>\EndKnitrBlock{example}

\BeginKnitrBlock{onebox}<div class="onebox">**Degrees of freedom for a single sample.**
  
If the sample has $n$ observations and we are examining a single mean, then we use the $t$-distribution with $df=n-1$ degrees of freedom.</div>\EndKnitrBlock{onebox}

%In our current example, we should use the $t$-distribution
%with $df=19-1=18$ degrees of freedom.
%We can generally identify $t_{18}^{\star}$
%using statistical software.
%Alternatively, we could use the $t$-table in
%Appendix \ref{tDistributionTable}.
%Generally the value of $t^{\star}_{df}$ is slightly larger
%than what we would get under the normal model with $z^{\star}$.

\BeginKnitrBlock{example}<div class="example">Compute and interpret the 95% confidence interval
    for the average mercury content in Risso's dolphins.
    
---

We can construct the confidence interval as
  \begin{align*}
  \bar{x} \ \pm\  t^{\star}_{18} \times SE
    \quad \to \quad 4.4 \ \pm\  2.10 \times 0.528
    \quad \to \quad (3.29, 5.51)
  \end{align*}
  We are 95% confident the average mercury content of muscles
  in Risso's dolphins is between 3.29 and 5.51 $\mu$g/wet gram,
  which is considered extremely high.</div>\EndKnitrBlock{example}

\index{data!dolphins and mercury|)}

\BeginKnitrBlock{onebox}<div class="onebox">**Finding a $t$-confidence interval for the mean, $\mu$.**
  
  Based on a sample of $n$ independent and nearly normal
  observations, a confidence interval for the population
  mean is
  \begin{align*}
  &\text{point estimate} \ \pm\  t^{\star}_{df} \times SE
  &&\to
  &&\bar{x} \ \pm\  t^{\star}_{df} \times \frac{s}{\sqrt{n}}
  \end{align*}
  where $\bar{x}$ is the sample mean, $t^{\star}_{df}$
  corresponds to the confidence level and degrees of freedom
  $df$, and $SE$ is the standard error as estimated by
  the sample.</div>\EndKnitrBlock{onebox}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">The FDA's webpage provides some data on mercury content of fish.
Based on a sample of 15 croaker white fish (Pacific),
a sample mean and standard deviation were computed as 0.287
and 0.069 ppm (parts per million), respectively.
The 15 observations ranged from 0.18 to 0.41 ppm.
We will assume these observations are independent.
Based on the summary statistics of the data,
do you have any objections to the normality condition
of the individual observations?^[The sample size is under 30,
  so we check for obvious outliers:
  since all observations are within 2 standard deviations
  of the mean, there are no such clear outliers.]</div>\EndKnitrBlock{guidedpractice}


\index{data!white fish and mercury|(}


\BeginKnitrBlock{example}<div class="example">Estimate the standard error of
    $\bar{x} = 0.287$ ppm using the data summaries in the previous Guided Practice.    If we are to use the $t$-distribution to create a
    90% confidence interval for the actual mean of the
    mercury content, identify the degrees of freedom
    and $t^{\star}_{df}$.
  
---
      
The standard error: $SE = \frac{0.069}{\sqrt{15}} = 0.0178$.

  Degrees of freedom: $df = n - 1 = 14$.

  Since the goal is a 90% confidence interval,
  we choose $t_{14}^{\star}$ so that the two-tail area
  is 0.1:
  $t^{\star}_{14} = 1.76$.</div>\EndKnitrBlock{example}

<!--
\begin{onebox}{Confidence interval for a single mean}
  Once you've determined a one-mean confidence interval
  would be helpful for an application,
  there are four steps to constructing the interval:
  \begin{description}
  \item[Prepare.]
      Identify $\bar{x}$, $s$, $n$, and determine what
      confidence level you wish to use.
  \item[Check.]
      Verify the conditions to ensure $\bar{x}$
      is nearly normal.
  \item[Calculate.]
      If the conditions hold, compute $SE$,
      find $t_{df}^{\star}$, and construct the interval.
  \item[Conclude.]
      Interpret the confidence interval in the context
      of the problem.
  \end{description}
\end{onebox}
-->


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Using the information and results of the previous Guided Practice and Example, compute a 90% confidence interval for the average mercury content of croaker white fish (Pacific).^[$\bar{x} \ \pm\ t^{\star}_{14} \times SE
      \ \to\  0.287 \ \pm\  1.76 \times 0.0178
      \ \to\ (0.256, 0.318)$.
  We are 90% confident that the average mercury content
  of croaker white fish (Pacific) is between 0.256 and 0.318 ppm.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">The 90% confidence interval from the previous
Guided Practice is 0.256 ppm to 0.318 ppm.
Can we say that 90% of croaker white fish (Pacific)
have mercury levels between 0.256 and 0.318 ppm?^[No, a confidence interval only provides a range
  of plausible values for a population parameter,
  in this case the population mean.
  It does not describe what we might observe
  for individual observations.]</div>\EndKnitrBlock{guidedpractice}

\index{data!white fish and mercury|)}

#### One sample $t$-tests {-}

Now that we've used the $t$-distribution for making a confidence
intervals for a mean, let's speed on through to
hypothesis tests for the mean.

<!--
\newcommand{\cherryblossomn}{100}
\newcommand{\cherryblossommean}{97.32}
\newcommand{\cherryblossomnull}{93.29}
\newcommand{\cherryblossomsd}{16.98}
\newcommand{\cherryblossomse}{1.70}
\newcommand{\cherryblossomz}{2.37}
-->

Is the typical US runner getting faster or slower over time? We consider this question in the context of the Cherry Blossom Race, which is a 10-mile race in Washington, DC each spring.

The average time for all runners who finished the Cherry Blossom Race in 2006 was 93.29 minutes (93 minutes and about 17 seconds). We want to determine using data from 100 participants in the 2017 Cherry Blossom Race whether runners in this race are getting faster or slower, versus the other possibility that there has been no change.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What are appropriate hypotheses for this context?^[$H_0$: The average 10-mile run time was the same for 2006 and 2017. $\mu = 93.29$ minutes. $H_A$: The average 10-mile run time for 2017 was \emph{different} than that of 2006. $\mu \neq 93.29$ minutes.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">The data come from a simple random sample of all participants,
so the observations are independent.
However, should we be worried about the normality condition?
See Figure \@ref(fig:run10SampTimeHistogram) for a histogram
of the differences and evaluate if we can move
forward.^[With a sample of 100,
  we should only be concerned if there is are particularly
  extreme outliers.
  The histogram of the data doesn't show any outliers of concern
  (and arguably, no outliers at all).]</div>\EndKnitrBlock{guidedpractice}



<div class="figure" style="text-align: center">
<img src="06-inference-num_files/figure-html/run10SampTimeHistogram-1.png" alt="A histogram of `time` for the sample Cherry Blossom Race data." width="70%" />
<p class="caption">(\#fig:run10SampTimeHistogram)A histogram of `time` for the sample Cherry Blossom Race data.</p>
</div>

When completing a hypothesis test for the one-sample mean,
the process is nearly identical to completing a hypothesis
test for a single proportion.
First, we find the Z score using the observed value,
null value, and standard error;
however, we call it a **T score** since we use
a $t$-distribution for calculating the tail area.
Then we finding the p-value using the same ideas we used
previously: find the one-tail area under the sampling
distribution, and double it.



\BeginKnitrBlock{example}<div class="example">With both the independence
    and normality conditions satisfied,
    we can proceed with a hypothesis test using
    the $t$-distribution.
    The sample mean and sample standard deviation
    of the sample
    of 100 runners from the
    2017 Cherry Blossom Race
    are 97.32 and 16.98 minutes,
    respectively.
    Recall that the sample size is 100
    and the average run time in 2006 was 93.29 minutes.
    Find the test statistic and p-value.
    What is your conclusion?

---
  
To find the test statistic (T score),
  we first must determine the standard error:
  \begin{align*}
  SE
    = 16.98 / \sqrt{100}
    = 1.70
  \end{align*}
  Now we can compute the \emph{T score}
  using the sample mean (97.32),
  null value (98.29), and $SE$:
  \begin{align*}
  T
    = \frac{97.32 - 93.29}{1.70}
    = 2.37
  \end{align*}
  For $df = 100 - 1 = 99$,
  we can determine using statistical software
  (or a $t$-table, see below) that the one-tail area is 0.01,
  which we double to get the p-value: 0.02.

  Because the p-value is smaller than 0.05,
  we reject the null hypothesis.
  That is, the data provide strong evidence that the average
  run time for the Cherry Blossom Run in 2017 is different
  than the 2006 average.
  Since the observed value is above the null value
  and we have rejected the null hypothesis, we would conclude
  that runners in the race were slower on average in 2017
  than in 2006.</div>\EndKnitrBlock{example}


```r
# using pt() to find the p-value
1 - pt(2.37, df = 99)
#> [1] 0.00986
```


\BeginKnitrBlock{onebox}<div class="onebox">**When using a $t$-distribution, we use a T score (same as Z score).**

To help us remember to use the $t$-distribution,
we use a $T$ to represent the test statistic,
and we often call this a **T score**.
The Z score and T score are computed in the exact same way
and are conceptually identical:
each represents how many standard errors the observed value
is from the null value.</div>\EndKnitrBlock{onebox}

<!--
\begin{onebox}{Hypothesis testing for a single mean}
  Once you've determined a one-mean hypothesis test is the
  correct procedure, there are four steps to completing the
  test:
  \begin{description}
  \item[Prepare.]
      Identify the parameter of interest,
      list out hypotheses,
      identify the significance level,
      and identify $\bar{x}$, $s$, and $n$.
  \item[Check.]
      Verify conditions to ensure $\bar{x}$ is nearly normal.
  \item[Calculate.]
      If the conditions hold, compute $SE$,
      compute the T score, and identify the p-value.
  \item[Conclude.]
      Evaluate the hypothesis test by comparing the p-value
      to $\alpha$, and provide a conclusion in the context
      of the problem.
  \end{description}
\end{onebox}
-->

<!--
\CalculatorVideos{confidence intervals and hypothesis tests for a single mean}


{\input{ch_inference_for_means/TeX/one-sample_means_with_the_t-distribution.tex}}
-->





## Paired difference {#paired-data}


### case study

### Randomization test for $H_0: \mu_d = 0$

for randomization, idea of coin flipping

##### Observed data {-}

##### Variability of the statistic {-}

##### Observed statistic vs. null statistics {-}

### Bootstrap confidence interval for $\mu_d$

for bootstrap and mathematical model there is not much to do here except go through a full example.  tie the ideas back to the one-sample problem.


##### Observed data {-}

##### Variability of the statistic {-}

### Mathematical model

for bootstrap and mathematical model there is not much to do here except go through a full example.  tie the ideas back to the one-sample problem.


##### Observed data {-}

##### Variability of the statistic {-}

##### Observed statistic vs. null statistics {-}



## Difference of two means


### case study

### Randomization test for $H_0: \mu_1 - \mu_2 = 0$

need to talk about the way to randomize is almost identical to chapter 5 & 6.  a new plot will probably help (but again, very similar to 5.7) 

##### Observed data {-}

##### Variability of the statistic {-}

##### Observed statistic vs. null statistics {-}

### Bootstrap confidence interval for $\mu_1 - \mu_2$

tie back to idea in chapter 6 for two proportion CI.  

##### Observed data {-}

##### Variability of the statistic {-}

### Mathematical model

t-test.  mention that there are lots of nuances outside the scope of this book.

##### Observed data {-}

##### Variability of the statistic {-}

##### Observed statistic vs. null statistics {-}


## More insights into the $t$-distribution

## Chapter 6 review {#chp6-review}


### Terms

We introduced the following terms in the chapter. 
If you're not sure what some of these terms mean, we recommend you go back in the text and review their definitions.
We are purposefully presenting them in alphabetical order, instead of in order of appearance, so they will be a little more challenging to locate. 
However you should be able to easily spot them as **bolded text**.




