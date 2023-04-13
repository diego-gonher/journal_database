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
To model the 21-cm signal and the underlying astrophysics of heating and reionization, we use the publicly available seminumerical formalism, 21cmFAST. We first generate the initial density perturbation at z = 300 which are evolved using the Zel’dovich approximation at later redshifts. To produce the ionization map, the density field is first mapped on a coarser grid. Then, 21cmFAST uses an excursion-set based formalism to identify the ionized regions by comparing the number of ionizing photons with the number of baryons within spheres of decreasing radius. The resulting ionization map is then converted into the 21-cm brightness temperature map, which can then be used to calculate the power spectrum. The model they consider using 21cmFAST has 6 parameters:

- Ionizing efficiency $\zeta$: Reflects how efficient are UV photons from high z galaxies in ionizing the neutral hydrogen. 
- Minimum virial temperature of haloes $T^{min}_{vir}$: The minimum threshold for a halo to host a star-forming galaxy is defined in terms of its virial temperature. Naturally, if the halo can't create stars, then it won't help in ionizing the hydrogen.
- Mean free path of the ionizing photons $R_{mfp}$: How far ionizing photons travel changes the evolution of the EoR.
- Integrated soft-band luminosity $L_{X<2 keV}/SFR$: analogous to the ionizing efficiency but for X-rays that drive the EoH. In other words, this controls the efficiency with which X-rays heat the IGM.
- X-ray energy threshold for self-absorption by the host galaxies $E_0$: The minimm energy that soft X-rays need in order to not be self absorbed by their host galaxy, approximated as a step function.
- X-ray spectral index $\alpha _X$: x Governs the spectrum that emerges from the X-ray sources and depends on the dominant physical process emitting the X-ray photons.

## Results


---
# References