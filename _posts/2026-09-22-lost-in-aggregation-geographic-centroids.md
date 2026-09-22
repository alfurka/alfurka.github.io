---
layout: post
title: "Lost in Aggregation: When Geographic Centroids Mislead"
subtitle: "New Economic Record paper on spatial measurement error"
tags: [NEWS, blog, publication, econometrics, spatial-data, measurement-error, australia]
image: /img/data.png
---

My new paper with [Henry Wen](https://onlinelibrary.wiley.com/doi/10.1111/1475-4932.70072), **"Lost in Aggregation: Quantifying Measurement Error from Geographic Centroids,"** has been published open access in *Economic Record*. The question behind it had stayed with me since my PhD. In [work with Alicia Rambaldi and James Hansen](https://iariw.org/wp-content/uploads/2024/08/4B-2Rambaldi.pdf), we constructed spatial variables such as distances to amenities from SA1 centroids because exact locations are often unavailable in linked Australian microdata. Later, Henry, whom I had previously taught in econometrics, contacted me about getting involved in research. We started with a fairly simple question: how much error do these centroid-based variables actually introduce?

Answering it became a much larger data exercise than I expected. We linked NSW property transactions to G-NAF after standardising the addresses, with a match rate of about 95%, and combined the linked transactions with ABS statistical boundaries, OpenStreetMap amenities and NSW zoning data. When I prepared the [replication materials](https://github.com/alfurka/quantify-measurement-error-replication-codes), I realised how much work had gone into building the final dataset. Henry did a large part of that work.

### Where does the centroid error come from?

A geographic centroid is not automatically a bad proxy. How well it works depends on the geography, the spatial variable being constructed, where the relevant entities are actually located inside the area, and which of those entities end up in the analytical sample.

One useful way to see this is to separate two sources of systematic displacement. For area $g$, let $\mathbf{c}_g$ be the geometric centroid, $\bar{\mathbf{p}}_g$ the centroid of the relevant underlying entities, such as the residential address stock in G-NAF, and $\bar{\mathbf{s}}_g$ the centroid of the observed sample transactions. Then

$$
\bar{\mathbf{s}}_g-\mathbf{c}_g
=
\underbrace{\left(\bar{\mathbf{p}}_g-\mathbf{c}_g\right)}_{\text{entity-distribution component}}
+
\underbrace{\left(\bar{\mathbf{s}}_g-\bar{\mathbf{p}}_g\right)}_{\text{sample-selection component}}.
$$

The first component appears because houses, apartments or people are rarely spread uniformly over a statistical area. The second appears when the observations in the study are not spatially representative of that underlying stock. The distinction matters because two studies using the same SA1 boundaries can have different centroid errors simply because their samples are distributed differently within those SA1s.

Our results also show why there is no simple ranking in which every coarser centroid is bad and every finer centroid is safe. For distance to the Sydney CBD, SA1 centroid estimates are extremely close to the exact-coordinate benchmark in our application. For the much more local measure of shops within 1 km, SA1 changes the coefficient by roughly 7 to 13 percent depending on the specification. SA2 is not automatically a bad proxy either. It remains quite close to the exact benchmark for some smooth variables and specifications, although the differences can become very large for highly local variables such as shops within 1 km. Postcode results are sometimes better and sometimes worse than SA2, so the ordering is not mechanical. Increasing the shop buffer also changes the picture considerably. My reading of our application is that SA1 is fairly reassuring, but I would not turn that into a general rule. The variable and the empirical specification matter.

### Why the error does not have to attenuate the coefficient

The usual textbook intuition says measurement error pushes a coefficient towards zero. That result needs the measurement error to satisfy strong conditions. Spatial measurement error has little reason to satisfy them.

The point becomes clear after partialling out the controls and fixed effects. Let $x$ be the exact spatial variable, $\widetilde{x}=x+e$ its centroid proxy, and $y$ the residualised outcome. The coefficient gap can be written as

$$
\widehat{\beta}_{\text{centroid}}-\widehat{\beta}_{\text{exact}}
=
\frac{\operatorname{Cov}(e,y)}{\operatorname{Var}(\widetilde{x})}
+
\operatorname{Cov}(x,y)
\left[
\frac{1}{\operatorname{Var}(\widetilde{x})}
-
\frac{1}{\operatorname{Var}(x)}
\right].
$$

The first term is especially important. If the centroid error is related to the residualised outcome, the bias can go in either direction. In spatial applications I would be reluctant to assume that this covariance is zero without checking it. Residential locations, density, land use, accessibility and the composition of the analytical sample all have spatial structure.

For applied work with Australian microdata, I think a simple sensitivity exercise is worth doing whenever possible. Construct the same variable using the available SA1, SA2 and postcode centroids, estimate the specification you actually intend to report, including its fixed effects, and compare the coefficients. Stability across those alternatives is reassuring. Large movements tell you that the geographic approximation is doing more work than you may want. If finer geography or controlled exact-location linkage is available, it can then be used to investigate the problem directly.

I also had a very positive experience with *Economic Record*. The editors and referees were constructive, and the second round led us to substantially improve the paper. It was also a very good experience working with Henry, from the first version of the idea through the data work and the replication package.

[Open-access paper](https://doi.org/10.1111/1475-4932.70072) | [Replication materials](https://github.com/alfurka/quantify-measurement-error-replication-codes)
