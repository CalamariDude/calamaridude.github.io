---
layout: post
title:  "Maximum Log Likelihood vs Maximum Aposterior"
date:   2019-09-29 21:22:18 -0400
categories: ML learning bayesian
---
# Maximum Log Likelihood vs Maximum Aposterior

If you're like me, this whole maximum log likihood vs bayesian ordeal mumbo jumbo has made you dizzy. The first time I learned this, I didn't quite understand what the fuss was about. Why are we using "Log" and "Likelihood" and why does "Aposterior" sound so foreign. I've made a short blog post to help those out that, like me, didn't quite get it the first time.

## What is a Maximum Likelihood?
The fundamental idea you have to understand about Maximum Likelihood is that it tries to ask the question, what is the best "model" or "estimate" that we can give <em>just</em> by looking at the data we have collected. It's actually really simple when we look at an example.

### Coin Flip Example
We want to get the probability of this coin landing on heads. We flip it 10 times to determine this coin's probability of landing heads. Let's say we get 6 heads and 4 tails. Maximum Likelihood says the probability of getting heads is $$P(X=Heads) = 6/10=.6$$. Based on our data, and no prior belief of the coin's chances, Maximum Likelihood tries to predict the probability. We will go over the math in a bit. 

## What about Maximum Aposterior?
Maximum Aposterior (MAP) takes a bayesian approach to this (aka using prior knowledge to help get a more accurate model) which can be particularly useful if you don't have too much data to let the [Law of Large Numbers][LLN] do its job. Now when you flip a fair coin, you should expect to see that you get about 5 heads in 10 flips. However, in our experiment, we got 6 heads in 10 flips. Based on our experience, coins typically give a 50/50 fair chance, so we can incorporate this into our model to improve its accuracy with low data. With a Maximum Aposterior estimate, we use both our data and our belief to make the model. Since our "Prior" is $$P(X=Heads)=.5$$ and our actual experiment yielded $$P(X=Heads)=.6$$, our Maximum Aposterior should yield somewhere between $$.5$$ and $$.6$$ probabiliy. Now I know this is vague, but with baysian probability the exact probability estimate will be determined by how much we believe in our prior vs our data. If we think we are super sure that a coin has .5 probability because its a really special coin made only for NFL coin flips, then we may weigh our prior to be harder to change, meaning our MAP estimator will output something very close to $$.5$$ unless there is overwhelming evidence pulling it elsewhere.

## Math time
### Maximum Likelihood Estimator
Now from the coin flip example, it seems like the way you determine the maximum liklihood is just by getting the simple probability. This just so happens to be the case when we are getting the maximum liklihood of a Univeriate <em>Gaussian Distribution</em>. The maximum liklihood of a some data tries to estimate the true parameters of the population that the data came from. In our instance, we assume outcomes of the coin flip correspond to sum <em>normal</em> distribution. With this assumption, we will work out why the MLE of a coin flip data is just the mean of our data. 
<br><br>
With our coin example, we will first make it more formal:

Using this, we can write the data as $$X=(x_1,\dots,x_n)$$ where:


