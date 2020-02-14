---
layout: post
title:  "Gabor Wavelets for Sparse Coding of Audio"
date:   2019-02-14 17:22:18 -0400
categories: wavelets signal processing
---

Chances are if you've been in the machine learning space you have attempted to classify some sort of audio using Fourier Coeffiecients or a STFT Spectrogram. This has proven to be a good standard for audio feature extraction for most applications. However, one weakeness of the Fourier transform is that the basis functions are indefinite, as sine waves and cosine waves go on forever.

<img src = "https://www.investopedia.com/thmb/o2tPuXZxDRHAPn-VtLuQLX3IALQ=/980x650/filters:no_upscale():max_bytes(150000):strip_icc()/dc-vs-ac-alternating-current-sine-wave-waveform-5b74a6e146e0fb005063fb8e.png">

Whereas with Gabor wavelets, the power is localized in space.

<img src = "https://www.researchgate.net/profile/Martin_Baltazar-Lopez/publication/26898388/figure/fig9/AS:609934647513088@1522431160045/Oscillatory-Gabor-function-used-for-wavelet-transformation-scale-a1-translation-b0.png">

## Formulation

You can think of Gabor wavelets as Gaussian waves, with mean $$ \mu $$ and variance $$ \sigma^2 $$. The equation for a 1D real-valued Gabor wavelet is actually just:

$$G(x, \mu, \sigma) = e^{-(x-\mu)^2/\sigma^2}$$

Which follows the Gaussian distribution. 

## Building a Gabor Overcomplete Dictionary

So the whole idea behind sparse representation is that you have a "dictionary" which is overcomplete, meaning that is has more vectors than dimensions and the vectors obviously form a basis for the dimensions. We can use small number of combinations of "words" in this dictionary to form new words. The reason we call this representation sparse is then because we only need to know about a few words to construct a new word, meaning less information has to be sent about the new word.

In order to then build the Gabor dictionary, we use an abundant combination of $$/mu$$'s and $$\sigma$$'s to represent the signal. So if the signal we are trying to represent is 10 samples long, we may use a dictionary of length 500, each sample with a unique, equally distributed $$\mu$$ and $$\sigma$$.