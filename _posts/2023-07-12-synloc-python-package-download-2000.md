---
layout: post
title: "`synloc` Surpasses 2k Downloads"
subtitle: "`synloc` downloads have crossed the 2000 mark"
tags: [NEWS, python, synloc, synthetic, package, code]
image: /img/data.png
---

In a [post](https://alfurka.github.io/2022-10-04-generate-synthetic-data-with-nearest-neighbor-algorithm/) last year, I unveiled `synloc`, my Python package designed for sequential and local estimation of distributions to generate synthetic data from a sample. The algorithm that powers synloc is flexible enough to integrate with both parametric and nonparametric distributions, and can be easily installed via PyPI with the command `pip install synloc`.

It recently came to my attention that the `synloc` package has achieved a milestone: it has been downloaded over 2000 times! This accomplishment is especially noteworthy given the package is still in its pre-alpha stage and requires further refinement. 

The spike in downloads was an unexpected but pleasant surprise. I strongly believe that the algorithmic innovations in `synloc` have helped it carve out its unique place in the landscape of Python packages, particularly due to its superiority over the `synthpop` package in R. 

Notably, `synloc` excels in its ability to naturally trim outliers in datasets, a feature that enhances the robustness of any analysis. Furthermore, I have demonstrated its proficiency in replicating nonlinear and multi-modal distributions in [my paper](https://arxiv.org/abs/2210.00884).