$$
x_i = \left\{
        \begin{array}{ll}
            1 & \quad \text{if Heads} \\
            0 & \quad \text{if Tails}
        \end{array}
    \right.
$$

Now let's define the MLE:

$$\theta_{MLE} = \max_{\theta \epsilon \Theta}P(X| \theta)$$

Okay so $$P(X ⎮ \theta)$$, what does it mean:

$$P(X|\theta) =P(x_1,\dots,x_n | \theta) = \prod_{i=1}^nP(x_i = x_i | \theta)$$

This is saying we need to pick parameters $$\theta$$ that maximizes the probability that each data point came from the probability distribution we are estimating the data points to come from. In our coin flip example, this means we need to pick a value that we can say, for a Bernoulli case of 10 coin flips where 6 outcomes are heads, what would be the average value we expect the next coin flip to be based on the previous 10 flips. This value works out to .6, and here's how we determine that:

Assume the coin flips $$X$$ comes from a normal distribution, $$X \sim N(\mu, \sigma^2)$$, so that means 

$$P(x|\theta) = \frac{1}{\sqrt(2\pi\sigma^2)}\exp(-\frac{1}{2\sigma^2}(x-\theta)^2)$$

Note that $$\theta$$ in this example means the $$\mu$$ that we are estimating, but we will continue to use $$\theta$$ for formality.

So essentially we want to maximize this probability term for every X by choosing a $$\theta$$ that maximizes it. But in math, minimizing a product of probabilities is hard, because taking the derivative of a huge product is quite ugly. We will use a trick by instead taking the **log** of the function. Using a log, we can split up the function and use the Log Product Rule to make this problem look a little easier:

$$\log(P(X|\theta)) = -\frac{n}{2}\log(2\pi\sigma^2)-\frac{1}{2\sigma^2}\sum_{i=1}^n(x_i-\theta)^2$$

Please refer to the [Log Rules][Rule] for how we got to this. Its also important to note that we can use log because log(x) is a *monotonically* increasing function and maximizing $$P(X)$$ is the *same* as maxmizing $$\log(P(X))$$. Now once we have this, we are ready to differentiate and set equal to 0 in order to find our [critical points][Critical]:

$$0 = \frac{d}{d\theta}\log(P(X|\theta) = \frac{1}{2\sigma^2}\sum2(x_i-\theta) = \frac{1}{\sigma^2}(\sum{}x_i-n\theta) = 0$$


$$\Rightarrow\frac{1}{n}\sum_{i=1}^nx_i = \theta_{MLE} = \bar{x}$$

That's crazy how this all simplifies down to $$\theta_{MLE} = \bar{x}$$, which means maximimizing our liklihood is just choosing the sample mean. BLows your MIND right? 

OK. Maybe not, but if that doesn't the MAP math will.

**Aside**: Now sometimes people will call the liklihood as the log liklihood, thats just because we maximize that instead, but its really doing the same thing, so most people use the liklihood and log liklihood synonymously.

### Maximium Aposterior

Now up until this point, we were pretty vague about what a prior meant, but let's make it absolutely clear what we mean by a prior. The prior refers to the $$P(A)$$ term in [Bayes' Theorem][Bayes].

$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

The top term is basically the maximim liklihood times a prior, this is the fundamental difference between the two, and you will see why this makes a difference as we work through the equations. So let's first define the the Maximum Aposterior.

$$\theta_{MAP} = \max_{\theta \epsilon \Theta}P(\theta | X)$$

Compare this function to the MLE objective function. What looks different?

If you noticed, we are maximizing $$P(\theta⎮X)$$ rather than $$P(X⎮\theta)$$. This essentially is saying we want to maxmize the posterior distribution in $\theta$. This fancy mumbo jumbo can be broken down to someting understandable. "Post" means after and "erior" refers to the the prior, so we want to maximize the probability of our after-prior distribution, or distribution we have after a prior knowledge of thinking its a fair coin. So what this objective function really is saying is:

$$\theta_{MAP} = \max_{\theta \epsilon \Theta}P(X | \theta)P(\theta)$$

which if you notice, comes from the numerator of Bayes rule. We can throw out the denominator of Bayes rule because maximizing the top is also maximizing the whole fraction.

Okay, let's get back to the math. In this problem, we also use the log to maximize so we can deal with each of those P terms separately using the log product rule. This will make our new objective function look like:

$$\theta_{MAP} = \max_{\theta \epsilon \Theta}\log(P(X | \theta)) + \log(P(\theta))$$

Let's just differentiate and set these bad boys equal to zero to maximize:

$$0 = \frac{d}{d\theta}(\log(P(X|\theta)) + \log(P(\theta)))$$

We already have the derivate of the first term since we already did it in MLE. It comes out to 

$$\frac{d}{d\theta}\log(P(X|\theta)) = \frac{1}{\sigma^2}(\sum{}x_i-n\theta)$$

Now we just have to take the derivative of the second term.

$$\frac{d}{d\theta}\log(P(\theta))$$
In order to differentiate this we need to define it:

$$P(\theta) = \frac{1}{\sqrt(2\pi\sigma^2)}\exp(-\frac{1}{2\sigma^2}(x-\mu)^2)$$

And now we take the log and differentiate this:

$$\frac{d}{d\theta}\log(P(\theta)) =\frac{d}{d\theta}(-\frac{1}{2}\log(2\pi) - \frac{1}{2}(\theta-\mu)^2)$$

$$= \theta-\mu$$

So now putting these together, we have 

$$\frac{d}{d\theta}(\log(P(X|\theta)) + \log(P(\theta)))= 0$$ 
$$\Rightarrow 0 = \frac{1}{\sigma^2}(\sum{}x_i-n\theta) + \mu + \theta$$

We rearrange this to get:

$$\Rightarrow \sum_{i=1}^n\frac{x_i}{\sigma^2} + \mu = \theta(\frac{n}{\sigma^2} + 1)$$

$$\Rightarrow\theta_{MAP} = \frac{n}{n+\sigma^2}\bar{x} + \frac{\sigma^2}{n+\sigma^2}\mu$$

I skipped some algebra, so go back and work out the last few steps yourself. But I wanted to get to the intuintion behind this result. 

As you can see, the left side of this equaition is the sample mean, and the right side is the prior mean. When the n is small, or you only flip the coin a few times, the term on the right will dominate more. However, as $$n\rightarrow\infty$$, the value of $$\theta_{MAP}$$ is mainly composed of the left term, as the right term goes to 0. Meaning, when we flip the coin a lot, and it keeps landing 6/10 times heads, we stop believing it was a fair sided NFL coin and more like a rigged unfrair coin. So as $$n\rightarrow\infty$$, MAP looks a lot like $$\bar{x}$$, which is the MLE.

## Conclusion
Whether you use MLE or MAP is up to you, if you think you the data is what you should base your estimates on and nothing else, MLE is the way to go. If you think you know something that will help the problem, especially if there isn't enough data or the data isn't that good, you might want to use Bayesian model like MAP.

[Bayes]: https://en.wikipedia.org/wiki/Bayes%27_theorem
[LLN]: https://en.wikipedia.org/wiki/Law_of_large_numbers
[Rule]: https://mathinsight.org/logarithm_basics
[Critical]: https://www.cliffsnotes.com/study-guides/calculus/calculus/applications-of-the-derivative/critical-points