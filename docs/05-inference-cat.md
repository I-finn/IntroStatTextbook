# Inference for categorical data {#inference-cat}

\BeginKnitrBlock{chapterintro}<div class="chapterintro">Statistical inference is primarily concerned with understanding and quantifying the _uncertainty_ of parameter estimates---that is, how variable is a sample statistic
from sample to sample?
While the equations and details change depending on the setting, the foundations for inference are the same throughout all of statistics. We will begin this chapter with a discussion of the foundations of inference, and introduce the two primary vehicles of inference: the **hypothesis test** and **confidence interval**.

Te rest of this chapter focuses statistical inference for categorical data. The two data structures we detail are one binary variable, summarized using a single proportion, and two binary variables, summarized using a difference (or ratio) of two proportions.</div>\EndKnitrBlock{chapterintro}



Throughout the book so far, you have worked with data in a variety of contexts.
You have learned how to summarize and visualize the data as well as how to model multiple variables at the same time.
Sometimes the data set at hand represents the entire research question.
But more often than not, the data have been collected to answer a research question about a larger group of which the data are a (hopefully) representative subset.

You may agree that there is almost always variability in data (one data set will not be identical to a second data set even if they are both collected from the same population using the same methods).
However, quantifying the variability in the data is neither obvious nor easy to do (**how** different is one data set from another?). 



\BeginKnitrBlock{example}<div class="example">Suppose your professor splits the students in class into two groups: students on the left and students on the right. If $\hat{p}_{_L}$ and $\hat{p}_{_R}$ represent the proportion of students who own an Apple product on the left and right, respectively, would you be surprised if $\hat{p}_{_L}$ did not *exactly* equal $\hat{p}_{_R}$?

---

While the proportions would probably be close to each other, it would be unusual for them to be exactly the same. We would probably observe a small difference due to *chance*.</div>\EndKnitrBlock{example}



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If we don't think the side of the room a person sits on in class is related to whether the person owns an Apple product, what assumption are we making about the relationship between these two variables?
(Reminder: for these Guided Practice questions, you can check your answer in the footnote.)^[We would be assuming that these two variables are **independent**\index{independent}.]</div>\EndKnitrBlock{guidedpractice}

Studying randomness of this form is a key focus of statistics. 
Throughout this chapter, and those that follow, we provide two different approaches for quantifying the variability inherent in data: simulation-based methods and theory-based methods (mathematical models).
Using the methods provided in this and future chapters, we will be able to draw conclusions beyond the data set at hand to research questions about larger populations.

## Foundations of inference {#inf-foundations}

Given results seen in a sample, the process of determining what we can _infer_ to the
population based on sample results is called **statistical inference**\index{statistical inference}. Statistical inferential methods enable us to understand and quantify the _uncertainty_ of our sample results. Statistical inference helps us answer two questions about the population:

1. How strong is the _evidence_ of an effect?
2. How _large_ is the effect?

The first question is answered through a **hypothesis test**\index{hypothesis test}, while the second is addressed with a **confidence interval**\index{confidence interval}.



### Motivating example: Martian alphabet {#Martian}

