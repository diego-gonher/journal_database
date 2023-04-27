# Constraining the X-ray heating and reionization using 21-cm power spectra with Marginal Neural Ratio Estimation

- Date Read: 2023-04-13
- Date Published: 2023-03-14
- Authors: Anchal Saxena, Alex Cole, Simon Gazagnes, et al.
- Status: Read ✅
- Rating: ⭐️⭐️⭐️⭐️⭐️ 
- Tags: [[SBI]] [[EoR]] [[21cm]]
- Arxiv: https://arxiv.org/pdf/2303.07339.pdf

---
# Abstract
The 21cm power spectrum can be used to understand the evolution of the Epoch of Heating (EoH) and the Epoch of Reionization (EoR). With the creation of extremely sensitive radio interfeometric observations (like SKA), the possibility to constrain the parameters that model these epochs becomes feasible. However, doing parameter estimation using the traditional MCMC methods is very computationally expensive. To alleviate this problem, the authors show how using a Simulation Based Inference (SBI) approach via Marginal Neural Ratio Estimation (MNRE) can be used.

---
# Notes
## Introduction
Given that the 21cm line is a great tracer of neutral hydrogen, its power spectrum can be used to rule out certain astrophysical models and provide a better understanding of the EoR. However, modeling the 21cm signal from the Cosmic Dawn (CD) and EoR using full Radiative Transfer (RT) simulations is extremelly expensive and unreasonable for parameter inference. To overcome this, various semi-analytical approaches and approximations have been developed. Particularly, 21cmFAST has been widely used to model the 21cm signal from these epochs. However, even these semi-analytical approaches can become quite cumbersome when trying to do parameter inference via MCMC, as the dimensionality of parameter space increases. To overcome this, the authors use 21cmFAST simulations to use a Neural Network that allows them to do MNRE, allowing them to do parameter estimation with around an order of magnitude less simulations. 

## Methods
There are two main components of the approach they used in this paper. First, there are the simulations themselves that model the 21cm signal, and then there is the SBI method they employed. Briefly:

### 21cmFAST
To model the 21-cm signal and the underlying astrophysics of heating and reionization, we use the publicly available seminumerical formalism, 21cmFAST. We first generate the initial density perturbation at z = 300 which are evolved using the Zel’dovich approximation at later redshifts. To produce the ionization map, the density field is first mapped on a coarser grid. Then, 21cmFAST uses an excursion-set based formalism to identify the ionized regions by comparing the number of ionizing photons with the number of baryons within spheres of decreasing radius. From the 21cmFAST paper:

> The foundation of this approach is to require that the number of ionizing photons inside a region be larger than the number of hydrogen atoms it contains. Then ionized regions are identified via an excursion-set approach, starting at large scales and progressing to small scales, analogous to the derivation of the Press-Schechter (PS) mass functions.

The resulting ionization map is then converted into the 21-cm brightness temperature map, which can then be used to calculate the power spectrum. The model they consider using 21cmFAST has 6 parameters:

- Ionizing efficiency $\zeta$: Reflects how efficient are UV photons from high z galaxies in ionizing the neutral hydrogen. 
- Minimum virial temperature of haloes $T^{min}_{vir}$: The minimum threshold for a halo to host a star-forming galaxy is defined in terms of its virial temperature. Naturally, if the halo can't create stars, then it won't help in ionizing the hydrogen.
- Mean free path of the ionizing photons $R_{mfp}$: How far ionizing photons travel changes the evolution of the EoR.
- Integrated soft-band luminosity $L_{X<2 keV}/SFR$: analogous to the ionizing efficiency but for X-rays that drive the EoH. In other words, this controls the efficiency with which X-rays heat the IGM.
- X-ray energy threshold for self-absorption by the host galaxies $E_0$: The minimm energy that soft X-rays need in order to not be self absorbed by their host galaxy, approximated as a step function.
- X-ray spectral index $\alpha _X$: x Governs the spectrum that emerges from the X-ray sources and depends on the dominant physical process emitting the X-ray photons.

The simulation data is composed of 20,000 power spectra samples evaluated at ten different redshifts in range (25, 6). These samples are drawn randomly from the priors used that are described in the paper. We use 80% of the samples for training, 10% for validation, and 10% for the test dataset.

### MNRE
For this part, I defintely suggest taking a look at section 2 *"IMPLEMENTATION OF MNRE USING swyft"* from the paper, as they do a good job at explaining NRE. Very broadly, you start with Bayes' Theorem. Since we don't have the likelihood nor the evidence, we can define a new quantity, the ratio, as the ratio between these two as they appear in the theorem. This ratio $r(x, \theta)$ is equal to the ratio of the joint probability density $p(x, \theta)$ to the product of marginal probability densities $p(x) p(\theta)$. A binary classifier $d_\phi(x, \theta)$ is then trained to distinguish between jointly-drawn and marginally drawn pairs. Here $\phi$ denotes the learnable parameters of the model, which are updated as the model is trained. This learning problem is associated with a binary cross-entropy loss function. Therefore, one can train a neural network that uses this loss function to approximate the classifier $d_\phi(x, \theta)$. Given its construction, the ratio $r(x, \theta)$ can be written in terms of the classifier $d_\phi(x, \theta)$ (as shown in Eq 5 of the paper), giving us a way to use the trained classifier to do parameter inference. A variant called Marginal Neural Ratio Estimation (MNRE) that is able to directly estimate marginal posteriors by omitting model parameters from the network’s input is used, via its implementation in the *swyft* package.

## Results
To test the performance of this approach, the authors create a mock observation. This model with {$\zeta$, $log_{10}(T^{min}_{vir})$, $R_{mfp}$, $log_{10}(L_X)$, $E_0$, $\alpha_X$} = {30, 4.70, 15, 40.5, 0.5, 1}. It corresponds to the FAINT GALAXIES model in which reionization is driven by numerous sources with low ionizing efficiency. This set of parameter values results in the reionization history and the Thomson optical depth $\tau$ consistent with Planck data.

In Figure 3, you can see the posteriors on the astrophysical parameters obtained from swyft for the FAINT GALAXIES model assuming 1000h observation from the SKA. They are able to tightly constrain all our model parameters except αX, which has a relatively small impact on the amplitude of the 21-cm power spectra.

In Figure 5 they show the same thing but this time separating the redshifts into two large bins, one coresponding to earlier times and the other to later times. The figure shows how, as you'd hopefully expect, later times better constrain the three parameters that are related to EoR, while the earlier times better constrain the parameters that more directly affect the EoH.

While their worlk shows that MNRE is a powerful framework to analyse the 21-cm power spectrum, in reality, the 21-cm signal during CD-EoR is highly non-Gaussian, so the 21-cm power spectrum is probably not the most optimal summary statistics to use for parameter inference. In future work, one can explore the higher-order summary statistics such as the 21-cm bispectrum, the morphology of the ionized regions  and convolutional neural networks on the 21-cm tomographic images for parameter inference through MNRE. What this work does show is the advantage of using MNRE as it allows for higher dimensional parameter spaces to be explored for more complex models.

> **NOTES:** This is a really good paper that describes the idea behind Neural Ratio Estimation.

---
# References

- [[Likelihood-free MCMC with Amortized Approximate Ratio Estimators]]
- [[On Contrastive Learning for Likelihood-free Inference]]
