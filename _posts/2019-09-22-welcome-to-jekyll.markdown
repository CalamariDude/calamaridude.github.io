---
layout: post
title:  "Maximum Log Likelihood vs Maximum Aposterior"
date:   2019-09-26 01:59:45 -0400
categories: jekyll update
permalink: /posts/1
---


# Maximum Log Likelihood vs Maximum Aposterior

If you're like me, this whole maximum log likihood vs baysian ordeal mumbo jumbo has made you dizzy. The first time I learned this, I didn't quite understand what the fuss was about. Why are we using "Log" and "Likelihood" and why does "Aposterior" sound so foreign. I've made a short blog post to help those out that, like me, didn't quite get it the first time.

## What is a Maximum Likelihood?
The fundamental idea you have to understand about Maximum Likelihood is that it tries to ask the question, what is the best "model" or "estimate" that we can give <em>just</em> by looking at the data we have collected. It's actually really simple when we look at an example.

### Coin Flip Example
We want to get the probability of this coin landing on heads. We flip it 10 times to determine this coin's probability of landing heads. Let's say we get 6 heads and 4 tails. Maximum Likelihood says the probability of getting heads is $$P(X=Heads) = 6/10=.6$$. Based on our data, and no prior belief of the coin's chances, Maximum Likelihood tries to predict the probability. We will go over the math in a bit.

## What about Maximum Aposterior?
Maximum Aposterior (MAP) takes a baysian approach to this (aka using prior knowledge to help get a more accurate model) which can be particularly useful if you don't have too much data to let the [Law of Large Numbers][LLN] do its job. Now when you flip a fair coin, you should expect to see that you get about 5 heads in 10 flips. However, in our experiment, we got 6 heads in 10 flips. Based on our experience, coins typically give a 50/50 fair chance, so we can incorporate this into our model to improve its accuracy with low data. With a Maximum Aposterior estimate, we use both our data and our believe to make the model. Since our "Prior" is $$P(X=Heads)= .5$$ and our actual experiment yielded $$P(X=Heads)= .6$$, our Maximum Aposterior should yield somewhere between $$.5$$ and $$.6$$ probabiliy. Now I know this is vague, but with baysian probability the exact probability estimate will be determined by how much we believe in our prior vs our data. If we think we are super sure that a coin has .5 probability because its a really special coin made only for NFL coin flips, then we may weigh our prior to be harder to change, meaning our MAP estimator will output something very close to $$.5$$.

<br><br><br><br><br>

[LLN]: https://en.wikipedia.org/wiki/Law_of_large_numbers