How well can humans distinguish one "Martian" letter from another? The Figure \@ref(fig:kiki-bumba)
displays two Martians---one named Kiki and another named Bumba. Which do you think
is Kiki and which do you think is Bumba?^[If you are a STAT 216 student, you will recognize this from our first week's in-class activity.]

<div class="figure" style="text-align: center">
<img src="05/images/bumBa-KiKi.png" alt="Two Martians named Bumba and Kiki. Do you think Bumba is on the left or the right?^[Bumba is the Martian on the left!]" width="75%" />
<p class="caption">(\#fig:kiki-bumba)Two Martians named Bumba and Kiki. Do you think Bumba is on the left or the right?^[Bumba is the Martian on the left!]</p>
</div>

This same image and question were presented to an introductory statistics class of
38 students. In that class, 34 students correctly identified Bumba as the Martian on the left. Assuming we can't read Martian, is this result surprising?

One of two possibilities occurred:

1. _We can't read Martian, and these results just occurred by chance._
2. _We can read Martian, and these results reflect this ability._

To decide between these two possibilities, we could calculate the probability
of observing such results in a randomly selected sample of 38 students, under
the assumption that students were just guessing. If this probability is _very low_,
we'd have reason to reject the first possibility in favor of the second.
We can calculate this probability using one of two methods:

* **Simulation-based method**: simulate lots of samples of 38 students under the assumption that
students are just guessing, then calculate the proportion of these
simulated samples where we saw 34 or more students guessing correctly, or
* **Theory-based method**: develop a mathematical model for the sample proportion in this
scenario and use the model to calculate the probability.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">How could you use a coin or cards to simulate the guesses of one sample of 38 students who cannot read Martian?^[A fair coin has a 50% chance of landing on heads, which is the chance a student would guess Bumba correctly if they were just guessing. Thus, toss a coin 38 times with heads representing "guess correctly"; then calculate the proportion of tosses that landed on heads. Another option would be to 10 black cards and 10 red cards, letting red represent "guess correctly". Shuffle the cards and draw one card, record if it is red or black, then replace the card and shuffle again. Do this 38 times and calculate the proportion of red cards observed.]</div>\EndKnitrBlock{guidedpractice}

For this situation---since "just guessing" means you have a 50% chance of guessing correctly---we could simulate a sample of 38 students' guesses by flipping a coin 38 times and counting the number of times it lands on heads. Using a computer to repeat this process 1,000 times, we create the dot plot in Figure \@ref(fig:MartianDotPlot).

<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/MartianDotPlot-1.png" alt="A dot plot of 1,000 sample proportions; each calculated by flipping a coin 38 times and calculating the proportion of times the coin landed on heads. None of the 1,000 simulations had sample proportion of at least 89%, which was the proportion observed in the study." width="70%" />
<p class="caption">(\#fig:MartianDotPlot)A dot plot of 1,000 sample proportions; each calculated by flipping a coin 38 times and calculating the proportion of times the coin landed on heads. None of the 1,000 simulations had sample proportion of at least 89%, which was the proportion observed in the study.</p>
</div>


None of our simulated samples produce 34 of 38 correct guesses! That is, if students were just guessing, it is nearly impossible to observe 34 or more correct guesses in a sample of 38 students. Given this low probability, the more plausible possibility is 2. _We can read Martian, and these results reflect this ability._ We've just completed our first hypothesis test!

Now, obviously no one can read Martian, so a more realistic possibility is that humans tend to choose Bumba on the left more often than the right---there is a greater than 50% chance of choosing Bumba on the left. Even though we may think we're guessing just by chance, we have a preference for Bumba on the left. It turns out that the explanation for this preference is called _synesthesia_, a tendency for humans to correlate sharp sounding noises (e.g., Kiki) with sharp looking images.^[To explore this further, watch this [TED Talk](https://www.ted.com/talks/vs_ramachandran_3_clues_to_understanding_your_brain) by neurologist Vilayanur Ramachandran (The synesthesia part begins at roughly 17:40 minutes).]

But wait---we're not done! We have evidence that humans tend to prefer Bumba on the left, but by how much? To answer this, we need a confidence interval---an interval of plausible values for the true probability humans will choose Bumba on the left. The width of this interval is determined by how variable sample proportions are from sample to sample. It turns out, there is a mathematical model for this variability that we will explore later in this chapter. For now, let's take the standard deviation from our simulated sample proportions as an estimate for this variability: 0.08. Since the simulated distribution of proportions is bell-shaped, we know about 95% of sample proportions should fall within two standard deviations of the true proportion, so we can add and subtract this **margin of error**\index{margin of error} to our sample proportion to calculate an approximate 95% confidence interval^[If you carry out the calculations, you'll note that the upper bound is actually $0.89 + 0.16 = 1.05$, but since a sample proportion cannot be greater than 1, we truncated the interval to 1.]:
\[
\frac{34}{38} \pm 2\times 0.08 = 0.89 \pm 0.16 = (0.73, 1)
\]
Thus, based on this data, we are 95% confident that the probability a human guesses Bumba on the left is somewhere between 73% and 100%.

### Variability in a statistic {#var-stat}

There are two approaches to modeling how a statistic may vary from sample to sample.
In the [Martian alphabet example](#Martian), we used a _simulation-based approach_ to model
this variability, using the standard deviation of the simulated distribution of sample proportions as a quantitative measure of this variability. We can also use a _theory-based approach_---one which makes use of
mathematical modeling.

\BeginKnitrBlock{onebox}<div class="onebox">**Central Limit Theorem.**\index{Central Limit Theorem}

For large sample sizes, the **sampling distribution** of a sample proportion (or sample mean) will appear to follow a bell-shaped curve called the *normal distribution*.</div>\EndKnitrBlock{onebox}



An example of a perfect normal distribution is shown in Figure \@ref(fig:simpleNormal). 
While the mean (center) and standard deviation (variability) may change for different scenarios, the general shape remains roughly intact.

<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/simpleNormal-1.png" alt="A normal curve." width="70%" />
<p class="caption">(\#fig:simpleNormal)A normal curve.</p>
</div>

Recall from Chapter \@ref(eda) that a _distribution_ of a variable is a description of the possible values it takes and how frequently each value occurs.  In a **sampling distribution**, our "variable" is a sample statistic, and the sampling distribution is a description of the possible values a sample statistic takes and how frequently each value occurs when looking across many many possible samples. It is quite amazing that something like a sample proportion, summarizing a categorical variable, will have a bell-shaped sampling distribution if we sample large enough samples!

Theory-based methods also give us mathematical expressions for the
standard deviation of a sampling distribution. For instance,
if the true population proportion is $\pi$, then the standard deviation
of the sampling distribution of sample proportions is
\[
SD(\hat{p}) = \sqrt{\frac{\pi(1-\pi)}{n}}.
\]
In the case of two samples, a sample of size $n_1$ taken from a population with proportion
$\pi_1$, and the other of size $n_2$ from a population with proportion $\pi_2$,
the standard deviation of the sampling distribution of the difference
in sample proportions is
\[
SD(\hat{p}_1 - \hat{p}_2) = \sqrt{\frac{\pi_1(1-\pi_1)}{n_1} + \frac{\pi_2(1-\pi_2)}{n}}
\]
We will revisit these formulas later in the chapter.

### Hypothesis tests {#HypothesisTesting}

In the [Martian alphabet example](#Martian), we utilized a **hypothesis test**\index{hypothesis test}, which is a formal technique for evaluating two competing possibilities. 
Each hypothesis test involves a **null hypothesis**\index{null hypothesis}, which represents either a skeptical perspective or a perspective of no difference or no effect, and an **alternative hypothesis**\index{alternative hypothesis}, which represents a new perspective such as the possibility that there has been a change or that there is a treatment effect in an experiment.  The alternative hypothesis is usually the reason the scientists set out to do the research in the first place.




\BeginKnitrBlock{onebox}<div class="onebox">**Null and alternative hypotheses.**

The **null hypothesis ($H_0$)** often represents either a skeptical perspective or a claim to be tested. The **alternative hypothesis ($H_A$)** represents an alternative claim under consideration and is often represented by a range of possible values for the parameter of interest. </div>\EndKnitrBlock{onebox}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">In the Martian alphabet example, which of the two competing possibilities was the null hypothesis? the alternative hypothesis?^[The first possibility (_We can't read Martian, and these results just occurred by chance._) was the null hypothesis; the second possibility (_We can read Martian, and these results reflect this ability._) was the alternative hypothesis.]</div>\EndKnitrBlock{guidedpractice}

The hypothesis testing framework is a very general tool, and we often use it without a second thought. 
If a person makes a somewhat unbelievable claim, we are initially skeptical. 
However, if there is sufficient evidence that supports the claim, we set aside our skepticism. 
The hallmarks of hypothesis testing are also found in the US court system. 

#### The US court system {-}


\BeginKnitrBlock{example}<div class="example">A US court considers two possible claims about a defendant: they are either innocent or guilty. If we set these claims up in a hypothesis framework, which would be the null hypothesis and which the alternative?

---
 
The jury considers whether the evidence is so convincing (strong) that there is no reasonable doubt regarding the person's guilt. 
That is, the skeptical perspective (null hypothesis) is that the person is innocent until evidence is presented that convinces the jury that the person is guilty (alternative hypothesis). Analogously, in a hypothesis test, we assume the null hypothesis until evidence is presented that convinces us the alternative hypothesis is true.</div>\EndKnitrBlock{example}


Jurors examine the evidence to see whether it convincingly shows a defendant is guilty. 
Notice that if a jury finds a defendant *not guilty*, this does not necessarily mean the jury is confident in the person's innocence. 
They are simply not convinced of the alternative that the person is guilty.

This is also the case with hypothesis testing: *even if we fail to reject the null hypothesis, we typically do not accept the null hypothesis as truth*. 
Failing to find strong evidence for the alternative hypothesis is not equivalent to providing evidence that the null hypothesis is true.

#### p-value and statistical significance {-}

In the [Martian alphabet example](#Martian), we performed a simulation-based hypothesis test of the hypotheses:

* $H_0$: The chance a human chooses Bumba on the left is 50%.

* $H_A$: Humans have a preference for choosing Bumba on the left.

The research question---can humans read Martian?---was framed in the context of these hypotheses.

The null hypothesis ($H_0$) was a perspective of no effect (no ability to read Martian).
The student data provided a point estimate of 89.5% ($34/38 \times 100$%) for the true probability of choosing Bumba on the left.
We determined that observing such a sample proportion from chance alone (assuming $H_0$) would be rare---it would only happen in less than 1 out of 1000 samples. When results
like these are inconsistent with $H_0$, we reject $H_0$ in favor of $H_A$. 
Here, we concluded there humans have a preference for choosing Bumba on the left.

The less than 1-in-1000 chance is what we call a **p-value**, which is a probability quantifying the strength of the evidence against the null hypothesis and in favor of the alternative. 

\BeginKnitrBlock{onebox}<div class="onebox">**p-value.**

The **p-value**\index{hypothesis testing!p-value|textbf} is the probability of observing data at least as favorable to the alternative hypothesis as our current data set, if the null hypothesis were true. 
We typically use a summary statistic of the data, such as a proportion or difference in proportions, to help compute the p-value and evaluate the hypotheses. 
This summary value that is used to compute the p-value is often called the **test statistic**\index{test statistic}.</div>\EndKnitrBlock{onebox}



\BeginKnitrBlock{protip}<div class="protip">When interpreting a p-value, remember that the definition of a p-value has three components. It is a (1) probability. What it is the probability of? It is the probability of (2) our observed sample statistic or one more extreme. Assuming what? It is the probability of our observed sample statistic or one more extreme, (3) assuming the null hypothesis is true:
  
(1) probability
(2) data^[Technically, the observed sample statistic or one more extreme in the direction of our alternative. But it is helpful to just remember this as "the data".]
(3) null hypothesis</div>\EndKnitrBlock{protip}

\BeginKnitrBlock{example}<div class="example">What was the test statistic in the [Martian alphabet example](#Martian)?
 
---
 
The test statistic in the the Martian alphabet example was the sample proportion, $\frac{34}{38} = 0.895$ (or 89.5%). This is also the **point estimate** of the true probability that humans would choose Bumba on the left.</div>\EndKnitrBlock{example}

When the p-value is small, i.e., less than a previously set threshold, we say the results are **statistically significant**. 
This means the data provide such strong evidence against $H_0$ that we reject the null hypothesis in favor of the alternative hypothesis. 
The threshold, called the **significance level**\index{hypothesis testing!significance level}\index{significance level} and often represented by $\alpha$ (the Greek letter *alpha*), is typically set to $\alpha = 0.05$, but can vary depending on the field or the application and the consequences of an incorrect decision. 
Using a significance level of $\alpha = 0.05$ in the Martian alphabet study, we can say that the data provided statistically significant evidence against the null hypothesis.

\BeginKnitrBlock{onebox}<div class="onebox">**Statistical significance.**

We say that the data provide **statistically significant**\index{hypothesis testing!statistically significant|textbf} evidence against the null hypothesis if the p-value is less than some reference value called the **significance level**\index{significance level}, denoted by $\alpha$.</div>\EndKnitrBlock{onebox}


\BeginKnitrBlock{onebox}<div class="onebox">**What's so special about 0.05?**

We often use a threshold of 0.05 to determine whether a result is statistically significant. 
But why 0.05? 
Maybe we should use a bigger number, or maybe a smaller number. 
If you're a little puzzled, that probably means you're reading with a critical eye---good job! 
The _OpenIntro_ authors have a video to help clarify *why 0.05*:
<center>
[https://www.openintro.org/book/stat/why05/](https://www.openintro.org/book/stat/why05/)
</center>
<br>
Sometimes it's also a good idea to deviate from the standard. 
We'll discuss when to choose a threshold different than 0.05 in Section ??.</div>\EndKnitrBlock{onebox}


### Confidence intervals {#ConfidenceIntervals}

A point estimate provides a single plausible value for a parameter. 
However, a point estimate is rarely perfect---usually there is some error in the estimate. 
In addition to supplying a point estimate of a parameter, a next logical step would be to provide a plausible *range of values* for the parameter.

A plausible range of values for the population parameter is called a **confidence interval**. 
Using only a single point estimate is like fishing in a murky lake with a spear, and using a confidence interval is like fishing with a net. 
We can throw a spear where we saw a fish, but we will probably miss. 
On the other hand, if we toss a net in that area, we have a good chance of catching the fish.

If we report a point estimate, we probably will not hit the exact population parameter. 
On the other hand, if we report a range of plausible values---a confidence interval---we have a good shot at capturing the parameter.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If we want to be very certain we capture the population parameter, should we use a wider interval or a smaller interval?^[If we want to be more certain we will capture the fish, we might use a wider net. Likewise, we use a wider confidence interval if we want to be more certain that we capture the parameter.]</div>\EndKnitrBlock{guidedpractice}

In Section ?? we will discuss different percentages for the confidence interval (e.g., 90% confidence interval or 99% confidence interval).  Section ?? also provides a longer discussion on what "95% confidence" actually means.


## One proportion {#single-prop}

A single proportion is used to summarize data when we measured a single categorical variable on each observational unit---the single variable is measured as either a success or failure (e.g., "surgical complication" vs. "no surgical complication")^[The terms "success" and "failure" may not actually represent outcomes we view as successful or not, but it is the typical generic way to referring to the possible outcomes of a binary variable. The "success" is whatever we count when calculating our sample proportion.]. 

\BeginKnitrBlock{onebox}<div class="onebox">**Notation**.

* $n$ = sample size (number of observational units in the data set)
* $\hat{p}$ = sample proportion (number of "successes" divided by the sample size)
* $\pi$ = population proportion^[When you see $\pi$ in this textbook, it will always symbolize a (typically unknown) population proportion, not the value 3.14....]</div>\EndKnitrBlock{onebox}

### Hypothesis test for $H_0: \pi = \pi_0$ {#one-prop-null-boot}

\index{data!medical consultant|(}

People providing an organ for donation sometimes seek the help of a special medical consultant. 
These consultants assist the patient in all aspects of the surgery, with the goal of reducing the possibility of complications during the medical procedure and recovery. 
Patients might choose a consultant based in part on the historical complication rate of the consultant's clients.

One consultant tried to attract patients by noting the average complication rate for liver donor surgeries in the US is about 10%, but her clients have had only 3 complications in the 62 liver donor surgeries she has facilitated. 
She claims this is strong evidence that her work meaningfully contributes to reducing complications (and therefore she should be hired!).

\BeginKnitrBlock{example}<div class="example">Using these data, is it possible to assess the consultant's claim that her work meaningfully contributes to reducing complications?

---

No. The claim is that there is a causal connection, but the data are observational, so we must be on the lookout for confounding variables. 
For example, maybe patients who can afford a medical consultant can afford better medical care, which can also lead to a lower complication rate.

While it is not possible to assess the causal claim, it is still possible to understand the consultant's true rate of complications.</div>\EndKnitrBlock{example}


We will let $\pi$ represent the true complication rate for liver donors working with this consultant. This "true" complication probability is called the **parameter** of interest^[Parameters were first introduced in Section \@ref(dotplots)].)
The sample proportion for the complication rate is 3 complications divided by the 62 surgeries the consultant has worked on: $\hat{p} = 3/62 = 0.048$. Since this value is estimated from sample data, it is called a **statistic**. The statistic $\hat{p}$ is our point estimate, or "best guess," for $\pi$.

\BeginKnitrBlock{onebox}<div class="onebox">**Parameters and statistics.**\index{parameter}

A **parameter** is the "true" value of interest. 
We typically estimate the parameter using a **statistic** or point estimate\index{point estimate} from a sample of data. 

For example, we estimate the probability $\pi$ of a complication for a client of the medical consultant by examining the past complications rates of her clients:

 
$$\hat{p} = 3 / 62 = 0.048\qquad\text{is used to estimate}\qquad \pi$$</div>\EndKnitrBlock{onebox}

\BeginKnitrBlock{protip}<div class="protip">Summary measures that summarize a sample of data, such as $\hat{p}$, are called **statistics**\index{statistic}. Numbers that summarize an entire population, such as $\pi$, are called **parameters**\index{parameter}. You can remember
this distinction by looking at the first letter of each term: 

> **S**tatistics summarize **S**amples; **P**arameters summarize **P**opulations.

We typically use Roman letters to symbolize statistics (e.g., $\bar{x}$, $\hat{p}$), and Greek letters to symbolize parameters (e.g., $\mu$, $\pi$).
Since we rarely can measure the entire population, and thus rarely know
the actual parameter values, we like to say, "We don't know Greek,
and we don't know parameters!"</div>\EndKnitrBlock{protip}

\BeginKnitrBlock{example}<div class="example">Write out hypotheses in both plain and statistical language to test for the association between the consultant's work and the true complication rate, $\pi$, for the consultant's clients.

---
  
In words:

> $H_0$: There is no association between the consultant's contributions and the clients' complication rate.  
> $H_A$: Patients who work with the consultant tend to have a complication rate lower than 10%. In statistical language

In statistical language:

> $H_0: \pi=0.10$  
> $H_A: \pi<0.10$</div>\EndKnitrBlock{example}

To assess these hypotheses, we need to evaluate the possibility of a sample value ($\hat{p}$) as far below the null value, $\pi_0=0.10$ as what was observed. 

\BeginKnitrBlock{onebox}<div class="onebox">**Null value of a hypothesis test.**

The **null value** is the reference value for the parameter in $H_0$, and it is sometimes represented with the parameter's label with a subscript 0 (or "null"), e.g., $\pi_0$ (just like $H_0$).</div>\EndKnitrBlock{onebox}

The deviation of the sample value from the null hypothesized parameter is usually quantified with a p-value. The p-value is computed based on the null distribution, which is the distribution of the test statistic if the null hypothesis is true. Supposing the null hypothesis is true, we can compute the p-value by identifying the chance of observing a test statistic that favors the alternative hypothesis at least as strongly as the observed test statistic. 

The null distribution can be created through simulation (simulation-based methods),
or can be modeled by a mathematical function (theory-based methods).


#### Simulation-based method for calculating the p-value


#### Theory-based method for calculating the p-value


e want to identify the sampling distribution of the test statistic ($\hat{p}$) if the null hypothesis was true. In other words, we want to see how the sample proportion changes due to chance alone. Then we plan to use this information to decide whether there is enough evidence to reject the null hypothesis.

Under the null hypothesis, 10% of liver donors have complications during or after surgery. Suppose this rate was really no different for the consultant's clients (for *all* the consultant's clients, not just the 62 previously measured). If this was the case, we could *simulate* 62 clients to get a sample proportion for the complication rate from the null distribution.  Simulating observations using a hypothesized null parameter value is often called a **parametric bootstrap simulation**\index{parametric bootstrap}.





Similar to the process described in Section \@ref(boot-ci), each client can be simulated using a bag of marbles with 10% red marbles and 90% white marbles.
Sampling a marble from the bag (with 10% red marbles) is one way of simulating whether a patient has a complication *if the true complication rate is 10%* for the data. If we select 62 marbles and then compute the proportion of patients with complications in the simulation, $\hat{p}_{sim}$, then the resulting sample proportion is exactly a sample from the null distribution.

An undergraduate student was paid $2 to complete this simulation. There were 5 simulated cases with a complication and 57 simulated cases without a complication, i.e., $\hat{p}_{sim} = 5/62 = 0.081$.

\BeginKnitrBlock{example}<div class="example">Is this one simulation enough to determine whether or not we should reject the null hypothesis?

---
  
No. To assess the hypotheses, we need to see a distribution of many $\hat{p}_{sim}$, not just a *single* draw from this sampling distribution.</div>\EndKnitrBlock{example}

#### Observed statistic vs. null statistics {-}

One simulation isn't enough to get a sense of the null distribution; many simulation studies are needed. Roughly 10,000 seems sufficient. However, paying someone to simulate 10,000 studies by hand is a waste of time and money. Instead, simulations are typically programmed into a computer, which is much more efficient.

Figure \@ref(fig:nullDistForPHatIfLiverTransplantConsultantIsNotHelpful) shows the results of 10,000 simulated studies. The proportions that are equal to or less than $\hat{p}=0.048$ are shaded. The shaded areas represent sample proportions under the null distribution that provide at least as much evidence as $\hat{p}$ favoring the alternative hypothesis. There were 1222 simulated sample proportions with $\hat{p}_{sim} \leq 0.048$. We use these to construct the null distribution's left-tail area and find the p-value:
\begin{align}
\text{left tail area }\label{estOfPValueBasedOnSimulatedNullForSingleProportion}
	&= \frac{\text{Number of observed simulations with }\hat{p}_{sim}\leq\text{ 0.048}}{10000}
\end{align}
Of the 10,000 simulated $\hat{p}_{sim}$, 1222 were equal to or smaller than $\hat{p}$. Since the hypothesis test is one-sided, the estimated p-value is equal to this tail area: 0.1222.


<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/nullDistForPHatIfLiverTransplantConsultantIsNotHelpful-1.png" alt="The null distribution for $\hat{p}$, created from 10,000 simulated studies. The left tail, representing the p-value for the hypothesis test, contains 12.22% of the simulations." width="70%" />
<p class="caption">(\#fig:nullDistForPHatIfLiverTransplantConsultantIsNotHelpful)The null distribution for $\hat{p}$, created from 10,000 simulated studies. The left tail, representing the p-value for the hypothesis test, contains 12.22% of the simulations.</p>
</div>

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Because the estimated p-value is 0.1222, which is larger than the significance level 0.05, we do not reject the null hypothesis. Explain what this means in plain language in the context of the problem.^[There isn't sufficiently strong evidence to support an association between the consultant's work and fewer surgery complications.]</div>\EndKnitrBlock{guidedpractice}

\index{data!medical consultant|)}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Does the conclusion in the previous Guided Practice imply there is no real association between the surgical consultant's work and the risk of complications? Explain.^[No. It might be that the consultant's work is associated with a reduction but that there isn't enough data to convincingly show this connection.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{onebox}<div class="onebox">**Null distribution of $\hat{p}$ with bootstrap simulation**
  
Regardless of the statistial method chosen, the p-value is always derived by analyzing the null distribution of the test statistic. The normal model poorly approximates the null distribution for $\hat{p}$ when the success-failure condition is not satisfied. As a substitute, we can generate the null distribution using simulated sample proportions and use this distribution to compute the tail area, i.e., the p-value.</div>\EndKnitrBlock{onebox}


In the previous Guided Practice, the p-value is *estimated*.
It is not exact because the simulated null distribution itself is not exact, only a close approximation.
An exact p-value can be generated using the binomial distribution, but that method will not be covered in this text.

<!--
#### Generating the exact null distribution and p-value  {-} {#exactNullDistributionUsingBinomialModel}

The number of successes in $n$ independent cases can be described using the binomial model, which was introduced in Section \ref{binomialModel}. Recall that the probability of observing exactly $k$ successes is given by
\begin{align} \label{binomialEquationShownForFindingNullDistributionInSmallSamplePropTest}
P(k\text{ successes}) = {n\choose k} p^{k}(1-p)^{n-k} = \frac{n!}{k!(n-k)!} p^{k}(1-p)^{n-k}
\end{align}
where $p$ is the true probability of success. The expression ${n\choose k}$ is read as \emph{$n$ choose $k$}, and the exclamation points represent factorials. For instance, $3!$ is equal to $3\times 2\times 1=6$, $4!$ is equal to $4\times 3\times 2\times 1 = 24$, and so on (see Section \ref{binomialModel}).

The tail area of the null distribution is computed by adding up the probability in Equation \eqref{binomialEquationShownForFindingNullDistributionInSmallSamplePropTest} for each $k$ that provides at least as strong of evidence favoring the alternative hypothesis as the data. If the hypothesis test is one-sided, then the p-value is represented by a single tail area. If the test is two-sided, compute the single tail area and double it to get the p-value, just as we have done in the past.

\begin{example}{Compute the exact p-value to check the consultant's claim that her clients' complication rate is below 105.}
Exactly $k=3$ complications were observed in the $n=62$ cases cited by the consultant. Since we are testing against the 10% national average, our null hypothesis is $p=0.10$. We can compute the p-value by adding up the cases where there are 3 or fewer complications:
\begin{align*}
\text{p-value}
	&= \sum_{j=0}^{3} {n\choose j} p^{j}(1-p)^{n-j} \\
	&= \sum_{j=0}^{3} {62\choose j} 0.1^{j}(1-0.1)^{62-j} \\
	&= {62\choose 0} 0.1^{0}(1-0.1)^{62-0} +
		{62\choose 1} 0.1^{1}(1-0.1)^{62-1} \\
	& \qquad + {62\choose 2} 0.1^{2}(1-0.1)^{62-2} +
		{62\choose 3} 0.1^{3}(1-0.1)^{62-3} \\
	&= 0.0015 + 0.0100 + 0.0340 + 0.0755 \\
	&= 0.1210
\end{align*}
This exact p-value is very close to the p-value based on the simulations (0.1222), and we come to the same conclusion. We do not reject the null hypothesis, and there is not statistically significant evidence to support the association.

If it were plotted, the exact null distribution would look almost identical to the simulated null distribution shown in Figure \ref{nullDistForPHatIfLiverTransplantConsultantIsNotHelpful} on page \pageref{nullDistForPHatIfLiverTransplantConsultantIsNotHelpful}.
\end{example}

-->


### Confidence interval for $\pi$

#### Conditions {-}

In Section \@ref(normalDist), we introduced the normal distribution and showed how it can be used as a mathematical model to describe the variability of a statistic.
There are conditions under which a sample proportion $\hat{p}$ is well modeled using a normal distribution.
When the sample observations
are independent and the sample size is sufficiently
large, the normal model will describe the variability quite well; when the observations violate the conditions, the normal model can be inaccurate

\BeginKnitrBlock{onebox}<div class="onebox">**Sampling distribution of
    $\hat{p}$**  

  The sampling distribution for $\hat{p}$ based on
  a sample of size $n$ from a population with a true
  proportion $p$ is nearly normal when:

1. The sample's observations are independent,
      e.g., are from a simple random sample.
2. We expected to see at least 10 successes and
      10 failures in the sample, i.e., $np\geq10$ and
      $n(1-p)\geq10$.
      This is called the **success-failure condition**.

  When these conditions are met, then the sampling
  distribution of $\hat{p}$ is nearly normal with mean
  $p$ and standard error of $\hat{p}$ as $SE = \sqrt{\frac{\ p(1-p)\ }{n}}$.</div>\EndKnitrBlock{onebox}

\index{success-failure condition}
\index{standard error (SE)!single proportion}
  


Typically we don't know the true proportion $p$,
so we substitute some value to check conditions
and estimate the standard error.
For confidence intervals, the sample proportion
$\hat{p}$ is used to check the success-failure condition
and compute the standard error.
For hypothesis tests, typically the null value --
that is, the proportion claimed in the null hypothesis --
is used in place of $p$.

The independence condition is a more nuanced requirement.
When it isn't met, it is important to understand how and why
it isn't met.
For example, there exist no statistical methods available to truly correct the inherent biases of data from a convenience sample.
On the other hand, if we took a cluster sample
(see Section \@ref(samp-methods)), the observations wouldn't be independent, but suitable statistical methods are available for analyzing the data (but they are beyond the scope of even most second or third courses
in statistic).

\BeginKnitrBlock{example}<div class="example">In the examples based on large sample theory, we modeled $\hat{p}$ using the normal distribution. Why is this not appropriate for the case study on the medical consultant?
  
---
  
The independence assumption may be reasonable if each of the surgeries is from a different surgical team. However, the success-failure condition is not satisfied. Under the null hypothesis, we would anticipate seeing $62\times 0.10=6.2$ complications, not the 10 required for the normal approximation.</div>\EndKnitrBlock{example}


While this book is scoped to well-constrained statistical
problems, do remember that this is just the first
book in what is a large library of statistical methods that
are suitable for a very wide range of data and contexts.


#### Confidence interval for $p$  {-}

\index{point estimate!single proportion}

A confidence interval provides a range of
plausible values for the parameter $p$,
and when $\hat{p}$ can be modeled using a
normal distribution, the confidence interval
for $p$ takes the form
\begin{align*}
\hat{p} \pm z^{\star} \times SE.
\end{align*}

We have seen $\hat{p}$ to be the sample proportion.  The value $z^{\star}$ determines the confidence level (previously set to be 1.96) and will be discussed in detail in the examples following.  The value of the standard error, $SE$, depends heavily on the sample size.

\BeginKnitrBlock{onebox}<div class="onebox">**Standard Error of one proportion, $\hat{p}$**
  
When the conditions are met so that the distribution fo $\hat{p}$ is nearly normal, the **variability** of a single proportion, $\hat{p}$ is well described by:
  
$$SE(\hat{p}) = \sqrt{\frac{p(1-p)}{n}}$$
  
Note that we almost never know the true value of $p$.  A more helpful formula to use is:
  
$$SE(\hat{p}) \approx \sqrt{\frac{(\mbox{best guess of }p)(1 - \mbox{best guess of }p)}{n}}$$
  
For hypothesis testing, we often use $p_0$ as the best guess of $p$.  For confidence intervals, we typically use $\hat{p}$ as the best guess of $p$.</div>\EndKnitrBlock{onebox}

\index{data!Payday regulation poll|(}

<!--
\newcommand{\paydayN}{826}
\newcommand{\paydayNHalf}{413}
\newcommand{\paydayRegPerc}{70\%}
\newcommand{\paydayRegProp}{0.70}
\newcommand{\paydayRegSE}{0.016}
\newcommand{\paydayRegSEPerc}{1.6\%}
\newcommand{\paydayRegLower}{0.669}
\newcommand{\paydayRegUpper}{0.731}
\newcommand{\paydayRegLowerPerc}{66.9\%}
\newcommand{\paydayRegUpperPerc}{73.1\%}
% https://www.pewtrusts.org/-/media/assets/2017/04/payday-loan-customers-want-more-protections-methodology.pdf

did search and replace for each term above.  for example 826 for 826

-->

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Consider taking many polls of registered voters (i.e., random samples) of size 300 asking them if they support legalized marijuana.
It is suspected that about 2/3 of all voters support legalized marijuana.
To understand how the sample proportion ($\hat{p}$) would vary across the samples, calculate the standard error of $\hat{p}$.^[Because the $p$ is unknown but expected to be around 2/3, we will use 2/3 in place of $p$ in the formula for the standard error.  
$SE = \sqrt{\frac{p(1-p)}{n}} \approx
    \sqrt{\frac{2/3 (1 - 2/3)}
        {300}} = 0.027$.]
</div>\EndKnitrBlock{guidedpractice}

##### Variability of the statistic {-}

\BeginKnitrBlock{example}<div class="example">
A simple random sample of 826
    payday loan borrowers was surveyed to better
    understand their interests around regulation and costs.
    70% of the responses supported new
    regulations on payday lenders.

  1. Is it reasonable to model the variability of $\hat{p}$ from sample to sample
    using a normal distribution?
    
  2. Estimate the standard error of $\hat{p}$.

  3. Construct a 95% confidence interval for $p$,
    the proportion of payday borrowers who support increased
    regulation for payday lenders.  
    
---
      
1.  The data are a random sample, so the observations are
  independent and representative of the population of
  interest.

  We also must check the success-failure condition,
  which we do using $\hat{p}$ in place
  of $p$ when computing a confidence interval:
  \begin{align*}
  \text{Support: }
      n p &
          \approx 826 \times 0.70
      = 578
  &\text{Not: }
      n (1 - p) &
        \approx 826 \times (1 - 0.70)
      = 248
  \end{align*}
  Since both values are at least 10, we can use the normal
  distribution to model $\hat{p}$.
  
2. Because $p$ is unknown and the standard error is for
a confidence interval, use $\hat{p}$ in place of $p$
in the formula.

$SE = \sqrt{\frac{p(1-p)}{n}} \approx
    \sqrt{\frac{0.70 (1 - 0.70)}
        {826}} = 0.016$.

3.       
  Using
  the point estimate 0.70,
  $z^{\star} = 1.96$ for a 95% confidence interval,
  and
  the standard error $SE = 0.016$ from the pervious
  Guided Practice,
  the confidence interval is
  \begin{eqnarray*}
  \text{point estimate} \ \pm\ z^{\star} \times SE
      \quad\to\quad
      0.70 \ \pm\ 1.96 \times 0.016
      \quad\to\quad
      (0.669, 0.731)
  \end{eqnarray*}
  We are 95% confident that the true proportion of
  payday borrowers who supported regulation at the time
  of the poll was between 0.669 and
  0.731.</div>\EndKnitrBlock{example}




\BeginKnitrBlock{onebox}<div class="onebox">**Constructing a confidence interval for a single proportion**

There are three steps to constructing a confidence
  interval for $p$.

1. Check independence and the success-failure condition
      using $\hat{p}$.
      If the conditions are met, the sampling distribution
      of $\hat{p}$ may be well-approximated by the normal model.
2. Construct the standard error using $\hat{p}$
      in place of $p$ in the standard error formula.
3. Apply the general confidence interval formula.</div>\EndKnitrBlock{onebox}


For additional one-proportion confidence interval examples,
see Section \@ref(ConfidenceIntervals).

#### Changing the confidence level {-}

\index{confidence interval!confidence level|(}

Suppose we want to consider confidence intervals where the confidence level is somewhat higher than 95%: perhaps we would like a confidence level of 99%. Think back to the analogy about trying to catch a fish: if we want to be more sure that we will catch the fish, we should use a wider net. To create a 99% confidence level, we must also widen our 95% interval. On the other hand, if we want an interval with lower confidence, such as 90%, we could make our original 95% interval slightly slimmer.

The 95% confidence interval structure provides guidance in how to make intervals with new confidence levels. Below is a general 95% confidence interval for a point estimate that comes from a nearly normal distribution:
\begin{eqnarray}
\text{point estimate}\ \pm\ 1.96\times SE
\end{eqnarray}
There are three components to this interval: the point estimate, "1.96", and the standard error. The choice of $1.96\times SE$ was based on capturing 95% of the data since the estimate is within 1.96 standard errors of the true value about 95% of the time. The choice of 1.96 corresponds to a 95% confidence level. 

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If $X$ is a normally distributed random variable, how often will $X$ be within 2.58 standard deviations of the mean?^[This is equivalent to asking how often the $Z$ score will be larger than -2.58 but less than 2.58. (For a picture, see Figure \@ref(fig:choosingZForCI).) To determine this probability, look up -2.58 and 2.58 in the normal probability table (0.0049 and 0.9951). Thus, there is a $0.9951-0.0049 \approx 0.99$ probability that the unobserved random variable $X$ will be within 2.58 standard deviations of the mean.]</div>\EndKnitrBlock{guidedpractice}


<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/choosingZForCI-1.png" alt="The area between -$z^{\star}$ and $z^{\star}$ increases as $|z^{\star}|$ becomes larger. If the confidence level is 99%, we choose $z^{\star}$ such that 99% of the normal curve is between -$z^{\star}$ and $z^{\star}$, which corresponds to 0.5% in the lower tail and 0.5% in the upper tail: $z^{\star}=2.58$." width="70%" />
<p class="caption">(\#fig:choosingZForCI)The area between -$z^{\star}$ and $z^{\star}$ increases as $|z^{\star}|$ becomes larger. If the confidence level is 99%, we choose $z^{\star}$ such that 99% of the normal curve is between -$z^{\star}$ and $z^{\star}$, which corresponds to 0.5% in the lower tail and 0.5% in the upper tail: $z^{\star}=2.58$.</p>
</div>

\index{confidence interval!confidence level|)}


To create a 99% confidence interval, change 1.96 in the 95% confidence interval formula to be $2.58$. The previous Guided Practice highlights that 99% of the time a normal random variable will be within 2.58 standard deviations of its mean. This approach -- using the Z scores in the normal model to compute confidence levels -- is appropriate when the point estimate is associated with a normal distribution and we can properly compute the standard error. Thus, the formula for a 99% confidence interval is:

\begin{eqnarray*}
\text{point estimate}\ \pm\ 2.58\times SE
\end{eqnarray*}

<!--
label for previous equation?
\label{99PercCIForMean}
\label{99PercCIForNormalPointEstimate}

%\Comment{I don't know where the equation number above gets referenced. Might drop the equation number.}
-->

The normal approximation is crucial to the precision of the $z^\star$ confidence intervals (in contrast to the bootstrap confidence intervals). When the normal model is not a good fit, we will use alternative distributions that better characterize the sampling distribution or we will use bootstrapping procedures.


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Create a 99% confidence interval for the impact of the stent on the risk of stroke using the data from Section \@ref(basic-stents-strokes). The point estimate is 0.090, and the standard error is $SE = 0.028$. It has been verified for you that the point estimate can reasonably be modeled by a normal distribution.^[Since the necessary conditions for applying the normal model have already been checked for us, we can go straight to the construction of the confidence interval: $\text{point estimate}\ \pm\ 2.58 \times SE \rightarrow (0.018, 0.162)$. We are 99% confident that implanting a stent in the brain of a patient who is at risk of stroke increases the risk of stroke within 30 days by a rate of 0.018 to 0.162 (assuming the patients are representative of the population).]</div>\EndKnitrBlock{guidedpractice}



\BeginKnitrBlock{onebox}<div class="onebox">**Mathematical model confidence interval for any confidence level.**

If the point estimate follows the normal model with standard error $SE$, then a confidence interval for the population parameter is
\begin{eqnarray*}
\text{point estimate}\ \pm\ z^{\star} \times SE
\end{eqnarray*}
where $z^{\star}$ corresponds to the confidence level selected.</div>\EndKnitrBlock{onebox}

Figure \@ref(fig:choosingZForCI) provides a picture of how to identify $z^{\star}$ based on a confidence level. We select $z^{\star}$ so that the area between -$z^{\star}$ and $z^{\star}$ in the normal model corresponds to the confidence level. 


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Previously, we found that implanting a stent in the brain of a patient at risk for a stroke *increased* the risk of a stroke. The study estimated a 9% increase in the number of patients who had a stroke, and the standard error of this estimate was about $SE = 2.8%$. Compute a 90% confidence interval for the effect.^[We must find $z^{\star}$ such that 90% of the distribution falls between -$z^{\star}$ and $z^{\star}$ in the standard normal model, $N(\mu=0, \sigma=1)$. We can look up -$z^{\star}$ in the normal probability table by looking for a lower tail of 5% (the other 5% is in the upper tail), thus $z^{\star}=1.65$. The 90% confidence interval can then be computed as $\text{point estimate}\ \pm\ 1.65\times SE \to (4.4\%, 13.6\%)$. (Note: the conditions for normality had earlier been confirmed for us.) That is, we are 90% confident that implanting a stent in a stroke patient's brain increased the risk of stroke within 30 days by 4.4% to 13.6%.]</div>\EndKnitrBlock{guidedpractice}

#### Hypothesis test for $H_0: p = p_0$ {-}

<!--
\label{htForPropSection}

\newcommand{\paydayCCPerc}{51\%}
\newcommand{\paydayCCProp}{0.51}
\newcommand{\paydayCCSE}{0.017}
\newcommand{\paydayCCSEPerc}{1.7\%}
\newcommand{\paydayCCZ}{0.59}
\newcommand{\paydayCCOneTail}{0.2776}
\newcommand{\paydayCCPvalue}{0.5552}
-->

One possible regulation for payday lenders is that they
would be required to do a credit check and evaluate debt
payments against the borrower's finances.
We would like to know: would borrowers support this form
of regulation?

<!--
\label{paydayCC_hypotheses_gp}%
-->

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Set up hypotheses to evaluate whether borrowers
have a majority support for this
type of regulation.^[$H_0$: there is not support for the regulation; $H_0$: $p \leq 0.50$. $H_A$: the majority of borrowers support the regulation; $H_A$: $p > 0.50$.]</div>\EndKnitrBlock{guidedpractice}

To apply the normal distribution framework in the context
of a hypothesis test for a proportion, the independence
and success-failure conditions must be satisfied.
In a hypothesis test, the success-failure condition is
checked using the null proportion:
we verify $np_0$ and $n(1-p_0)$ are at least 10,
where $p_0$ is the null value.



<!--
\label{paydayCC_conditions_gp}%
-->
\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Do payday loan borrowers support a regulation
that would
require lenders to pull their credit report
and evaluate their debt payments?
From a random sample of 826 borrowers,
51% said they would support such
a regulation.
Is it reasonable use a normal distribution to model $\hat{p}$
for a hypothesis test here?^[Independence holds since the poll
    is based on a random sample.
    The success-failure condition also holds,
    which is checked
    using the null value ($p_0 = 0.5$) from $H_0$:
    $np_0 = 826 \times 0.5 = 413$,
    $n(1 - p_0) = 826 \times 0.5 = 413$.  Recall that here, the best guess for $p$ is $p_0$ which comes from the null hypothesis (because we assume the null hypothesis is true when performing the testing procedure steps).  $H_0$: there is not support for the regulation; $H_0$: $p \leq 0.50$. $H_A$: the majority of borrowers support the regulation; $H_A$: $p > 0.50$.]</div>\EndKnitrBlock{guidedpractice}

    
\BeginKnitrBlock{example}<div class="example">Using the hypotheses and data from the previous
    Guided Practices,
    evaluate whether the poll on lending regulations provides convincing evidence
    that a majority of payday loan borrowers support
    a new regulation that would
    require lenders to pull credit reports
    and evaluate debt payments.
    
---
      
  With hypotheses already set up and conditions checked,
  we can move onto calculations.
  The standard error in the context of a one-proportion
  hypothesis test is computed using the null value, $p_0$:
  \begin{align*}
  SE = \sqrt{\frac{p_0 (1 - p_0)}{n}}
      = \sqrt{\frac{0.5 (1 - 0.5)}{826}}
      = 0.017
  \end{align*}
  A picture of the normal model is shown below
  with the p-value represented by the shaded region.

  Based on the normal model, the test statistic can be
  computed as the Z-score of the point estimate:
  \begin{align*}
  Z = \frac{\text{point estimate} - \text{null value}}{SE}
      = \frac{0.51 - 0.50}{0.017}
      = 0.59
  \end{align*}
  The single tail area which represents the p-value is 0.2776.
  Because the p-value is larger than 0.05,
  we do not reject $H_0$.
  The poll does not provide convincing evidence that
  a majority of payday loan borrowers support regulations around credit checks and evaluation of
  debt payments.
  
  In Section \@ref(two-prop-errors) we discuss two-sided hypothesis tests of which the payday example may have been better structured.  
That is, we might have wanted to ask whether the borrows **support or oppose** the regulations (to study opinion in either direction away from the 50% benchmark).
In that case, the p-value would have been doubled to 0.5552 (again, we would not reject $H_0$).
In the two-sided hypothesis setting, the appropriate conclusion would be to claim that the poll does not provide convincing evidence that a majority of payday loan borrowers support  or oppose regulations around credit checks and evaluation of debt payments.

In both the one-sided or two-sided setting, the conclusion is somewhat unsatisfactory because there is no conclusion.
That is, there is no resolution one way or the other about public opinion.
We cannot claim that exactly 50% of people support the regulation, but we cannot claim a majority in either direction.</div>\EndKnitrBlock{example}

  
<img src="05-inference-cat_files/figure-html/paydayCC-norm-pvalue-1.png" width="70%" style="display: block; margin: auto;" />

<!--
\oneprophtsummary{}
-->

\BeginKnitrBlock{onebox}<div class="onebox">**Mathematical model hypothesis test for a proportion.**
  
Set up hypotheses and verify the conditions using the null value, $p_0$, to ensure $\hat{p}$ is nearly normal under $H_0$. If the conditions hold, construct the standard error, again using $p_0$, and show the p-value in a drawing. Lastly, compute the p-value and evaluate the hypotheses.</div>\EndKnitrBlock{onebox}


For additional one-proportion hypothesis test examples,
see Section \@ref(HypothesisTesting).

\index{data!Payday regulation poll|)}
<!--
\CalculatorVideos{confidence intervals and hypothesis tests for a single proportion}
-->

#### Violating conditions {-}

We've spent a lot of time discussing conditions for when
$\hat{p}$ can be reasonably modeled by a normal distribution.
What happens when the success-failure condition fails?
What about when the independence condition fails?
In either case, the general ideas of confidence intervals
and hypothesis tests remain the same, but the strategy
or technique used to generate the interval or p-value
change.

When the success-failure condition isn't met
for a hypothesis test, we can simulate the null distribution 
of $\hat{p}$ using the null value, $p_0$, as seen in Section \@ref(one-prop-null-boot).  Unfortunately, methods for dealing with observations which are not independent are outside the scope of this book.


<!--
#### Choosing a sample size when estimating a proportion {-}

\BeginKnitrBlock{onebox}<div class="onebox">**Margin of error.**

In a confidence interval, $z^{\star}\times SE$ is called the **margin of error**\index{margin of error}.</div>\EndKnitrBlock{onebox}






\index{margin of error|(}

When collecting data, we choose a sample size suitable
for the purpose of the study. 
You might agree that the following interval estimate would not be particularly useful: a 95% confidence interval for the proportion of liver transplants with complications is between 0 and 1.
Often times "suitable for the study" means choosing a sample size large
enough that the **margin of error**\index{margin of error} --
which is the part we add and subtract from the point
estimate in a confidence interval --
is sufficiently small that the result is useful.
For example, our task might be to find a sample size
$n$ so that the sample proportion is within $\pm 0.04$
of the actual proportion in a 95% confidence interval.

<!--
% For example, the margin of error for a point estimate using 95% confidence can be written as $1.96\times SE$. We set up a general equation to represent the problem:
%\begin{align*}
%ME = z^{\star} \times SE \leq m
%\end{align*}
%where $ME$ represented the actual margin of error and $z^{\star}$ was chosen to correspond to the confidence level. The standard error formula is specified to correspond to the particular setting. For instance, in the case of means, the standard error was given as $\sigma / \sqrt{n}$. In the case of a single proportion, we use $\sqrt{p(1-p) / n\ }$ for the standard error.


\index{data!Student football stadium|(}

\BeginKnitrBlock{example}<div class="example">A university newspaper is conducting
    a survey to determine what fraction of students
    support a $200 per year increase in fees to pay
    for a new football stadium.
    How big of a sample is required to ensure the
    margin of error is smaller than 0.04 using a
    95% confidence level?

---
      
  The margin of error for a sample proportion is
  \begin{align*}
  z^{\star} \sqrt{\frac{p (1 - p)}{n}}
  \end{align*}
  Our goal is to find the smallest sample size $n$
  so that this margin of error is smaller than $0.04$.
  For a 95% confidence level, the value $z^{\star}$
  corresponds to 1.96:
  \begin{align*}
  1.96\times \sqrt{\frac{p(1-p)}{n}} \ < \ 0.04
  \end{align*}
  There are two unknowns in the equation: $p$ and $n$.
  If we have an estimate of $p$, perhaps from a prior
  survey, we could enter in that value and solve for $n$.
  If we have no such estimate, we must use some other
  value for $p$.
  It turns out that the margin of error is largest
  when $p$ is 0.5, so we typically use this
  \emph{worst case value} if no estimate of the
  proportion is available:
  \begin{align*}
	1.96\times \sqrt{\frac{0.5(1-0.5)}{n}} &\ < \ 0.04 \\
	1.96^2\times \frac{0.5(1-0.5)}{n} &\ < \ 0.04^2 \\
	1.96^2\times \frac{0.5(1-0.5)}{0.04^2} &\ < \ n \\
	600.25 &\ < \  n
  \end{align*}
  We would need over 600.25 participants, which means
  we need 601 participants or more, to ensure the
  sample proportion is within 0.04 of the true proportion
  with 95% confidence.</div>\EndKnitrBlock{example}

\index{data!Student football stadium|)}

When an estimate of the proportion is available, we use it in place of the worst case proportion value, 0.5.


\index{data!Tire failure rate|(}

\BeginKnitrBlock{todo}<div class="todo">not sure why the footnote didn't go into the footnote</div>\EndKnitrBlock{todo}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">A manager is about to oversee the mass
production of a new tire model in her factory,
and she would like to estimate what proportion of
these tires will be rejected through quality control.
The quality control team has monitored the last three
tire models produced by the factory,
failing 1.7% of tires in the first model,
6.2% of the second model,
and 1.3% of the third model.
The manager would like to examine enough tires
to estimate the failure rate of the new tire model
to within about 1% with a 90% confidence level.
There are three different failure rates to choose from.
Perform the sample size computation for each separately,
and identify three sample sizes to consider.^[For a 90% confidence interval, $z^{\star} = 1.65$,
  and since an estimate of the proportion 0.017 is available,
  we'll use it in the margin of error formula:
  \begin{align*}
  1.65\times \sqrt{\frac{0.017(1-0.017)}{n}} &\ < \ 0.01
    \qquad\to\qquad
      \frac{0.017(1-0.017)}{n} \ < \ 
          \left(\frac{0.01}{1.65}\right)^2
    \qquad\to\qquad
      454.96 \ < \ n
  \end{align*}
  For sample size calculations, we always round up,
  so the first tire model suggests 455 tires would
  be sufficient.

  A similar computation can be accomplished using 0.062
  and 0.013 for $p$, and you should verify that using these
  proportions results in minimum sample sizes of 1584
  and 350 tires, respectively.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{example}<div class="example">The sample sizes vary widely in the previous
    Guided Practice.
    Which of the three would you suggest using?
    What would influence your choice?

---
      
  We could examine which of the old models is most
  like the new model, then choose the corresponding sample
  size.
  Or if two of the previous estimates are based on small
  samples while the other is based on a larger sample,
  we might consider the value corresponding to the larger
  sample.
  There are also other reasonable approaches.

  Also observe that the success-failure
  condition would need to be checked in the final sample.
  For instance, if we sampled $n = 1584$ tires and found
  a failure rate of 0.5%, the normal approximation would
  not be reasonable, and we would require more advanced
  statistical methods for creating the confidence interval.</div>\EndKnitrBlock{example}

\index{data!Tire failure rate|)}
\index{data!Payday regulation poll|(}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Suppose we want to continually track the support
of payday borrowers for regulation on lenders,
where we would conduct a new poll every month.
Running such frequent polls is expensive, so we decide
a wider margin of error of 5% for each individual survey
would be acceptable.
Based on a previous sample of borrowers where
70% supported some form of regulation,
how big should our monthly sample be for a margin
of error of 0.04 with 95% confidence?^[We complete the same computations as before,
   except now we use $0.70$ instead of $0.5$
   for $p$:
   \begin{align*}
   1.96\times \sqrt{\frac{p(1-p)}{n}}
       \approx 1.96\times
           \sqrt{\frac{0.70(1-0.70)}
               {n}}
       &\leq 0.05
     \qquad\to\qquad
       n \geq 322.7
  \end{align*}
  A sample size of 323 or more would be reasonable.
  (Reminder: always round up for sample size calculations!)
  Given that we plan to track this poll over time,
  we also may want to periodically repeat these calculations
  to ensure that we're being thoughtful in our sample
  size recommendations in case the baseline rate fluctuates.]</div>\EndKnitrBlock{guidedpractice}

\index{data!Payday regulation poll|)}
\index{margin of error|)}

{\input{ch_inference_for_props/TeX/inference_for_a_single_proportion.tex}}
-->



## Difference of two proportions {#diff-two-prop}

We now extend the methods from Section \@ref(single-prop) to apply confidence intervals and hypothesis tests to differences in population proportions that come from two groups: $p_1 - p_2$.

<!--
%We consider three examples.
%In the first, we compare the utility of a blood thinner
%for heart attack patients.
%In the second application, we examine the efficacy of
%mammograms in reducing deaths from breast cancer.
%In the last example, a quadcopter company weighs whether
%to switch to a higher quality manufacturer of rotor blades.
-->

In our investigations, we'll identify a reasonable
point estimate of $p_1 - p_2$ based on the sample,
and you may have already guessed its form:
$\hat{p}_1 - \hat{p}_2$.
\index{point estimate!difference of proportions}
Then we'll look at the inferential analysis in three different ways: using a randomization test, applying bootstrapping for interval estimates, and, if
we verify that the point estimate
can be modeled using a normal distribution,
we compute the estimate's standard error, and
we apply the mathematical framework.



### Randomization test for $H_0: p_1 - p_2 = 0$ {#two-prop-errors}

#### Observed data {-}

We consider a study on a new malaria vaccine
called PfSPZ.
In this study, volunteer patients were randomized
into one of two experiment groups:
14 patients received an experimental vaccine
or 6 patients received a placebo vaccine.
Nineteen weeks later, all 20 patients were exposed
to a drug-sensitive malaria virus strain;
the motivation of using a drug-sensitive strain
of virus here is for ethical considerations,
allowing any infections to be treated effectively.
The results are summarized in
Table \@ref(tab:malaria-vaccine-20-exp-summary),
where 9 of the 14 treatment patients remained free
of signs of infection while all of the 6 patients
in the control group patients showed some baseline
signs of infection.

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:malaria-vaccine-20-exp-summary)Summary results for the malaria vaccine experiment.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">`outcome`</div></th>
<th style="border-bottom:hidden" colspan="1"></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> infection </th>
   <th style="text-align:left;"> no infection </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> vaccine </td>
   <td style="text-align:left;"> 5 </td>
   <td style="text-align:left;"> 9 </td>
   <td style="text-align:left;"> 14 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> `treatment` </td>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:left;"> 6 </td>
   <td style="text-align:left;"> 0 </td>
   <td style="text-align:left;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 11 </td>
   <td style="text-align:left;"> 9 </td>
   <td style="text-align:left;"> 20 </td>
  </tr>
</tbody>
</table>

<!--
\newcommand{\malariaAA}{5}
\newcommand{\malariaAB}{9}
\newcommand{\malariaAD}{14}
\newcommand{\malariaBA}{6}
\newcommand{\malariaBB}{0}
\newcommand{\malariaBD}{6}
\newcommand{\malariaDA}{11}
\newcommand{\malariaDB}{9}
\newcommand{\malariaDD}{20}
\newcommand{\malariaVIR}{0.357}
\newcommand{\malariaVIRPerc}{35.7\%}
\newcommand{\malariaPIR}{1.000}
\newcommand{\malariaPIRPerc}{100\%}
\newcommand{\malariaIRDiff}{0.643}
\newcommand{\malariaIRDiffPerc}{64.3\%}
-->


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Is this an observational study or an experiment?
What implications does the study type have on what can
be inferred from the results?^[The
  study is an experiment, as patients were randomly
  assigned an experiment group.
  Since this is an experiment, the results can be used
  to evaluate a causal relationship between the malaria
  vaccine and whether patients showed signs
  of an infection.]</div>\EndKnitrBlock{guidedpractice}

In this study, a smaller proportion of patients
who received the vaccine showed signs of an infection
(35.7% versus 100%).
However, the sample is very small,
and it is unclear whether the difference provides
*convincing evidence* that the vaccine is
effective.

<!--
<div class="example">
<p>Data scientists are sometimes called upon to evaluate the strength of evidence. When looking at the rates of infection for patients in the two groups in this study, what comes to mind as we try to determine whether the data show convincing evidence of a real difference?</p>
<hr />
<p>The observed infection rates (35.7% for the treatment group versus 100% for the control group) suggest the vaccine may be effective. However, we cannot be sure if the observed difference represents the vaccine’s efficacy or is just from random chance. Generally there is a little bit of fluctuation in sample data, and we wouldn’t expect the sample proportions to be <em>exactly</em> equal, even if the truth was that the infection rates were independent of getting the vaccine. Additionally, with such small samples, perhaps it’s common to observe such large differences when we randomly split a group due to chance alone!</p>
</div>

The previous malaria example
is a reminder that the observed outcomes in the data
sample may not perfectly reflect the true relationships
between variables since there is \term{random noise}.
While the observed difference in rates of infection
is large, the sample size for the study is small,
making it unclear if this observed difference represents
efficacy of the vaccine or whether it is simply due to
chance.
We label these two competing claims, $H_0$ and $H_A$,
which are spoken as ``H-nought'' and ``H-A'':
\begin{itemize}
\setlength{\itemsep}{0mm}
\item[$H_0$:] \textbf{Independence model.}
    The variables \var{treatment} and \var{outcome}
    are independent.
    They have no relationship, and the observed difference
    between the proportion of patients who developed
    an infection in the two groups, 64.3%,
    was due to chance.
\item[$H_A$:] \textbf{Alternative model.}
    The variables are \emph{not} independent.
    The difference in infection rates of
    64.3%
    was not due to chance,
    and vaccine affected the rate of infection.
\end{itemize}

What would it mean if the independence model,
which says the vaccine had no influence on the
rate of infection, is true?
It would mean 11 patients were going to
develop an infection \emph{no matter which group
they were randomized into},
and 9 patients would not develop an infection
\emph{no matter which group they were randomized
into}.
That is, if the vaccine did not affect the rate
of infection, the difference in the infection rates
was due to chance alone in how the patients were
randomized.

Now consider the alternative model:
infection rates were influenced by whether a patient
received the vaccine or not.
If this was true, and especially if this influence
was substantial, we would expect to see some difference
in the infection rates of patients in the groups.

We choose between these two competing claims
by assessing if the data conflict so much with
$H_0$ that the independence model cannot be deemed
reasonable.
If this is the case, and the data support $H_A$,
then we will reject the notion of independence
and conclude there was discrimination.


\subsection{Simulating the study}
\label{simulatingTheStudy}

We're going to implement
\termsub{simulations}{simulation},
where we will pretend we know that the malaria
vaccine being tested does \emph{not} work.
Ultimately, we want to understand if the large
difference we observed is common in these
simulations.
If it is common, then maybe the difference
we observed was purely due to chance.
If it is very uncommon, then the possibility
that the vaccine was helpful seems more plausible.

Table \@ref(tab:malaria-vaccine-20-exp-summary)
shows that 11 patients developed infections and 9 did not.
For our simulation, we will suppose the infections
were independent of the vaccine and we were able to
\emph{rewind} back to when the researchers randomized
the patients in the study.
If we happened to randomize the patients differently,
we may get a different result in this hypothetical
world where the vaccine doesn't influence the infection.
Let's complete another \term{randomization} using
a simulation.


In this \term{simulation}, we take 20 notecards to
represent the 20 patients, where we write down ``infection''
on 11 cards and ``no infection'' on 9 cards.
In this hypothetical world, we believe each patient
that got an infection was going to get it regardless
of which group they were in, so let's see what happens
if we randomly assign the patients to the treatment
and control groups again.
We thoroughly shuffle the notecards and deal 14 into
a \resp{vaccine} pile and 6 into a \resp{placebo} pile.
Finally, we tabulate the results, which are shown in
Figure \ref{malaria-vaccine-20-exp-summary_rand_1}.

\begin{figure}[ht]
\centering
\begin{tabular}{l l cc rr}
  & & \multicolumn{2}{c}{\var{outcome}} \\
  \cline{3-4}
  &  &  {infection} & {no infection} & Total & \hspace{3mm}  \\ 
  \cline{2-5}
  treatment & {vaccine} & 7 & 7 & 14 \\ 
  (simulated) & {placebo} & 4 & 2 & 6 \\ 
  \cline{2-5}
  & Total & 11 & 9 & 20 \\
  \cline{2-5}
\end{tabular}
\caption{Simulation results, where any difference
    in infection rates is purely due to chance.}
\label{malaria-vaccine-20-exp-summary_rand_1}
\end{figure}

\begin{exercisewrap}
\begin{nexercise}
\label{malaria-vaccine-20-exp-summary_rand_1_diff}
What is the difference in infection rates between
the two simulated groups in
Figure \ref{malaria-vaccine-20-exp-summary_rand_1}?
How does this compare to the observed
64.3% difference
in the actual data?\footnotemark{}
\end{nexercise}
\end{exercisewrap}
\footnotetext{$4 / 6 - 7 / 14 = 0.167$
  or about 16.7\% in favor of the vaccine.
  This difference due to chance is much smaller than the
  difference observed in the actual groups.}
-->

As we saw in Section \@ref(inf-rand), we can randomize the responses (`infection` or `no infection`) to the treatment conditions under the null hypothesis of independence and compute possible differences in proportions.
The process by which we randomize observations to two groups is summarized and visualized in Figure \@ref(fig:fullrand). 

<!--
We computed one possible difference under the
independence model in Guided
Practice \ref{malaria-vaccine-20-exp-summary_rand_1_diff},
which represents one difference due to chance.
While in this first simulation, we physically dealt
out notecards to represent the patients,
it is more efficient to perform this simulation
using a computer.
Repeating the simulation on a computer, we get another
difference due to chance:
\begin{align*}
\frac{2}{\malariaBD{}} - \frac{9}{\malariaAD{}} = -0.310
\end{align*}
And another:
\begin{align*}
\frac{3}{\malariaBD{}} - \frac{8}{\malariaAD{}} = -0.071
\end{align*}
And so on until we repeat the simulation enough times
that we have a good idea of what represents the
\emph{distribution of differences from chance alone}.
-->

#### Variability of the statistic {-}

Figure \@ref(fig:malaria-rand-dot-plot) shows a stacked plot
of the differences found from 100 randomization simulations (i.e., repeated iterations as described in Figure \@ref(fig:fullrand)),
where each dot represents a simulated difference between
the infection rates (control rate minus treatment rate).

<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/malaria-rand-dot-plot-1.png" alt="A stacked dot plot of differences from 100 simulations produced under the independence model $H_0$, where in these simulations infections are unaffected by the vaccine. Two of the 100 simulations had a difference of at least 64.3%, the difference observed in the study." width="70%" />
<p class="caption">(\#fig:malaria-rand-dot-plot)A stacked dot plot of differences from 100 simulations produced under the independence model $H_0$, where in these simulations infections are unaffected by the vaccine. Two of the 100 simulations had a difference of at least 64.3%, the difference observed in the study.</p>
</div>

#### Observed statistic vs null statistics {-}

Note that the distribution of these simulated differences
is centered around 0.
We simulated the differences assuming that the independence
model was true, and under this condition,
we expect the difference to be near zero with some random
fluctuation, where *near* is pretty generous in this
case since the sample sizes are so small in this study.



\BeginKnitrBlock{example}<div class="example">How often would you observe a difference
    of at least 64.3% (0.643)
    according to Figure \@ref(fig:malaria-rand-dot-plot)?
    Often, sometimes, rarely, or never?
      
---
      
  It appears that a difference of at least
  64.3% due to chance alone would only
  happen about 2% of the time according to
  Figure \@ref(fig:malaria-rand-dot-plot).
  Such a low probability indicates a rare event.</div>\EndKnitrBlock{example}

The difference of 64.3% being
a rare event suggests two possible interpretations
of the results of the study:

* $H_0$ Independence model. The vaccine has no effect on infection rate, and we just happened to observe a difference that would only occur on a rare occasion.
* $H_A$ Alternative model. The vaccine has an effect on infection rate, and the difference we observed was actually due to the vaccine being effective at combating malaria, which explains the large difference of 64.3%.

Based on the simulations, we have two options:   

1. We conclude that the study results do not provide
strong evidence against the independence model.
That is, we do not have sufficiently strong evidence
to conclude the vaccine had an effect in this
clinical setting.  

2. We conclude the evidence is sufficiently strong
to reject $H_0$ and assert that the vaccine was useful.
When we conduct formal studies, usually we reject the
notion that we just happened to observe a rare
event.^[This reasoning does not generally extend
    to anecdotal observations.
    Each of us observes incredibly rare events every day,
    events we could not possibly hope to predict.
    However, in the non-rigorous setting of anecdotal
    evidence, almost anything may appear to be a rare event,
    so the idea of looking for rare events in day-to-day
    activities is treacherous.
    For example, we might look at the lottery:
    there was only a 1 in 292 million chance that the
    Powerball numbers for the largest jackpot in history
    (January 13th, 2016) would be (04, 08, 19, 27, 34)
    with a Powerball of (10),
    but nonetheless those numbers came up!
    However, no matter what numbers had turned up,
    they would have had the same incredibly rare odds.
    That is, *any set of numbers we could have
    observed would ultimately be incredibly rare*.
    This type of situation is typical of our daily lives:
    each possible event in itself seems incredibly rare,
    but if we consider every alternative, those outcomes
    are also incredibly rare.
    We should be cautious not to misinterpret such
    anecdotal evidence.]
In this case, we reject the independence model in favor
of the alternative. 
That is, we are concluding the data provide strong evidence
that the vaccine provides some protection against malaria
in this clinical setting.

\index{data!malaria vaccine|)}

Statistical inference, is built
on evaluating whether such differences are due to chance.
In statistical inference, data scientists evaluate which
model is most reasonable given the data.
Errors do occur, just like rare events, and we might choose
the wrong model.
While we do not always choose correctly, statistical
inference gives us tools to control and evaluate how
often these errors occur.


#### Decision errors {-}

\index{hypothesis testing!decision errors|(}

Hypothesis tests are not flawless. Just think of the court system: innocent people are sometimes wrongly convicted and the guilty sometimes walk free. 
Similarly, data can point to the wrong conclusion. 
However, what distinguishes statistical hypothesis tests from a court system is that our framework allows us to quantify and control how often the data lead us to the incorrect conclusion.

In a hypothesis test, there are two competing hypotheses: the null and the alternative. 
We make a statement about which one might be true, but we might choose incorrectly. There are four possible scenarios in a hypothesis test, which are summarized in Table \@ref(tab:fourHTScenarios).


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:fourHTScenarios)Four different scenarios for hypothesis tests.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">**Test conclusion**</div></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;">  </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> $H_0$ true </td>
   <td style="text-align:left;"> good decision </td>
   <td style="text-align:left;"> Type 1 Error </td>
  </tr>
  <tr>
   <td style="text-align:left;"> **Truth** </td>
   <td style="text-align:left;"> $H_A$ true </td>
   <td style="text-align:left;"> Type 2 Error </td>
   <td style="text-align:left;"> good decision </td>
  </tr>
</tbody>
</table>


A **Type 1 Error**\index{Type 1 Error} is rejecting the null hypothesis when $H_0$ is actually true. 
Since we rejected the null hypothesis in the gender discrimination and opportunity cost studies, it is possible that we made a Type 1 Error in one or both of those studies. 
A **Type 2 Error**\index{Type 2 Error} is failing to reject the null hypothesis when the alternative is actually true.




\BeginKnitrBlock{example}<div class="example">In a US court, the defendant is either innocent ($H_0$) or guilty ($H_A$). 
What does a Type 1 Error represent in this context? 
What does a Type 2 Error represent? 
Table \@ref(tab:fourHTScenarios) may be useful.

---
 
If the court makes a Type 1 Error, this means the defendant is innocent ($H_0$ true) but wrongly convicted. 
A Type 2 Error means the court failed to reject $H_0$ (i.e., failed to convict the person) when they were in fact guilty ($H_A$ true).</div>\EndKnitrBlock{example}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Consider the opportunity cost study where we concluded students were less likely to make a DVD purchase if they were reminded that money not spent now could be spent later. What would a Type 1 Error represent in this context?^[Making a Type 1 Error in this context would mean that reminding students that money not spent now can be spent later does not affect their buying habits, despite the strong evidence (the data suggesting otherwise) found in the experiment. Notice that this does *not* necessarily mean something was wrong with the data or that we made a computational mistake. Sometimes data simply point us to the wrong conclusion, which is why scientific studies are often repeated to check initial findings.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{example}<div class="example">How could we reduce the Type 1 Error rate in US courts? 
What influence would this have on the Type 2 Error rate?

---
 
To lower the Type 1 Error rate, we might raise our standard for conviction from "beyond a reasonable doubt" to "beyond a conceivable doubt" so fewer people would be wrongly convicted. However, this would also make it more difficult to convict the people who are actually guilty, so we would make more Type 2 Errors.</div>\EndKnitrBlock{example}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">How could we reduce the Type 2 Error rate in US courts? 
What influence would this have on the Type 1 Error rate?^[To lower the Type 2 Error rate, we want to convict more guilty people. We could lower the standards for conviction from "beyond a reasonable doubt" to "beyond a little doubt". Lowering the bar for guilt will also result in more wrongful convictions, raising the Type 1 Error rate.]</div>\EndKnitrBlock{guidedpractice}


\index{hypothesis testing!decision errors|)}

The example and guided practice above provide an important lesson: if we reduce how often we make one type of error, we generally make more of the other type.

<!--
%Hypothesis testing is built around rejecting or failing to reject the null hypothesis. That is, we do not reject $H_0$ unless the data provide strong evidence against it. But what precisely does *strong evidence* mean? As a general rule of thumb, for those cases where the null hypothesis is actually true, we do not want to incorrectly reject $H_0$ more than 5% of the time. This corresponds to our default significance level of $\alpha = 0.05$, which we use as a comparison with the p-value. In the next section, we discuss the appropriateness of different significance levels.
-->

#### Choosing a significance level {-}

\index{hypothesis testing!significance level|(}
\index{significance level|(}

Choosing a significance level for a test is important in many contexts, and the traditional level is 0.05. 
However, it is sometimes helpful to adjust the significance level based on the application. 
We may select a level that is smaller or larger than 0.05 depending on the consequences of any conclusions reached from the test.

If making a Type 1 Error is dangerous or especially costly, we should choose a small significance level (e.g., 0.01 or 0.001). 
If we want to be very cautious about rejecting the null hypothesis, we demand very strong evidence favoring the alternative $H_A$ before we would reject $H_0$.

If a Type 2 Error is relatively more dangerous or much more costly than a Type 1 Error, then we should choose a higher significance level (e.g., 0.10). 
Here we want to be cautious about failing to reject $H_0$ when the null is actually false.


\BeginKnitrBlock{tipbox}<div class="tipbox">**Significance levels should reflect consequences of errors.**

The significance level selected for a test should reflect the real-world consequences associated with making a Type 1 or Type 2 Error.</div>\EndKnitrBlock{tipbox}

#### Two-sided hypotheses {-}

<!--
%_________________
%\section[Case study: CPR and blood thinner (randomization)]{Case study: blood thinner and CPR\\(randomization)}
-->

\index{hypothesis testing!two tails|(}

In Section \@ref(inf-rand) we explored whether women were discriminated against and whether a simple trick could make students a little thriftier. 
In these two case studies, we've actually ignored some possibilities:

* What if *men* are actually discriminated against?
* What if the money trick actually makes students *spend more*?

These possibilities weren't considered in our original hypotheses or analyses. 
The disregard of the extra alternatives may have seemed natural since the data pointed in the directions in which we framed the problems. However, there are two dangers if we ignore possibilities that disagree with our data or that conflict with our world view:

1. Framing an alternative hypothesis simply to match the direction that the data point will generally inflate the Type 1 Error rate. After all the work we've done (and will continue to do) to rigorously control the error rates in hypothesis tests, careless construction of the alternative hypotheses can disrupt that hard work. 

2. If we only use alternative hypotheses that agree with our worldview, then we're going to be subjecting ourselves to **confirmation bias**\index{confirmation bias}, which means we are looking for data that supports our ideas. That's not very scientific, and we can do better!


The original hypotheses we've seen are called **one-sided hypothesis tests**\index{one-sided hypothesis test} because they only explored one direction of possibilities. 
Such hypotheses are appropriate when we are exclusively interested in the single direction, but usually we want to consider all possibilities. 
To do so, let's learn about **two-sided hypothesis tests**\index{two-sided hypothesis test} in the context of a new study that examines the impact of using blood thinners on patients who have undergone CPR.




\index{data!CPR and blood thinner|(}

Cardiopulmonary resuscitation (CPR) is a procedure used on individuals suffering a heart attack when other emergency resources are unavailable. 
This procedure is helpful in providing some blood circulation to keep a person alive, but CPR chest compressions can also cause internal injuries. Internal bleeding and other injuries that can result from CPR complicate additional treatment efforts. 
For instance, blood thinners may be used to help release a clot that is causing the heart attack once a patient arrives in the hospital. However, blood thinners negatively affect internal injuries.

Here we consider an experiment with patients who underwent CPR for a heart attack and were subsequently admitted to a hospital.^[B$\ddot{\text{o}}$ttiger et al. "Efficacy and safety of thrombolytic therapy after initially unsuccessful cardiopulmonary resuscitation: a prospective clinical trial." The Lancet, 2001.] 
Each patient was randomly assigned to either receive a blood thinner (treatment group) or not receive a blood thinner (control group). 
The outcome variable of interest was whether the patient survived for at least 24 hours.

<!--
% The way p_c and p_t are described make it sound like we are only considering the samples
-->


\BeginKnitrBlock{example}<div class="example">Form hypotheses for this study in plain and statistical language. 
Let $p_c$ represent the true survival rate of people who do not receive a blood thinner (corresponding to the control group) and $p_t$ represent the survival rate for people receiving a blood thinner (corresponding to the treatment group).

---
 
We want to understand whether blood thinners are helpful or harmful. 
We'll consider both of these possibilities using a two-sided hypothesis test.

* $H_0$: Blood thinners do not have an overall survival effect, i.e., the survival proportions are the same in each group. $p_t - p_c = 0$.

* $H_A$: Blood thinners have an impact on survival, either positive or negative, but not zero. $p_t - p_c \neq 0$.

Note that if we had done a one-sided hypothesis test, the resulting hypotheses would have been:

* $H_0$: Blood thinners do not have a positive overall survival effect, i.e., the survival proportions for the blood thinner group is the same or lower than the control group. $p_t - p_c \leq 0$.

* $H_A$: Blood thinners have a positive impact on survival. $p_t - p_c > 0$.
</div>\EndKnitrBlock{example}

There were 50 patients in the experiment who did not receive a blood thinner and 40 patients who did. 
The study results are shown in Table \@ref(tab:resultsForCPRStudyInSmallSampleSection).

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:resultsForCPRStudyInSmallSampleSection)Results for the CPR study. Patients in the treatment group were given a blood thinner, and patients in the control group were not</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> Survived </th>
   <th style="text-align:left;"> Died </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:left;"> 11 </td>
   <td style="text-align:left;"> 39 </td>
   <td style="text-align:left;"> 50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:left;"> 14 </td>
   <td style="text-align:left;"> 26 </td>
   <td style="text-align:left;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 25 </td>
   <td style="text-align:left;"> 65 </td>
   <td style="text-align:left;"> 90 </td>
  </tr>
</tbody>
</table>

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What is the observed survival rate in the control group? 
And in the treatment group? 
Also, provide a point estimate of the difference in survival proportions of the two groups: $\hat{p}_t - \hat{p}_c$.^[Observed control survival rate: $p_c = \frac{11}{50} = 0.22$. 
Treatment survival rate: $p_t = \frac{14}{40} = 0.35$. 
Observed difference: $\hat{p}_t - \hat{p}_c = 0.35 - 0.22 = 0.13$.]</div>\EndKnitrBlock{guidedpractice}

According to the point estimate, for patients who have undergone CPR outside of the hospital, an additional 13% of these patients survive when they are treated with blood thinners. 
However, we wonder if this difference could be easily explainable by chance.

As we did in our past two studies this chapter, we will simulate what type of differences we might see from chance alone under the null hypothesis. 
By randomly assigning "simulated treatment" and "simulated control" stickers to the patients' files, we get a new grouping. 
If we repeat this simulation 10,000 times, we can build a **null distribution**\index{null distribution} of the differences shown in Figure \@ref(fig:CPR-study-right-tail).





<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/CPR-study-right-tail-1.png" alt="Null distribution of the point estimate for the difference in proportions, $\hat{p}_t - \hat{p}_c$. The shaded right tail shows observations that are at least as large as the observed difference, 0.13." width="70%" />
<p class="caption">(\#fig:CPR-study-right-tail)Null distribution of the point estimate for the difference in proportions, $\hat{p}_t - \hat{p}_c$. The shaded right tail shows observations that are at least as large as the observed difference, 0.13.</p>
</div>

The right tail area is 0.131. (Note: it is only a coincidence that we also have $\hat{p}_t - \hat{p}_c=0.13$.) 
However, contrary to how we calculated the p-value in previous studies, the p-value of this test is not 0.131!

The p-value is defined as the chance we observe a result at least as favorable to the alternative hypothesis as the result (i.e., the difference) we observe. 
In this case, any differences less than or equal to -0.13 would also provide equally strong evidence favoring the alternative hypothesis as a difference of +0.13 did. 
A difference of -0.13 would correspond to 13% higher survival rate in the control group than the treatment group. 
In Figure \@ref(fig:CPR-study-p-value) we've also shaded these differences in the left tail of the distribution. 
These two shaded tails provide a visual representation of the p-value for a two-sided test.

<!--
%There is something different in this study than in the past studies: in this study, we are particularly interested in whether blood thinners increase *or* decrease the risk of death in patients who undergo CPR before arriving at the hospital.\footnote{Realistically, we probably are interested in either direction in the past studies as well, and so we should have used the approach we now discuss in this section. However, for simplicity and the sake of not introducing too many concepts at once, we skipped over these details in earlier sections.} For example, there are chance differences of $\hat{p}_t - \hat{p}_c = -0.14$, that would have been stronger evidence against the null hypothesis as our observed difference of +0.13. Likewise, anything less than or equal -0.13 would provide as much evidence against the null hypothesis as +0.13, and for this reason, we must count both tails towards the p-value, as shown in Figure \ref{CPR-study-p-value}.
-->


<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/CPR-study-p-value-1.png" alt="Null distribution of the point estimate for the difference in proportions, $\hat{p}_t - \hat{p}_c$. All values that are at least as extreme as +0.13 but in either direction away from 0 are shaded." width="70%" />
<p class="caption">(\#fig:CPR-study-p-value)Null distribution of the point estimate for the difference in proportions, $\hat{p}_t - \hat{p}_c$. All values that are at least as extreme as +0.13 but in either direction away from 0 are shaded.</p>
</div>


For a two-sided test, take the single tail (in this case, 0.131) and double it to get the p-value: 0.262. 
Since this p-value is larger than 0.05, we do not reject the null hypothesis. 
That is, we do not find statistically significant evidence that the blood thinner has any influence on survival of patients who undergo CPR prior to arriving at the hospital. 

<!--%Once again, we can discuss the causal conclusion since this is an experiment.
-->

\index{data!CPR and blood thinner|)}

\BeginKnitrBlock{onebox}<div class="onebox">**Default to a two-sided test.**

We want to be rigorous and keep an open mind when we analyze data and evidence. 
Use a one-sided hypothesis test only if you truly have interest in only one direction.</div>\EndKnitrBlock{onebox}

\BeginKnitrBlock{onebox}<div class="onebox">**Computing a p-value for a two-sided test.**

First compute the p-value for one tail of the distribution, then double that value to get the two-sided p-value. 
That's it!</div>\EndKnitrBlock{onebox}

\BeginKnitrBlock{example}<div class="example">Consider the situation of the medical consultant. 
Now that you know about one-sided and two-sided tests, which type of test do you think is more appropriate?

---

The setting has been framed in the context of the consultant being helpful (which is what led us to a one-sided test originally), but what if the consultant actually performed *worse* than the average? 
Would we care? 
More than ever! 
Since it turns out that we care about a finding in either direction, we should run a two-sided test. 
The p-value for the two-sided test is double that of the one-sided test, here the simulated p-value would be 0.2444.</div>\EndKnitrBlock{example}


Generally, to find a two-sided p-value we double the single tail area, which remains a reasonable approach even when the sampling distribution is asymmetric.
However, the approach can result in p-values larger than 1 when the point estimate is very near the mean in the null distribution; in such cases, we write that the p-value is 1. 
Also, very large p-values computed in this way (e.g., 0.85), may also be slightly inflated. 
Typically, we do not worry too much about the precision of very large p-values because they lead to the same analysis conclusion, even if the value is slightly off.

#### Controlling the Type 1 Error rate {-}

Now that we understand the difference between one-sided and two-sided tests, we must recognize when to use each type of test.
Because of the result of increased error rates, it is never okay to change two-sided tests to one-sided tests after observing the data. 
We explore the consequences of ignoring this advice in the next example.

\BeginKnitrBlock{example}<div class="example">Using $\alpha=0.05$, we show that freely switching from two-sided tests to one-sided tests will lead us to make twice as many Type 1 Errors as intended.

---
 
Suppose we are interested in finding any difference from 0. 
We've created a smooth-looking **null distribution** representing differences due to chance in Figure \@ref(fig:type1ErrorDoublingExampleFigure).

Suppose the sample difference was larger than 0. 
Then if we can flip to a one-sided test, we would use $H_A$: difference $> 0$. 
Now if we obtain any observation in the upper 5% of the distribution, we would reject $H_0$ since the p-value would just be a the single tail. 
Thus, if the null hypothesis is true, we incorrectly reject the null hypothesis about 5% of the time when the sample mean is above the null value, as shown in Figure \@ref(fig:type1ErrorDoublingExampleFigure).

Suppose the sample difference was smaller than 0. 
Then if we change to a one-sided test, we would use $H_A$: difference $< 0$. 
If the observed difference falls in the lower 5% of the figure, we would reject $H_0$. 
That is, if the null hypothesis is true, then we would observe this situation about 5% of the time.

By examining these two scenarios, we can determine that we will make a Type 1 Error $5\%+5\%=10\%$ of the time if we are allowed to swap to the "best" one-sided test for the data. 
This is twice the error rate we prescribed with our significance level: $\alpha=0.05$ (!).</div>\EndKnitrBlock{example}



<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/type1ErrorDoublingExampleFigure-1.png" alt="The shaded regions represent areas where we would reject $H_0$ under the bad practices considered in when $\alpha = 0.05$." width="70%" />
<p class="caption">(\#fig:type1ErrorDoublingExampleFigure)The shaded regions represent areas where we would reject $H_0$ under the bad practices considered in when $\alpha = 0.05$.</p>
</div>

\BeginKnitrBlock{cautionbox}<div class="cautionbox">**Hypothesis tests should be set up *before* seeing the data.**

After observing data, it is tempting to turn a two-sided test into a one-sided test. 
Avoid this temptation. 
Hypotheses should be set up *before* observing the data.</div>\EndKnitrBlock{cautionbox}

<!--
%\Comment{Should we scrap this subsection and example and just leave the caution box? Downside: weakens item 1 near the start of Section \@ref(IntroducingTwoSidedHypotheses).}
-->

\index{hypothesis testing!two tails|)}



### Bootstrap confidence interval for $p_1 - p_2$ {#two-prop-boot-ci}

The key will be to use two different bags to simulate from the original data.

Use the CPR data.  After we find the CI (use percentile and SE methods), write the interval values down below in the math section that describes the generic confidence interval method. 

In Section \@ref(two-prop-errors), we worked with the randomization distribution to understand the distribution of $\hat{p}_1 - \hat{p}_2$ when the null hypothesis $H_0: p_1 - p_2 = 0$ is true.
Now, through bootstrapping, we study the variability of $\hat{p}_1 - \hat{p}_2$ without the null assumption.

#### Observed data {-}

Reconsider the CPR data from Section \@ref(two-prop-errors) which is provided in Table \@ref(tab:resultsForCPRStudyInSmallSampleSection).   The experiment consisted of two treatments on patients who underwent CPR for a heart attack and were subsequently admitted to a hospital.  Each patient was randomly assigned to either receive a blood thinner (treatment group) or not receive a blood thinner (control group). 
The outcome variable of interest was whether the patient survived for at least 24 hours.

Again, we use the difference in sample proportions as the observed statistic of interest.  Here, the value of the statistic is: $\hat{p}_t - \hat{p}_c = 0.35 - 0.22 = 0.13$.

#### Variability of the statistic {-}

The bootstrap method applied to two samples is an extension of the method described in Section \@ref(boot-ci).  Now, we have two samples, so each sample estimates the population from which they came.  In the CPR setting, the `treatment` sample estimates the population of all individuals who have gotten (or will get) the treatment; the `control` sample estimate the population of all individuals who do not get the treatment and are controls.  Figure \@ref(fig:boot2samp1) extends Figure \@ref(fig:boot1) to show the bootstrapping process from two samples simultaneously.

<div class="figure" style="text-align: center">
<img src="05/figures/boot2samp1.png" alt="(probably populations only, no BS samples) two sample estimating two populations (two infinite popuations)" width="75%" />
<p class="caption">(\#fig:boot2samp1)(probably populations only, no BS samples) two sample estimating two populations (two infinite popuations)</p>
</div>

The variability of the statistic (the difference in sample proportions) can be calculated by taking one treatment bootstrap sample and one control bootstrap sample and calculating the difference of the bootstrap survival proportions.  One sample from each of the estimated populations has been taken with the sample proportions calculated for the treatment bootstrap sample and the control bootstrap sample.  

\BeginKnitrBlock{todo}<div class="todo">once the image is in, we need to describe above (and below) the values (proportions, differences) in the image explicitly.</div>\EndKnitrBlock{todo}


<div class="figure" style="text-align: center">
<img src="05/figures/boot2samp2.png" alt="some way to connect the first BS sample on the left wiht the first BS sample on the right." width="75%" />
<p class="caption">(\#fig:boot2samp2)some way to connect the first BS sample on the left wiht the first BS sample on the right.</p>
</div>


As always, the variability of the difference in proportions can only be estimated by repeated simulations, in this case, repeated bootstrap samples.  Figure \@ref(fig:boot2samp2) shows multiple bootstrap differences calculated for each of the repeated bootstrap samples.

<div class="figure" style="text-align: center">
<img src="05/figures/boot2samp3.png" alt="in this graph, some kind of connection between each of the two sides" width="75%" />
<p class="caption">(\#fig:boot2samp3)in this graph, some kind of connection between each of the two sides</p>
</div>

\BeginKnitrBlock{todo}<div class="todo">Do we also want to visualize "sampling with replacement" in the two sample case?</div>\EndKnitrBlock{todo}

Repeated bootstrap simulations lead to a bootstrap sampling distribution of the statistic of interest, here the difference in sample proportions.
Figure \@ref(fig:bootCPR1000) shows 1000 bootstrap differences in proportions for the CPR data.

<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/bootCPR1000-1.png" alt="A histogram of differences in proportions from 1000 bootstrap simulations." width="70%" />
<p class="caption">(\#fig:bootCPR1000)A histogram of differences in proportions from 1000 bootstrap simulations.</p>
</div>


#### Percentile vs. SE bootstrap confidence intervals {-}

Figure \@ref(fig:bootCPR1000) provides an estimate for the variability of the difference in survival proportions from sample to sample,  The values in the histogram can be used in two different ways to create a confidence interval for the parameter of interest:  $p_1 - p_2$.

##### Percentile bootstrap interval {-}

As in Section \@ref(boot-ci), the bootstrap confidence interval can be calculated directly from the bootstrapped differences in Figure \@ref(fig:bootCPR1000).  The interval created from the percentiles of the distribution is called the **percentile interval**.
Note that here we calculate the 90% confidence interval by finding the $5^{th}$ and $95^{th}$ percentile values from the bootstrapped differences.
The bootstrap 5 percentile proportion is -0.155 and the 95 percentile is 0.167.
The result is: we are 90% confident that, in the population, the true difference in probability of survival is between -0.155 and 0.167. 
The interval shows that we do not have much definitive evidence of the affect of blood thinners, one way or another.



<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/CPR percentile interval-1.png" alt="The CPR data is bootstrapped 1000 times. Each simulation creates a sample from the original data where the probability of survival in the treatment group is $\hat{p}_{t}  = 14/40$ and the probability of survival in the control group is $\hat{p}_{c} = 11/50$. " width="70%" />
<p class="caption">(\#fig:CPR percentile interval)The CPR data is bootstrapped 1000 times. Each simulation creates a sample from the original data where the probability of survival in the treatment group is $\hat{p}_{t}  = 14/40$ and the probability of survival in the control group is $\hat{p}_{c} = 11/50$. </p>
</div>



##### SE bootstrap interval {-}

Alternatively, we can use the variability in the bootstrapped differences to calculate a standard error of the difference.
The resulting interval is called the **SE interval**.
Section \@ref(math-2prop) details the mathematical model for the standard error of the difference in sample proportions, but the bootstrap distribution typically does an excellent job of estimating the variability.



$$SE(\hat{p}_t - \hat{p}_c) \approx SD(\hat{p}_{bs,t} - \hat{p}_{bs,c}) = 0.0975$$

The calculation above was performed in R using the `sd()` function, but any statistical software will calculate the standard deviation of the differences, here, the exact quantity we hope to approximate.

Because we don't know the true distribution of $\hat{p}_t - \hat{p}_c$, we will use a rough approximation to find a confidence interval for $p_t - p_c$.  A 95% confidence interval for $p_t - p_c$ is given by:

\begin{align}
\hat{p}_t - \hat{p}_c &\pm& 2 SE(\hat{p}_t - \hat{p}_c)\\
14/40 - 11/50 &\pm& 0.0975\\
&&(-0.065, 0.325)
\end{align}

We are 95% confident that the true value of $p_t - p_c$ is between -0.065 and 0.325.  Again, the wide confidence interval that overlaps zero indicates that the study provides very little evidence about the effectiveness of blood thinners.

### Mathematical model {#math-2prop}

#### Variability of $\hat{p}_1 - \hat{p}_2$ {-}

Like with $\hat{p}$, the difference of two sample
proportions $\hat{p}_1 - \hat{p}_2$ can be modeled
using a normal distribution when certain conditions
are met.
First, we require a broader independence condition,
and secondly,
the success-failure condition must be met by both groups.

\BeginKnitrBlock{onebox}<div class="onebox">**Conditions for the sampling distribution of $\hat{p}_1 -\hat{p}_2$ to be normal.**
  
The difference $\hat{p}_1 - \hat{p}_2$ can be modeled
  using a normal distribution when

1. *Independence*, extended.  The data are independent within and between
    the two groups. Generally this is satisfied if the data come from two independent random samples or if the data come from a randomized experiment.
2. *Success-failure condition.*
    The success-failure condition holds for both
    groups, where we check successes and failures
    in each group separately.

    
  When these conditions are satisfied,
  the standard error of $\hat{p}_1 - \hat{p}_2$ is

  \begin{eqnarray*}
  SE = \sqrt{\frac{p_1(1-p_1)}{n_1} + \frac{p_2(1-p_2)}{n_2}}
  \end{eqnarray*}
  where $p_1$ and $p_2$ represent the population proportions,
  and $n_1$ and $n_2$ represent the sample sizes.</div>\EndKnitrBlock{onebox}

\index{standard error (SE)!difference in proportions}

<!--
SE reference above?
    \label{seForDiffOfProp}
-->






#### Confidence interval for $p_1 - p_2$ {-}

<!--
{Confidence intervals for $\pmb{p_1 - p_2}$}
-->

\index{data!CPR and blood thinner|(}

We can apply the generic confidence interval formula
for a difference of two proportions,
where we use $\hat{p}_1 - \hat{p}_2$ as the point
estimate and substitute the $SE$ formula:
\begin{align*}
&\text{point estimate} \ \pm\  z^{\star} \times SE
&&\to
&&\hat{p}_1 - \hat{p}_2 \ \pm\ 
    z^{\star} \times
   \sqrt{\frac{p_1(1-p_1)}{n_1} + \frac{p_2(1-p_2)}{n_2}}
\end{align*}

<!--
We can also follow the same
Prepare, Check, Calculate, Conclude steps for
computing a confidence interval
or completing a hypothesis test.
The details change a little,
but the general approach remain the same.
Think about these steps when you apply statistical methods.
-->

\BeginKnitrBlock{onebox}<div class="onebox">**Standard Error of the difference in two proportions, $\hat{p}_1 -\hat{p}_2$.**
  
When the conditions are met so that the distribution fo $\hat{p}$ is nearly normal, the **variability** of the difference in proportions, $\hat{p}_1 -\hat{p}_2$, is well described by:
\begin{eqnarray*}
  SE(\hat{p}_1 -\hat{p}_2) = \sqrt{\frac{p_1(1-p_1)}{n_1} + \frac{p_2(1-p_2)}{n_2}}
  \end{eqnarray*}</div>\EndKnitrBlock{onebox}

<!--
\BeginKnitrBlock{onebox}<div class="onebox">**Confidence interval for a difference of two proportions**
  Once you've determined a confidence interval for the
  difference of two proportions would be helpful for an
  application, there are four steps to constructing the interval:

* **Prepare.**
      Identify the sample proportions and sample sizes
      for each of the two groups,
      determine what confidence level you wish to use.
* **Check.**
      Verify the conditions to ensure each sample
      proportion is nearly normal.
      The success-failure condition should be checked
      for each group.
* **Calculate.**
      If the conditions hold, compute $SE$,
      find $z^{\star}$, and construct the interval.
* **Conclude.**
      Interpret the confidence interval in the context
      of the problem.</div>\EndKnitrBlock{onebox}
-->



\BeginKnitrBlock{example}<div class="example">We reconsider the experiment for patients
    who underwent cardiopulmonary resuscitation (CPR)
    for a heart attack and were
    subsequently admitted to a
    hospital.
    These patients were randomly divided into a treatment
    group where they received a blood thinner or the control
    group where they did not receive a blood thinner.
    The outcome variable of interest was whether the
    patients survived for at least 24 hours.
    The results are shown in
    Table \@ref(tab:resultsForCPRStudyInSmallSampleSection).
    Check whether we can model the difference in
    sample proportions using the normal distribution.

---
      
  We first check for independence:
  since this is a randomized experiment,
  this condition is satisfied.
  
  Next, we check the success-failure condition for
  each group.
  We have at least 10 successes and 10 failures in
  each experiment arm (11, 14, 39, 26),
  so this condition is also satisfied.

  With both conditions satisfied,
  the difference in sample proportions can be
  reasonably modeled using a normal distribution
  for these data.</div>\EndKnitrBlock{example}

<!--
<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:resultsForCPRStudyInSmallSampleSectionDup)Results for the CPR study.
    Patients in the treatment group were given
    a blood thinner, and patients in the control
    group were not.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> Survived </th>
   <th style="text-align:left;"> Died </th>
   <th style="text-align:left;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:left;"> 11 </td>
   <td style="text-align:left;"> 39 </td>
   <td style="text-align:left;"> 50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Treatment </td>
   <td style="text-align:left;"> 14 </td>
   <td style="text-align:left;"> 26 </td>
   <td style="text-align:left;"> 40 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Total </td>
   <td style="text-align:left;"> 25 </td>
   <td style="text-align:left;"> 65 </td>
   <td style="text-align:left;"> 90 </td>
  </tr>
</tbody>
</table>
-->

\BeginKnitrBlock{example}<div class="example">Create and interpret a 90% confidence interval of the
    difference for the survival rates in the CPR study.

---
      
We'll use $p_t$ for the survival
  rate in the treatment group and $p_c$ for the control
  group:
  \begin{align*}
  \hat{p}_{t} - \hat{p}_{c}
    = \frac{14}{40} - \frac{11}{50}
    = 0.35 - 0.22
    = 0.13
  \end{align*}
  We use the standard error formula previously provided.
  As with the one-sample proportion case,
  we use the sample estimates of each proportion
  in the formula in the confidence interval context:
  \begin{align*}
  SE \approx \sqrt{\frac{0.35 (1 - 0.35)}{40} +
      \frac{0.22 (1 - 0.22)}{50}}
    = 0.095
  \end{align*}
  For a 90% confidence interval, we use $z^{\star} = 1.65$:
  \begin{align*}
  \text{point estimate} \ \pm\ z^{\star} \times SE
    \quad \to \quad 0.13 \ \pm\ 1.65 \times  0.095
    \quad \to \quad (-0.027, 0.287)
  \end{align*}
  We are 90% confident that blood thinners have
  a difference of -2.7% to +28.7% percentage point
  impact on survival rate for patients who are like
  those in the study.
  Because 0% is contained in the interval,
  we do not have enough information to say
  whether blood thinners help or harm
  heart attack patients who have been admitted after
  they have undergone CPR.</div>\EndKnitrBlock{example}

\index{data!CPR and blood thinner|)}



\BeginKnitrBlock{todo}<div class="todo">not sure why the footnote in the guidedpractice below doesn't show up?

should be right after: "Also interpret the interval in the context of the study."</div>\EndKnitrBlock{todo}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">A 5-year experiment
was conducted to evaluate the effectiveness
of fish oils on reducing cardiovascular events,
where each subject was randomized into one of two
treatment groups.
We'll consider heart attack outcomes in the patients listed in Table \@ref(tab:fish-oil-data).

Create a 95% confidence interval for the effect of fish oils
on heart attacks for patients who are well-represented by
those in the study.
Also interpret the interval in the context of the
study.^[Because the patients were randomized, the subjects are independent, both within and between the two groups.
The success-failure condition is also met for both groups as all counts are at least 10.
This satisfies the conditions necessary to model the difference in proportions using a normal distribution.

Compute the sample proportions ($\hat{p}_{\text{fish oil}} = 0.0112$, $\hat{p}_{\text{placebo}} = 0.0155$), point estimate of the difference ($0.0112 - 0.0155 = -0.0043$), and standard error $SE = \sqrt{\frac{0.0112 \times 0.9888}{12933} + \frac{0.0155 \times 0.9845}{12938}} = 0.00145$. 

Next, plug the values into the general formula for a confidence interval, where we'll use a 95% confidence level with $z^{\star} = 1.96$:
\begin{align*}
  -0.0043 \pm 1.96 \times 0.00145
      \quad \to \quad
      (-0.0071, -0.0015)
\end{align*}  
We are 95% confident that fish oils decreases
  heart attacks by
  0.15 to 0.71 percentage points
  (off of a baseline of about 1.55%)
  over a 5-year period for subjects who are similar
  to those in the study.
  Because the interval is entirely below 0, and the treatment was randomly assigned
  the data provide strong evidence
  that fish oil supplements reduce heart attacks
  in patients like those in the study.]</div>\EndKnitrBlock{guidedpractice}

<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:fish-oil-data)Results for the study on n-3 fatty acid supplement and related health benefits.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:right;"> heart attack </th>
   <th style="text-align:right;"> no event </th>
   <th style="text-align:right;"> Total </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> fish oil </td>
   <td style="text-align:right;"> 145 </td>
   <td style="text-align:right;"> 12788 </td>
   <td style="text-align:right;"> 12933 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> placebo </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 12738 </td>
   <td style="text-align:right;"> 12938 </td>
  </tr>
</tbody>
</table>

#### Hypothesis test for $H_0: p_1 - p_2 = 0$ {-}

\index{data!mammography|(}
\index{data!breast cancer|(}

A mammogram is an X-ray procedure used to check for
breast cancer.
Whether mammograms should be used is part of a
controversial discussion, and it's the topic of our
next example where we learn about 2-proportion
hypothesis tests when $H_0$ is $p_1 - p_2 = 0$
(or equivalently, $p_1 = p_2$).

A 30-year study was conducted with nearly 90,000 female participants. During a 5-year screening period, each woman was randomized to one of two groups: in the first group, women received regular mammograms to screen for breast cancer, and in the second group, women received regular non-mammogram breast cancer exams. No intervention was made during the following 25 years of the study, and we'll consider death resulting from breast cancer over the full 30-year period. Results from the study are summarized in Figure \@ref(tab:mammogramStudySummaryTable).

If mammograms are much more effective than non-mammogram breast cancer exams, then we would expect to see additional deaths from breast cancer in the control group. On the other hand, if mammograms are not as effective as regular breast cancer exams, we would expect to see an increase in breast cancer deaths in the mammogram group.


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:mammogramStudySummaryTable)Summary results for breast cancer study.</caption>
 <thead>
<tr>
<th style="border-bottom:hidden" colspan="1"></th>
<th style="border-bottom:hidden; padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Death from breast cancer?</div></th>
</tr>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> Yes </th>
   <th style="text-align:left;"> No </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Mammogram </td>
   <td style="text-align:left;"> 500 </td>
   <td style="text-align:left;"> 44,425 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Control </td>
   <td style="text-align:left;"> 505 </td>
   <td style="text-align:left;"> 44,405 </td>
  </tr>
</tbody>
</table>


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Is this study an experiment or an observational study?^[This is an experiment. Patients were randomized
    to receive mammograms or a standard breast cancer exam.
    We will be able to make causal conclusions based on this study.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Set up hypotheses to test whether there was a difference
in breast cancer deaths in the mammogram and control groups.^[$H_0$: the breast cancer death rate for patients
    screened using mammograms is the same as the breast cancer
    death rate for patients in the control,
    $p_{mgm} - p_{ctrl} = 0$.  
    $H_A$: the breast cancer death rate for patients screened
    using mammograms is different than the breast cancer death
    rate for patients in the control,
    $p_{mgm} - p_{ctrl} \neq 0$.]</div>\EndKnitrBlock{guidedpractice}

Using the previous example,
we will check the conditions for using a normal distribution to
analyze the results of the study.
The details are very similar to that of confidence intervals.
However, when the null hypothesis is that $p_1 - p_2 = 0$,
we use a special proportion called the
**pooled proportion**\index{pooled proportion} to check the success-failure condition:
\begin{align*}
\hat{p}_{\textit{pool}}
    &= \frac
        {\text{# of patients who died from breast cancer in the entire study}}
        {\text{# of patients in the entire study}} \\
	&= \frac{500 + 505}{500 + \text{44,425} + 505 + \text{44,405}} \\
	&= 0.0112
\end{align*}
This proportion is an estimate of the breast cancer death rate
across the entire study, and it's our best estimate of the
proportions $p_{mgm}$ and $p_{ctrl}$
*if the null hypothesis is true that $p_{mgm} = p_{ctrl}$*.
We will also use this pooled proportion when computing
the standard error.



\BeginKnitrBlock{example}<div class="example">Is it reasonable to model the difference
    in proportions using a normal distribution in this
    study?
  
---
      
  Because the patients are randomized, they can be treated
  as independent, both within and between groups.
  We also must check the success-failure condition for each group.
  Under the null hypothesis, the proportions $p_{mgm}$
  and $p_{ctrl}$ are equal, so we check the success-failure
  condition with our best estimate of these values under $H_0$,
  the pooled proportion from the two samples,
  $\hat{p}_{\textit{pool}} = 0.0112$:
  \begin{align*}
  \hat{p}_{\textit{pool}} \times n_{mgm}
      &= 0.0112 \times \text{44,925} = 503
    & (1 - \hat{p}_{\textit{pool}}) \times n_{mgm}
      &= 0.9888 \times \text{44,925} = \text{44,422} \\
  \hat{p}_{\textit{pool}} \times n_{ctrl}
      &= 0.0112 \times \text{44,910} = 503
    & (1 - \hat{p}_{\textit{pool}}) \times n_{ctrl}
      &= 0.9888 \times \text{44,910} = \text{44,407}
  \end{align*}
  The success-failure condition is satisfied since
  all values are at least 10.
  With both conditions satisfied, we can safely model
  the difference in proportions using a normal
  distribution.</div>\EndKnitrBlock{example}



\BeginKnitrBlock{onebox}<div class="onebox">**Use the pooled proportion when $H_0$ is $p_1 - p_2 = 0$.**
  
  When the null hypothesis is that the proportions are equal,
  use the pooled proportion ($\hat{p}_{\textit{pooled}}$)
  to verify the
  success-failure condition and estimate the standard error:
  \begin{eqnarray*}
  \hat{p}_{\textit{pooled}}
    = \frac{\text{number of ``successes"}}
      {\text{number of cases}}
    = \frac{\hat{p}_1 n_1 + \hat{p}_2 n_2}{n_1 + n_2}
  \end{eqnarray*}
  Here $\hat{p}_1 n_1$ represents the number of successes in
  sample 1 since
  \begin{eqnarray*}
  \hat{p}_1
    = \frac{\text{number of successes in sample 1}}{n_1}
  \end{eqnarray*}
  Similarly, $\hat{p}_2 n_2$ represents the number
  of successes in sample 2.</div>\EndKnitrBlock{onebox}

In the previous example,
the pooled proportion was used to check the success-failure
condition^[For an example of a two-proportion
  hypothesis test that does not require the
  success-failure condition to be met, see
  Section \@ref(two-prop-errors).].  In the next example, we see an additional place where the pooled
proportion comes into play: the standard error calculation.


\BeginKnitrBlock{example}<div class="example">Compute the point estimate of the difference
    in breast cancer death rates in the two groups,
    and use the pooled proportion
    $\hat{p}_{\textit{pool}} = 0.0112$ to calculate
    the standard error.
    
---
      
  The point estimate of the difference in breast cancer death
  rates is
  \begin{align*}
  \hat{p}_{mgm} - \hat{p}_{ctrl}
    &= \frac{500}{500 + 44,425} - \frac{505}{505 + 44,405} \\
    &= 0.01113 - 0.01125 \\
    &= -0.00012
  \end{align*}
  The breast cancer death rate in the mammogram group
  was 0.012% less than in the control group.
  Next, the standard error is calculated
  \emph{using the pooled proportion}, $\hat{p}_{\textit{pool}}$:
\begin{align*}
SE = \sqrt{
      \frac{\hat{p}_{\textit{pool}}(1-\hat{p}_{\textit{pool}})}
          {n_{mgm}}
      + \frac{\hat{p}_{\textit{pool}}(1-\hat{p}_{\textit{pool}})}
          {n_{ctrl}}
    }
	= 0.00070
\end{align*}</div>\EndKnitrBlock{example}



\BeginKnitrBlock{example}<div class="example">Using the point estimate $\hat{p}_{mgm} - \hat{p}_{ctrl} = -0.00012$ and standard error $SE = 0.00070$, calculate a p-value for the hypothesis test and write a conclusion.

---
  
Just like in past tests, we first compute a test statistic and draw a picture:
\begin{align*}
Z = \frac{\text{point estimate} - \text{null value}}{SE}
	= \frac{-0.00012 - 0}{0.00070}
	= -0.17
\end{align*}

The lower tail area is 0.4325, which we double to get the p-value: 0.8650. Because this p-value is larger than 0.05, we do not reject the null hypothesis. That is, the difference in breast cancer death rates is reasonably explained by chance, and we do not observe benefits or harm from mammograms relative to a regular breast exam.</div>\EndKnitrBlock{example}

<img src="05-inference-cat_files/figure-html/unnamed-chunk-89-1.png" width="70%" style="display: block; margin: auto;" />

Can we conclude that mammograms have no benefits or harm?
Here are a few considerations to keep in mind when reviewing
the mammogram study as well as any other medical study:


* We do not accept the null hypothesis, which means
    we don't have sufficient evidence to conclude that
    mammograms reduce or increase breast cancer deaths.
* If mammograms are helpful or harmful, the data
    suggest the effect isn't very large.
* Are mammograms more or less expensive than
    a non-mammogram breast exam?
    If one option is much more expensive than the
    other and doesn't offer clear benefits,
    then we should lean towards the less expensive
    option.
* The study's authors also found that mammograms
    led to over-diagnosis of breast cancer,
    which means some breast cancers were found
    (or thought to be found) but that these cancers
    would not cause symptoms during patients' lifetimes.
    That is, something else would kill the patient
    before breast cancer symptoms appeared.
    This means some patients may have been treated
    for breast cancer unnecessarily, and this
    treatment is another cost to consider.
    It is also important to recognize that
    over-diagnosis can cause unnecessary physical
    or emotional harm to patients.

These considerations highlight the complexity around medical care and treatment recommendations. Experts and medical boards who study medical treatments use considerations like those above to provide their best recommendation based on the current evidence.

\index{data!breast cancer|)}
\index{data!mammography|)}


## More insights into the normal distribution

\index{normal distribution|(}


Among all the distributions we see in statistics, one is overwhelmingly the most common. 
The symmetric, unimodal, bell curve is ubiquitous throughout statistics. 
It is so common that people know it as a variety of names including the **normal curve**\index{normal curve}, **normal model**\index{normal model}, or **normal distribution**\index{normal distribution}.^[It is also introduced as the Gaussian distribution after Frederic Gauss, the first person to formalize its mathematical expression.] 
Under certain conditions, sample proportions, sample means, and sample differences can be modeled using the normal distribution. 
Additionally, some variables such as SAT scores and heights of US adult males closely follow the normal distribution.





\BeginKnitrBlock{onebox}<div class="onebox">**Normal distribution facts.**

Many summary statistics and variables are nearly normal, but none are exactly normal. 
Thus the normal distribution, while not perfect for any single problem, is very useful for a variety of problems. 
We will use it in data exploration and to solve important problems in statistics.</div>\EndKnitrBlock{onebox}

In this section, we will discuss the normal distribution in the context of data to become more familiar with normal distribution techniques. 

#### Normal distribution model {-}


The normal distribution always describes a symmetric, unimodal, bell-shaped curve. 
However, normal curves can look different depending on the details of the model. 
Specifically, the normal model can be adjusted using two parameters: mean and standard deviation. 
As you can probably guess, changing the mean shifts the bell curve to the left or right, while changing the standard deviation stretches or constricts the curve. 
Figure \@ref(fig:twoSampleNormals) shows the normal distribution with mean $0$ and standard deviation $1$  (which is commonly referred to as the **standard normal distribution**\index{standard normal distribution}) on top.
A normal distribution with mean $19$ and standard deviation $4$ is shown on the bottom. Figure \@ref(fig:twoSampleNormalsStacked) shows the same two normal distributions on the same axis.



<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/twoSampleNormals-1.png" alt="Both curves represent the normal distribution, however, they differ in their center and spread. The normal distribution with mean 0 and standard deviation 1 is called the **standard normal distribution**." width="70%" /><img src="05-inference-cat_files/figure-html/twoSampleNormals-2.png" alt="Both curves represent the normal distribution, however, they differ in their center and spread. The normal distribution with mean 0 and standard deviation 1 is called the **standard normal distribution**." width="70%" />
<p class="caption">(\#fig:twoSampleNormals)Both curves represent the normal distribution, however, they differ in their center and spread. The normal distribution with mean 0 and standard deviation 1 is called the **standard normal distribution**.</p>
</div>






<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/twoSampleNormalsStacked-1.png" alt="The two normal models shown above and now plotted together on the same scale." width="70%" />
<p class="caption">(\#fig:twoSampleNormalsStacked)The two normal models shown above and now plotted together on the same scale.</p>
</div>

If a normal distribution has mean $\mu$ and standard deviation $\sigma$, we may write the distribution as $N(\mu, \sigma)$\marginpar[\raggedright\vspace{-5mm}

$N(\mu, \sigma)$\vspace{1mm}\\\footnotesize Normal dist.\\with mean $\mu$\\\& st. dev. $\sigma$]{\raggedright\vspace{-5mm}

$N(\mu, \sigma)$\vspace{1mm}\\\footnotesize Normal dist.\\with mean $\mu$\\\& st. dev. $\sigma$}. The two distributions in Figure \@ref(fig:twoSampleNormalsStacked) can be written as
\begin{align*}
N(\mu=0,\sigma=1)\quad\text{and}\quad N(\mu=19,\sigma=4)
\end{align*}
Because the mean and standard deviation describe a normal distribution exactly, they are called the distribution's **parameters**\index{parameter}.



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Write down the short-hand for a normal distribution with (a) mean 5 and standard deviation 3, (b) mean -100 and standard deviation 10, and (c) mean 2 and standard deviation 9.^[(a) $N(\mu=5,\sigma=3)$. (b) $N(\mu=-100, \sigma=10)$. (c) $N(\mu=2, \sigma=9)$.]</div>\EndKnitrBlock{guidedpractice}


#### Standardizing with Z scores {-}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Table \@ref(tab:satACTstats) shows the mean and standard deviation for total scores on the SAT and ACT. The distribution of SAT and ACT scores are both nearly normal. Suppose Ann scored 1800 on her SAT and Tom scored 24 on his ACT. Who performed better?^[We use the standard deviation as a guide. Ann is 1 standard deviation above average on the SAT: $1500 + 300=1800$. Tom is 0.6 standard deviations above the mean on the ACT: $21+0.6\times 5=24$. In Figure \@ref(fig:satActNormals), we can see that Ann tends to do better with respect to everyone else than Tom did, so her score was better.]</div>\EndKnitrBlock{guidedpractice}


<table class="table" style="margin-left: auto; margin-right: auto;">
<caption>(\#tab:satACTstats)Mean and standard deviation for the SAT and ACT.</caption>
 <thead>
  <tr>
   <th style="text-align:left;">  </th>
   <th style="text-align:left;"> SAT </th>
   <th style="text-align:left;"> ACT </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Mean </td>
   <td style="text-align:left;"> 1500 </td>
   <td style="text-align:left;"> 21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> SD </td>
   <td style="text-align:left;"> 300 </td>
   <td style="text-align:left;"> 5 </td>
  </tr>
</tbody>
</table>

<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/satActNormals-1.png" alt="Ann's and Tom's scores shown with the distributions of SAT and ACT scores." width="70%" />
<p class="caption">(\#fig:satActNormals)Ann's and Tom's scores shown with the distributions of SAT and ACT scores.</p>
</div>


The solution to the previous example relies on a standardization technique called a Z score, a method most commonly employed for nearly normal observations (but that may be used with any distribution). The **Z score**\index{Z score}\marginpar[\raggedright\vspace{-3mm}


$Z$\vspace{1mm}\\\footnotesize Z score, the\\standardized\\observation]{\raggedright\vspace{-3mm}

$Z$\vspace{1mm}\\\footnotesize Z score, the\\standardized\\observation}\index{Z@$Z$} of an observation is defined as the number of standard deviations it falls above or below the mean. 
If the observation is one standard deviation above the mean, its Z score is 1. If it is 1.5 standard deviations *below* the mean, then its Z score is -1.5. 
If $x$ is an observation from a distribution $N(\mu, \sigma)$, we define the Z score mathematically as



\begin{eqnarray*}
Z = \frac{x-\mu}{\sigma}
\end{eqnarray*}
Using $\mu_{SAT}=1500$, $\sigma_{SAT}=300$, and $x_{Ann}=1800$, we find Ann's Z score:
\begin{eqnarray*}
Z_{Ann} = \frac{x_{Ann} - \mu_{SAT}}{\sigma_{SAT}} = \frac{1800-1500}{300} = 1
\end{eqnarray*}


\BeginKnitrBlock{onebox}<div class="onebox">**The Z score.**

The Z score of an observation is the number of standard deviations it falls above or below the mean. 
We compute the Z score for an observation $x$ that follows a distribution with mean $\mu$ and standard deviation $\sigma$ using
\begin{eqnarray*}
Z = \frac{x-\mu}{\sigma}
\end{eqnarray*}</div>\EndKnitrBlock{onebox}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Use Tom's ACT score, 24, along with the ACT mean and standard deviation to compute his Z score.^[$Z_{Tom} = \frac{x_{Tom} - \mu_{ACT}}{\sigma_{ACT}} = \frac{24 - 21}{5} = 0.6$]</div>\EndKnitrBlock{guidedpractice}

Observations above the mean always have positive Z scores while those below the mean have negative Z scores. 
If an observation is equal to the mean (e.g., SAT score of 1500), then the Z score is $0$.


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Let $X$ represent a random variable from $N(\mu=3, \sigma=2)$, and suppose we observe $x=5.19$. (a) Find the Z score of $x$. (b) Use the Z score to determine how many standard deviations above or below the mean $x$ falls.^[(a) Its Z score is given by $Z = \frac{x-\mu}{\sigma} = \frac{5.19 - 3}{2} = 2.19/2 = 1.095$. (b) The observation $x$ is 1.095 standard deviations *above* the mean. We know it must be above the mean since $Z$ is positive.]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Head lengths of brushtail possums follow a nearly normal distribution with mean 92.6 mm and standard deviation 3.6 mm. Compute the Z scores for possums with head lengths of 95.4 mm and 85.8 mm.^[For $x_1=95.4$ mm: $Z_1 = \frac{x_1 - \mu}{\sigma} = \frac{95.4 - 92.6}{3.6} = 0.78$. For $x_2=85.8$ mm: $Z_2 = \frac{85.8 - 92.6}{3.6} = -1.89$.]</div>\EndKnitrBlock{guidedpractice}

We can use Z scores to roughly identify which observations are more unusual than others. 
One observation $x_1$ is said to be more unusual than another observation $x_2$ if the absolute value of its Z score is larger than the absolute value of the other observation's Z score: $|Z_1| > |Z_2|$. 
This technique is especially insightful when a distribution is symmetric.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Which of the two brushtail possum observations in the previous guided practice is more *unusual*?^[Because the *absolute value* of Z score for the second observation is larger than that of the first, the second observation has a more unusual head length.]</div>\EndKnitrBlock{guidedpractice}


#### Normal probability calculations in R {-}

\BeginKnitrBlock{example}<div class="example">Ann from the SAT Guided Practice earned a score of 1800 on her SAT with a corresponding $Z=1$. She would like to know what percentile she falls in among all SAT test-takers.

---
 
Ann's **percentile**\index{percentile} is the percentage of people who earned a lower SAT score than Ann. We shade the area representing those individuals in Figure \@ref(fig:satBelow1800). The total area under the normal curve is always equal to 1, and the proportion of people who scored below Ann on the SAT is equal to the *area* shaded in Figure \@ref(fig:satBelow1800): 0.8413. In other words, Ann is in the $84^{th}$ percentile of SAT takers.</div>\EndKnitrBlock{example}




<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/satBelow1800-1.png" alt="The normal model for SAT scores, shading the area of those individuals who scored below Ann." width="70%" />
<p class="caption">(\#fig:satBelow1800)The normal model for SAT scores, shading the area of those individuals who scored below Ann.</p>
</div>


We can use the normal model to find percentiles or probabilities. In `R`, the function
to calculate normal probabilities is `pnorm`. The `normTail` function is available in the `openintro` R package and will draw the associated curve if it is helpful. In the code below, we find the percentile of $Z=0.43$ is 0.6664, or the $66.64^{th}$ percentile. 


```r
pnorm(0.43, m = 0, s = 1)
#> [1] 0.666
openintro::normTail(0.43, m = 0, s = 1)
```

<img src="05-inference-cat_files/figure-html/unnamed-chunk-104-1.png" width="70%" style="display: block; margin: auto;" />

We can also find the Z score associated with a percentile. 
For example, to identify Z for the $80^{th}$ percentile, we use `qnorm` which identifies the **quantile** for a given percentage.  The quantile represents the cutoff value.  (To remember the function `qnorm` as providing a cutoff, notice that both `qnorm` and "cutoff" start with the sound "kuh".  To remember the `pnorm` function as providing a probability from a given cutoff, notice that both `pnorm` and probability start with the sound "puh".) 
We determine the Z score for the $80^{th}$ percentile using `qnorm`: 0.84.


```r
qnorm(0.80, m = 0, s = 1)
#> [1] 0.842
openintro::normTail(0.80, m = 0, s = 1)
```

<img src="05-inference-cat_files/figure-html/unnamed-chunk-105-1.png" width="70%" style="display: block; margin: auto;" />

We can use these functions with other normal distributions than the standard normal distribution by specifying the mean as the argument for `m` and the standard deviation as the argument for `s`. Here we determine the proportion of ACT test takers who scored worse than Tom on the ACT: 0.73.


```r
pnorm(24, m = 21, s = 5)
#> [1] 0.726
openintro::normTail(24, m = 21, s = 5)
```

<img src="05-inference-cat_files/figure-html/unnamed-chunk-106-1.png" width="70%" style="display: block; margin: auto;" />

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Determine the proportion of SAT test takers who scored better than Ann on the SAT.^[If 84% had lower scores than Ann, the number of people who had better scores must be 16%. (Generally ties are ignored when the normal model, or any other continuous distribution, is used.)]</div>\EndKnitrBlock{guidedpractice}


#### Normal probability examples {-}

Cumulative SAT scores are approximated well by a normal model, $N(\mu=1500, \sigma=300)$.

\BeginKnitrBlock{example}<div class="example">Shannon is a randomly selected SAT taker, and nothing is known about Shannon's SAT aptitude. What is the probability that Shannon scores at least 1630 on her SATs?

---

First, always draw and label a picture of the normal distribution. (Drawings need not be exact to be useful.) We are interested in the chance she scores above 1630, so we shade the upper tail.  See the normal curve below.


The picture shows the mean and the values at 2 standard deviations above and below the mean. The simplest way to find the shaded area under the curve makes use of the Z score of the cutoff value. With $\mu=1500$, $\sigma=300$, and the cutoff value $x=1630$, the Z score is computed as
\begin{eqnarray*}
Z = \frac{x - \mu}{\sigma} = \frac{1630 - 1500}{300} = \frac{130}{300} = 0.43
\end{eqnarray*}
We use software to find the percentile of $Z=0.43$, which yields 0.6664. However, the percentile describes those who had a Z score *lower* than 0.43. To find the area *above* $Z=0.43$, we compute one minus the area of the lower tail, as seen below.

The probability Shannon scores at least 1630 on the SAT is 0.3336.</div>\EndKnitrBlock{example}

<img src="05-inference-cat_files/figure-html/satAbove1630-1.png" width="70%" style="display: block; margin: auto;" />


<img src="05-inference-cat_files/figure-html/subtractingArea-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{protip}<div class="protip">**Always draw a picture first, and find the Z score second.**

For any normal probability situation, *always always always* draw and label the normal curve and shade the area of interest first. 
The picture will provide an estimate of the probability.

After drawing a figure to represent the situation, identify the Z score for the observation of interest.</div>\EndKnitrBlock{protip}


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">If the probability of Shannon scoring at least 1630 is 0.3336, then what is the probability she scores less than 1630? Draw the normal curve representing this exercise, shading the lower region instead of the upper one.^[We found the probability to be 0.6664. A picture for this exercise is represented by the shaded area below "0.6664".]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{example}<div class="example">Edward earned a 1400 on his SAT. What is his percentile?

---
 
First, a picture is needed. Edward's percentile is the proportion of people who do not get as high as a 1400. These are the scores to the left of 1400.

Identifying the mean $\mu=1500$, the standard deviation $\sigma=300$, and the cutoff for the tail area $x=1400$ makes it easy to compute the Z score:
\begin{eqnarray*}
Z = \frac{x - \mu}{\sigma} = \frac{1400 - 1500}{300} = -0.33
\end{eqnarray*}
Using the normal probability table, identify the row of $-0.3$ and column of $0.03$, which corresponds to the probability $0.3707$. Edward is at the $37^{th}$ percentile.</div>\EndKnitrBlock{example}


<img src="05-inference-cat_files/figure-html/satBelow1400-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Use the results of the previous example to compute the proportion of SAT takers who did better than Edward. Also draw a new picture.^[If Edward did better than 37% of SAT takers, then about 63% must have done better than him. 
\includegraphics[height=14mm]{02/figures/satBelow1400/satAbove1400}]</div>\EndKnitrBlock{guidedpractice}


\BeginKnitrBlock{protip}<div class="protip">**Areas to the right.**

The normal probability table in most books gives the area to the left. If you would like the area to the right, first find the area to the left and then subtract this amount from one.</div>\EndKnitrBlock{protip}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Stuart earned an SAT score of 2100. Draw a picture for each part. (a) What is his percentile? (b) What percent of SAT takers did better than Stuart?^[Numerical answers: (a) 0.9772. (b) 0.0228.]</div>\EndKnitrBlock{guidedpractice}

Based on a sample of 100 men,^[This sample was taken from the USDA Food Commodity Intake Database.] the heights of male adults between the ages 20 and 62 in the US is nearly normal with mean 70.0'' and standard deviation 3.3''.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Mike is 5'7'' and Jim is 6'4''. (a) What is Mike's height percentile? (b) What is Jim's height percentile? Also draw one picture for each part.^[First put the heights into inches: 67 and 76 inches. Figures are shown below. (a) $Z_{Mike} = \frac{67 - 70}{3.3} = -0.91\ \to\ 0.1814$. (b) $Z_{Jim} = \frac{76 - 70}{3.3} = 1.82\ \to\ 0.9656$. \\\includegraphics[height=14mm]{02/figures/mikeAndJimPercentiles/mikeAndJimPercentiles}]</div>\EndKnitrBlock{guidedpractice}

The last several problems have focused on finding the probability or percentile for a particular observation. 
What if you would like to know the observation corresponding to a particular percentile?


\BeginKnitrBlock{example}<div class="example">Erik's height is at the $40^{th}$ percentile. How tall is he?

---

As always, first draw the picture (see below).

In this case, the lower tail probability is known (0.40), which can be shaded on the diagram. We want to find the observation that corresponds to this value. As a first step in this direction, we determine the Z score associated with the $40^{th}$ percentile.

Because the percentile is below 50%, we know $Z$ will be negative. Looking in the negative part of the normal probability table, we search for the probability *inside* the table closest to 0.4000. We find that 0.4000 falls in row $-0.2$ and between columns $0.05$ and $0.06$. Since it falls closer to $0.05$, we take this one: $Z=-0.25$.

Knowing $Z_{Erik}=-0.25$ and the population parameters $\mu=70$ and $\sigma=3.3$ inches, the Z score formula can be set up to determine Erik's unknown height, labeled $x_{Erik}$:
\begin{eqnarray*}
-0.25 = Z_{Erik} = \frac{x_{Erik} - \mu}{\sigma} = \frac{x_{Erik} - 70}{3.3}
\end{eqnarray*}
Solving for $x_{Erik}$ yields the height 69.18 inches. That is, Erik is about 5'9'' (this is notation for 5-feet, 9-inches).</div>\EndKnitrBlock{example}


```r
qnorm(0.4, m = 0, s = 1)
#> [1] -0.253
```

<img src="05-inference-cat_files/figure-html/height40Perc-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{example}<div class="example">What is the adult male height at the $82^{nd}$ percentile?
 
---
 
Again, we draw the figure first (see below).

Next, we want to find the Z score at the $82^{nd}$ percentile, which will be a positive value. Using `qnorm()`, the $82^{nd}$ percentile corresponds to $Z=0.92$. Finally, the height $x$ is found using the Z score formula with the known mean $\mu$, standard deviation $\sigma$, and Z score $Z=0.92$:
\begin{eqnarray*}
0.92 = Z = \frac{x-\mu}{\sigma} = \frac{x - 70}{3.3}
\end{eqnarray*}
This yields 73.04 inches or about 6'1'' as the height at the $82^{nd}$ percentile.</div>\EndKnitrBlock{example}


```r
qnorm(0.82, m = 0, s = 1)
#> [1] 0.915
```

<img src="05-inference-cat_files/figure-html/height82Perc-1.png" width="70%" style="display: block; margin: auto;" />


\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">(a) What is the $95^{th}$ percentile for SAT scores?   
(b) What is the $97.5^{th}$ percentile of the male heights? As always with normal probability problems, first draw a picture.^[Remember: draw a picture first, then find the Z score. (We leave the pictures to you.) The Z score can be found by using the percentiles and the normal probability table. (a) We look for 0.95 in the probability portion (middle part) of the normal probability table, which leads us to row 1.6 and (about) column 0.05, i.e., $Z_{95}=1.65$. Knowing $Z_{95}=1.65$, $\mu = 1500$, and $\sigma = 300$, we setup the Z score formula: $1.65 = \frac{x_{95} - 1500}{300}$. We solve for $x_{95}$: $x_{95} = 1995$. (b) Similarly, we find $Z_{97.5} = 1.96$, again setup the Z score formula for the heights, and calculate $x_{97.5} = 76.5$.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">(a) What is the probability that a randomly selected male adult is at least 6'2'' (74 inches)?  
(b) What is the probability that a male adult is shorter than 5'9'' (69 inches)?^[Numerical answers: (a) 0.1131. (b) 0.3821.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{example}<div class="example">What is the probability that a random adult male is between 5'9'' and 6'2''?
 
---
 
These heights correspond to 69 inches and 74 inches. First, draw the figure. The area of interest is no longer an upper or lower tail.  


The total area under the curve is 1. If we find the area of the two tails that are not shaded (from the previous Guided Practice, these areas are $0.3821$ and $0.1131$), then we can find the middle area:
  
That is, the probability of being between 5'9'' and 6'2'' is 0.5048.</div>\EndKnitrBlock{example}


<img src="05-inference-cat_files/figure-html/unnamed-chunk-123-1.png" width="70%" style="display: block; margin: auto;" />



<img src="05-inference-cat_files/figure-html/unnamed-chunk-124-1.png" width="70%" style="display: block; margin: auto;" />




\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What percent of SAT takers get between 1500 and 2000?^[This is an abbreviated solution. (Be sure to draw a figure!) First find the percent who get below 1500 and the percent that get above 2000: $Z_{1500} = 0.00 \to 0.5000$ (area below), $Z_{2000} = 1.67 \to 0.0475$ (area above). Final answer: $1.0000-0.5000 - 0.0475 = 0.4525$.]</div>\EndKnitrBlock{guidedpractice}

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">What percent of adult males are between 5'5'' and 5'7''?^[5'5'' is 65 inches. 5'7'' is 67 inches. Numerical solution: $1.000 - 0.0649 - 0.8183 = 0.1168$, i.e., 11.68%.]</div>\EndKnitrBlock{guidedpractice}

#### 68-95-99.7 rule {-}

Here, we present a useful general rule for the probability of falling within 1, 2, and 3 standard deviations of the mean in the normal distribution. The rule will be useful in a wide range of practical settings, especially when trying to make a quick estimate without a calculator or Z table.


<div class="figure" style="text-align: center">
<img src="05-inference-cat_files/figure-html/er6895997-1.png" alt="Probabilities for falling within 1, 2, and 3 standard deviations of the mean in a normal distribution." width="70%" />
<p class="caption">(\#fig:er6895997)Probabilities for falling within 1, 2, and 3 standard deviations of the mean in a normal distribution.</p>
</div>



\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">Use `pnorm()` (or a  Z table) to confirm that about 68%, 95%, and 99.7% of observations fall within 1, 2, and 3, standard deviations of the mean in the normal distribution, respectively. For instance, first find the area that falls between $Z=-1$ and $Z=1$, which should have an area of about 0.68. Similarly there should be an area of about 0.95 between $Z=-2$ and $Z=2$.^[First draw the pictures. To find the area between $Z=-1$ and $Z=1$, use `pnorm()` or the normal probability table to determine the areas below $Z=-1$ and above $Z=1$. Next verify the area between $Z=-1$ and $Z=1$ is about 0.68. Repeat this for $Z=-2$ to $Z=2$ and also for $Z=-3$ to $Z=3$.]</div>\EndKnitrBlock{guidedpractice}

It is possible for a normal random variable to fall 4, 5, or even more standard deviations from the mean. However, these occurrences are very rare if the data are nearly normal. The probability of being further than 4 standard deviations from the mean is about 1-in-30,000. For 5 and 6 standard deviations, it is about 1-in-3.5 million and 1-in-1 billion, respectively.

\BeginKnitrBlock{guidedpractice}<div class="guidedpractice">SAT scores closely follow the normal model with mean $\mu = 1500$ and standard deviation $\sigma = 300$.   
(a) About what percent of test takers score 900 to 2100?  
(b) What percent score between 1500 and 2100?^[(a) 900 and 2100 represent two standard deviations above and below the mean, which means about 95% of test takers will score between 900 and 2100. (b) Since the normal model is symmetric, then half of the test takers from part (a) ($\frac{95\%}{2} = 47.5\%$ of all test takers) will score 900 to 1500 while 47.5% score between 1500 and 2100.]</div>\EndKnitrBlock{guidedpractice}


## Chapter 5 review {#chp5-review}

### Terms

We introduced the following terms in the chapter. 
If you're not sure what some of these terms mean, we recommend you go back in the text and review their definitions.
We are purposefully presenting them in alphabetical order, instead of in order of appearance, so they will be a little more challenging to locate. 
However you should be able to easily spot them as **bolded text**.

<table>
<tbody>
  <tr>
   <td style="text-align:left;"> alternative hypothesis </td>
   <td style="text-align:left;"> normal model </td>
   <td style="text-align:left;"> percentile interval </td>
   <td style="text-align:left;"> statistical inference </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Central Limit Theorem </td>
   <td style="text-align:left;"> null distribution </td>
   <td style="text-align:left;"> point estimate </td>
   <td style="text-align:left;"> success-failure condition </td>
  </tr>
  <tr>
   <td style="text-align:left;"> confidence interval </td>
   <td style="text-align:left;"> null hypothesis </td>
   <td style="text-align:left;"> pooled proportion </td>
   <td style="text-align:left;"> test statistic </td>
  </tr>
  <tr>
   <td style="text-align:left;"> confirmation bias </td>
   <td style="text-align:left;"> one-sided hypothesis test </td>
   <td style="text-align:left;"> sampling distribution </td>
   <td style="text-align:left;"> two-sided hypothesis test </td>
  </tr>
  <tr>
   <td style="text-align:left;"> hypothesis test </td>
   <td style="text-align:left;"> p-value </td>
   <td style="text-align:left;"> SE interval </td>
   <td style="text-align:left;"> Type 1 Error </td>
  </tr>
  <tr>
   <td style="text-align:left;"> margin of error </td>
   <td style="text-align:left;"> parameter </td>
   <td style="text-align:left;"> standard error for difference in proportions </td>
   <td style="text-align:left;"> Type 2 Error </td>
  </tr>
  <tr>
   <td style="text-align:left;"> normal curve </td>
   <td style="text-align:left;"> parametric bootstrap </td>
   <td style="text-align:left;"> standard error of single proportion </td>
   <td style="text-align:left;"> Z score </td>
  </tr>
  <tr>
   <td style="text-align:left;"> normal distribution </td>
   <td style="text-align:left;"> percentile </td>
   <td style="text-align:left;"> standard normal distribution </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>
