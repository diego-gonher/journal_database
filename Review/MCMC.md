# MCMC
In preparation for my new research project, here is a series of videos that I'm finding very useful to understand MCMC. The videos are from a series, and I've ordered them in the sequence in which I think they should be viewed.

---

## Accept-Reject Sampling
A method to produce approximate samples from an unkown underlying distribution, by using a simpler distribution that we can more easily sample from. It is really good, and the heart of MCMC, but it is highly inefficient in its simplest form.

🎥   https://www.youtube.com/watch?v=OXDqjdVVePY&ab_channel=ritvikmath

---

## Markov Chains
A simple model that allows us to explore the behavior within a system that has a defined set of states that transition between each other defined by a transition probability (summarized in a transition matrix) that **only** depends on the previous state. A key concept is that of a **steady state**, which is the case where the probabilities for each one of the states converge to specific values if the chain is long enough.

🎥   https://www.youtube.com/watch?v=prZMpThbU3E&ab_channel=ritvikmath

---

## Markov Chain Stationary Distribution
This is an extension of the idea of **steady state** for a Markov Chain. The idea here is that, for a given Markov Chain, you may have a probability distribution $\vec{\pi} = \{\pi_A, \pi_B, \pi_C, \dots\}$ (which describes the probability of the Markov Chain of being in a specif state at any given step) that stays unchanged, no matter how many forward steps you take. Such distribution is known as the stationary distribution of the chain. Written in terms of Linear Algebra, and using the definition of the Transition Matrix, the stationary distribution is the left eigenvector of the Transition Matrix with eigenvalue 1. It is important to know that stationary distributions are not necessarily unique for a given Markov Chain.

🎥   https://www.youtube.com/watch?v=4sXiCxZDrTU&ab_channel=ritvikmath

---

## Markov Chain Monte Carlo
MCMC is, in a way, a more efficient sampling method than Accept-Reject sampling. The idea of Rejection Sampling is relatively simple, yet powerful. However, it tends to be very efficient since every new sample is totally uncorrelated from the previous ones. MCMC methods instead try to be more efficient by using the probability that a given sample (for a given state) will be accepted to know if it should keep sampling nearby. The idea is that if a new sample has an unusually high probability of being accepted, then it might be a good idea to spend some time exploring the neighborhood around it, as it might be a mode within the distribution.

More specifically, MCMC methods treat the sampling process of Rejection sampling as a Markov Chain, where the state used for the next sample depends on the current state and its sample. The algorithms themselves are designed such that the Markov Chain will eventually reach a steady state with a Stationary Distribution that is the same as the target (underlying) distribution that we are trying to approximate. This involves thinking about how to set our probability matrix to make this happen, and it uses the principle of Detailed Balance, which is closely related to a Stationary Distribution being a left eigenvector of the Transition Matrix.

🎥   https://www.youtube.com/watch?v=yApmR-c_hKU&ab_channel=ritvikmath

---

## Metropolis - Hastings

🎥   https://www.youtube.com/watch?v=yCv2N7wGDCw&ab_channel=ritvikmath
https://www.youtube.com/watch?v=OTO1DygELpY&ab_channel=StataCorpLLC

---

## Hamiltonian Monte Carlo

🎥   https://www.youtube.com/watch?v=a-wydhEuAm0&ab_channel=BenLambert
📄   https://arxiv.org/pdf/1701.02434.pdf
