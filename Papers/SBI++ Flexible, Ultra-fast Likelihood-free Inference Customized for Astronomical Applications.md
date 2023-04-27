# SBI++: Flexible, Ultra-fast Likelihood-free Inference Customized for Astronomical Applications

- Date Read: 2023-04-17
- Date Published: 2023-01-12
- Authors: Bingjie Wang, Joel Leja, V. Ashley Villar, et al.
- Status: Read ✅
- Rating: ⭐️⭐️⭐️⭐️⭐️ 
- Tags: [[SBI]] [[Redshift Estimation]] [[SED fitting]]
- Arxiv: https://arxiv.org/pdf/2304.05281.pdf

---
# Abstract
Effective applications of traditional SBI in real-world applications is very complicated, as most traditional methods assume that the observed data has the same characteristics as the training data set (such as same underlying distribution and a similar level of uncertainty). In astronomy, where the need for SBI applications is rapidly increasing, this is certainly a problem, as data can often be inhomogenous (affecting the observed underlying distribution and uncertainties, or causing missing data). To tackle this, the paper introduces SBI++, a method that can handle Out-of-Distribution (OOD) observed errors and missing data. As a demonstration, the paper focuses on applying this approach to calculating photometric redshifts of galaxies, as well as some of their key properties via SED fitting. The main advantage of SBI++ is that it allows for significant speedups found within SBI methods, while improving the limitations of astronomical datasets.

---
# Notes
## Introduction
### Context
SED fitting of photometric data for galaxies can be used to infer some of their key properties, as well as a photometric redshift. The way this is usually done is to use a stellar population sythesis (SPS) code, which can be coupled with something like MCMC or Nested Sampling (NS), to create a best fit and recover the desired parameters. However, a single fit requires around a million SPS models, meaning that producing these fits for the increasingly larger photometric surveys becomes too computationally expensive to be feasible.

Currently, there are no obvious ways in which to improve the model generation. Thus, the most promising idea is to completely bypass the likelihood calculation entirely, which can be acomplished by using SBI methods. There are different SBI methods, but the paper mainly focuses on using normalizing flows to learn the posterior densities directly. 

There is, however, an important limitation. The data to be modeled must have identitcal properties as the training data, including nearly identical noise properties, free parameters, exact priors and so forth. Naturally, this is not a very realistic case, and certainly not the norm in observational astronomy, where data can be largely inhomogenous. 

For comparison purposes, the "traditional" method that they're using is an NS code called "dynesty". 

### Baseline SBI
Also for comparison, they create a "baseline" SBI model. This baseline SBI consists of using normalizing flows to do posterior density estimation. Specifically, they use an 18-parameter SED model to create a dataset of around 2 million instances. They inject noise to the mock observations (for a detailed description of their noise model, see Section 2 of the paper). For the validation set, they create 5,000 examples that seem to be actual results from the tradional NS method (hence why there aren't that many, and it seems like a different interpretation of what the validation set is). 

For their network, they use an implementation of the Masked Autoregressive Flow implemented in the sbi package. Their network consists of 15 blocks, each with 2 hidden layers and 500 hidden units.

As a demonstration, they evaluate the performance of the baseline SBI within the idealized case, and show that it performs exceptionally well on data that closely follows the training set distribution. 

## Methods
### SBI++
There are two main issues that SBI++ tries to adress:

#### Missing data
If there is a missing band in the photometry, SBI++ imputes the missing data by first finding all the SEDs in the training data set whose reduse $\chi^2$ calculated with respect to the observed SED are $\leq5$. Then a Kernel Density Estimation (KDE) is created from those nearest neighbors, weighted by the inverse of their Euclidean distances in photometry space. Finally, they draw random samples from such KDE and pass them to the baseline SBI.

#### OOD measurement errors
For this part, SBI++ first compares the observed uncertainty with the uncertainty used for the toy noise model in their training dataset. They use a metric and a threshold to mark if any observed uncertainty is OOD. In the cases where this happens, a set of simulated photometry drawing form a Gaussian distribution with a mean of the observed value and a standard deviation of the observed uncertainty is created. These measurements are passed then through the baseline SBI model to produce posteriors, averaging over all the "noisy" posterior samples and providing the final parameter estimations. 

Here it is important to note that the MC process to generate the noisy data might instatiate simulated data that lies outside of one's model space entirely, causing an extremely incorrect parameter estimation when passed through the baseline SBI. The way they adress this issue is found in Section 4.1 of the paper.

Lastly, it is important to also note that the introduction of MC samples introduces to hyperparameters to the model: the number of MC samples and the number of posterior samples. They argue that one can adjust these two hyperparameters to play with the accuracy-execution time trade-off.

## Results
Figure 2 shows the overall approach of SBI++ and how it compares to NS and the baseline SBI. As we can see, SBI++ seems to closely match the standard results from NS in handling missing data, and it seems to work better than traditional SBI in handling OOD measurement errors. They also apply this to observed JWST data, but I didn't have time to get into that, so refer to Section 5 of the paper for more details. Also a detailed interpretation of the results and a comparison with traditional NS can be found in section 6 of the paper, which I highly reccomend if you're curious about it.

> **NOTES:** A little complicated to read/understand, but I think their ideas on how to handle missing data are quite interesting and perhaps useful.

---
# References

- [[Masked Autoregressive Flow for Density Estimation]]
- [[SBI -- A toolkit for simulation-based inference]]