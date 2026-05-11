# Nested Sampling — Marginal Likelihood Estimation

This notebook implements and studies the Nested Sampling algorithm (Skilling, 2006) for approximating the marginal likelihood (Bayesian evidence). It is based on the theoretical analysis of Chopin & Robert (2009).

> Reference article: [Chopin & Robert (2009), arXiv:0801.3887](https://arxiv.org/pdf/0801.3887)

---

## Overview

The marginal likelihood (or evidence) is defined as:

$$Z = \int L(\theta)\,\pi(\theta)\,d\theta$$

where $\pi(\theta)$ is the prior and $L(\theta)$ is the likelihood. This integral is intractable in high dimensions. Nested Sampling addresses this by transforming it into a one-dimensional integral over $[0,1]$ and approximating it via a sequential particle scheme.

The notebook implements and evaluates the algorithm on a multivariate Gaussian model across three increasingly complex settings.

---

## Structure

### Part 1 — Analytical Derivations
Closed-form derivations for the Gaussian model $y_i \sim \mathcal{N}_d(\theta, \Sigma)$ with prior $\theta \sim \mathcal{N}_d(0, I_d)$:
- **1a** — Posterior distribution $\hat{\pi}_n(\theta)$
- **1b** — Marginal likelihood $p(y)$
- **1c** — Likelihood $L(\theta)$
- **1d** — Rewriting of the constraint $L(\theta) > l$ as a ball condition

### Part 2 — Exact Constrained Sampling ($\bar{y} = 0$, $\Sigma = I_d$)
In this special case, the constraint $L(\theta) > l$ reduces to $\theta \in B_d(0, r_l)$, allowing exact sampling from the truncated prior using:
- Random direction sampling (uniform on the sphere)
- Inverse CDF sampling of the radius from a truncated $\chi^2$ distribution

Performance is evaluated across dimensions $d \in \{1, 2, 3, 5\}$ with $n=30$ observations and $N=200$ particles.

### Part 3 — MCMC Constrained Sampling (known $\Sigma$, arbitrary $\bar{y}$)
When exact sampling is no longer possible, a Metropolis-Hastings MCMC kernel replaces the exact sampler. 

### Part 4 — Unknown $\Sigma$ with NIW Prior
Extension to the case where $\Sigma$ is unknown, using a Normal-Inverse-Wishart (NIW) conjugate prior. The MCMC kernel jointly updates $\theta$ and $\Sigma$ via:
- Random walk proposals on $\theta$
- Cholesky-based proposals on $\Sigma$
