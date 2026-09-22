---
layout: post
title: "Lost in Aggregation: When Geographic Centroids Mislead"
subtitle: "New Economic Record paper on spatial measurement error"
tags: [NEWS, blog, publication, econometrics, spatial-data, measurement-error, australia]
image: /img/data.png
---

My new paper with [Henry Wen](https://onlinelibrary.wiley.com/doi/10.1111/1475-4932.70072), **“Lost in Aggregation: Quantifying Measurement Error from Geographic Centroids,”** has just been published open access in *Economic Record*. The question behind it had stayed with me since my PhD. In [work with Alicia Rambaldi and James Hansen](https://iariw.org/wp-content/uploads/2024/08/4B-2Rambaldi.pdf), we constructed spatial variables such as distances to amenities using SA1 centroids because exact locations are generally unavailable in linked microdata. Later, Henry, whom I had previously taught in econometrics, contacted me about getting involved in research. We started with a simple question: **how much measurement error do these centroid-based variables actually create?**

Answering that question turned into a much larger data project than I initially expected. We linked NSW property transactions to G-NAF address records after standardising addresses, achieving a match rate of about 95%, and combined these data with Australian statistical boundaries, OpenStreetMap amenities and NSW zoning information. When I eventually prepared the [replication materials](https://github.com/alfurka/quantify-measurement-error-replication-codes), I realised how much data engineering had gone into what initially sounded like a simple benchmarking exercise. Much of that work was done by Henry.

Why does this matter? Australian microdatasets such as **HILDA, PLIDA, the 45 and Up Study and BLADE** commonly provide geographic identifiers rather than exact coordinates. If researchers construct a distance, accessibility measure or local exposure using the centroid of an SA1, SA2 or postcode, the resulting error is not necessarily classical measurement error. A geometric centroid may not represent where people or properties are actually located within an area, and the analytical sample itself may be spatially selected relative to the underlying population. The measurement error can therefore be correlated with the true spatial variable, so the usual intuition that measurement error simply attenuates estimates towards zero does not generally apply.

That is exactly what we find. SA1 centroids reproduce coefficients for a smooth measure such as distance to Sydney CBD quite closely, but even SA1 can noticeably change coefficients for highly local measures such as the number of shops within 1 km. At SA2, the shop-count coefficient differences are much larger in our benchmark models, and across the wider analysis the bias can go in either direction. The practical message is therefore not that centroid-based variables should never be used. Rather, researchers should think carefully about the scale of the released geography relative to the spatial variable being constructed, report sensitivity across available geographies and specifications, consider larger buffers for very local measures, and use finer geography or controlled location linkage where it is important and feasible.

I also had a very positive experience with *Economic Record*. The editors and referees were constructive, and the second round in particular led us to substantially improve the paper. It was also an excellent experience working with Henry, from the first version of the idea through the considerable data work and replication package.

**Links:** [Open-access paper](https://doi.org/10.1111/1475-4932.70072) | [Replication materials](https://github.com/alfurka/quantify-measurement-error-replication-codes)
