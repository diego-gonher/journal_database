# SBI++: Flexible, Ultra-fast Likelihood-free Inference Customized for Astronomical Applications

- Date Read: 2023-04-17
- Date Published: 2023-01-12
- Authors: Bingjie Wang, Joel Leja, V. Ashley Villar, et al.
- Status: read
- Rating: ⭐️⭐️⭐️⭐️⭐️ 
- Tags: [[SBI]]
- Arxiv: https://arxiv.org/pdf/2304.05281.pdf

---
# Abstract
Effective applications of traditional SBI in real-world applications is very complicated, as most traditional methods assume that the observed data has the same characteristics as the training data set (such as same underlying distribution and a similar level of uncertainty). In astronomy, where the need for SBI applications is rapidly increasing, this is certainly a problem, as data can often be inhomogenous (affecting the observed underlying distribution and uncertainties, or causing missing data). To tackle this, the paper introduces SBI++, a method that can handle Out-of-Distribution (OOD) observed errors and missing data. As a demonstration, the paper focuses on applying this approach to calculating photometric redshifts of galaxies, as well as some of their key properties via SED fitting. The main advantage of SBI++ is that it allows for significant speedups found within SBI methods, while improving the limitations of astronomical datasets.

---
# Notes
## Introduction
SED fitting of photometric data for galaxies can be used to infer some of their key properties, as well as a photometric redshift. This method is, however, relatively slow. This is because the SED fitting uses stellar population synthesis (SPS) models. Since a signle SED fit requires around 1 million SPS models,

### Context
### Baseline SBI

## Methods
### SBI++

## Results


---
# References