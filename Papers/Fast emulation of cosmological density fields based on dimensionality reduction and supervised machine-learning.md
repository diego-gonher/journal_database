# Fast emulation of cosmological density fields based on dimensionality reduction and supervised machine-learning

- Date Read: 2023-04-27
- Date Published:
- Authors: Miguel Conceição, Alberto Krone-Martins, Antonio da Silva, et al.
- Status: Read ✅
- Rating: ⭐️⭐️⭐️
- Tags: [[Emulation]]
- Arxiv: https://arxiv.org/pdf/2304.06099.pdf

---
# Abstract
N-body simulations are costly, a problem certainly seen in cosmological simulations, where they're used to study the non-linear evolution of structure formation. In this paper, the authors propose two emulators, one that maps $\Omega_m$ to a simulated cube, and another that maps $\Omega_m, z$ also to a simulated cube. Their approach is interesting however, as before training the emulator, they use principal component analysis (PCA) and then investigate the use of different supervised-learning methods to do the emulations. They claim a speed-up of around three orders of magnitude for each emulation.

---
# Notes
## Introduction
N-body simulations can be used to provide high-accuracy predictions on how the dark matter (DM) density and velocity fields evolve, allowing the identification of model signatures and observables that can be compared against observations. However, these simulations can be very computationally expensive, which limits our ability to explore the parameter space within a reasonable time (something necessary for model evaluation and parameter inference, for instance). Emulation, thus, has become an increasingly common technique to accelerate and approximate these simulations. In this paper, they study two scenarios:

- Emulations using only the dark matter density (Ωm) as a single free-parameter. 
- Emulations with the redshift $z$ as an additional free-parameter.

## Methods
Considering that we are dealing with non-random data with an high degree of symmetries brought by the cosmological principle, it should be possible to take advantage of the redundancy in this data, compressing it in a more compact basis. Using this reduced data representation, it becomes more feasible to apply simpler machine learning models, decreasing further the amount of computational resources and time needed to obtain the out- puts, and hopefully increasing the interpretability of the models. This is why the authors propose taking the simulated dataset (training dataset) and compress it via PCA before applying any supervised machine learning method. To perform this PCA, they apply Singular Value Decomposition (SVD).

The supervised ML methods that they explore are:

- Neural Networks
- SVMs
- Random Forests
- Extremely Randomized Trees: A version of RF where pruning is used to reduce the number of data "features" are used as input for each tree.

For the training of these ML algorithms, they choose a loss that works as a distance metric between the real power spectrum and the emulated one. See eq 1 for the definition.

For the training set, they have 23 simulations for the one-parameter scneario, and 23 $\times$ 4 for the two-parameter one.

## Results
To evaluate their results, they compute the power spectrum and bispectrum of the emulations versus the test dataset. 

### One-parameter
In this scenario, the NN and the RF perform the best in the power spectrum for the different redshifts evaluated. The power spectrum is well predicted to a few percent accuracy, though the bispectrum predictions are somewhat worse.

### Two-parameter
Surprisingly, here SVMs work the best. Look in the actual paper if you're interested.


> **NOTES:** Honestly, the paper and the results weren't that interesting. However, the idea of doing dimensionality reduction before supervised ML was interesting.

---
# References