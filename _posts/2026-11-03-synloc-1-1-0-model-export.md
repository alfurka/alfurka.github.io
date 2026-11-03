---
layout: post
title: "Synloc 1.1.0: Export a Fitted Model, Sample Anywhere"
subtitle: "parameter-only model export for clusterCov and LocalCov"
date: 2026-11-03
tags: [synloc, python, synthetic-data, release, package, blog]
image: /img/data.png
---

I released **synloc 1.1.0**, and the headline feature is **model export**: fit a synthetic-data model once, export only its parameters, and generate synthetic data in a completely separate environment — without the original records.

This is the workflow I kept wanting for data custodians: fit inside a secure environment, hand over a small parameter file, and let researchers draw synthetic samples elsewhere. The existing `fit()` workflow is unchanged; export is an optional addition.

```bash
pip install --upgrade synloc
```

### Two ways to export

**Cluster models** — `clusterCov` can now fit a reusable Gaussian mixture without generating any synthetic rows:

```python
from synloc import clusterCov, ClusterCovSampler

model = clusterCov(data, n_clusters=20, size_min=20).fit_model()
diagnostics = model.disclosure_diagnostics()  # inspect before export
model.export_model("cluster-model.npz")

# In a separate environment, with just the .npz file:
sampler = ClusterCovSampler.load("cluster-model.npz")
synthetic = sampler.sample(1000, random_state=123)
```

**kNN models** — `LocalCov.prepare_export()` selects anchors using the full dataset, estimates their local distributions, and returns a fixed proposal you can inspect before exporting:

```python
from synloc import LocalCov

model = LocalCov(data, K=100, normalize=True)
proposal = model.prepare_export(n_components=50, random_state=42)
diagnostics = proposal.disclosure_diagnostics()
proposal.export_model("local-model.npz")
```

`n_components=None` uses every row as an anchor; a smaller count selects distinct anchors uniformly. `K` is the neighbourhood size, independent of how many components you export, and `random_state` seeds anchor selection, not later sampling.

### What the file contains

The version-1 NPZ stores only `version`, `column_names`, `weights`, `means`, and `covariances`. No source rows, no cluster memberships, no contributor counts, no original extrema, no pickled estimator. Means and covariances are in **original units** — normalization only affects neighbour or cluster selection, so nothing needs rescaling when loading. Loading validates the format with pickle disabled.

One honest caveat: exported sampling does not clip to source bounds, so generated values can fall outside the original range.

### Inspecting a model before release

`disclosure_diagnostics()` returns one row per component, in the same order as the fitted means:

- `count`: source rows in the component.
- `rank`: rank of the component's centred rows — a low rank means the component is close to a few records.
- `constant_columns`: variables with one shared value in the component.
- `top1_share_<column>` / `top2_share_<column>`: the largest one and two values as shares of the component total (`NaN` for negative values or a zero total).

These diagnostics are computed from the source rows, so keep them confidential; they are not written to the exported file. They apply no thresholds and do not establish privacy — apply your own disclosure rules before releasing a model.

### The privacy fine print

I want to be clear about what this is and is not. The export is a tool for the disclosure-control workflow, not a substitute for it. Loading validates the numerical format, not privacy: means, covariances, weights, overlapping neighbourhoods, and repeated exports can still disclose information. Counts refer to rows, not verified unique people.

### Other 1.1 changes

- `LocalCov` accepts an optional `random_state`, so kNN fitting is reproducible across worker counts; omitting it keeps the legacy behaviour.
- Fitted mixture parameters are read-only, so sampling, export, and diagnostics can't silently desynchronize.
- Tighter input validation (complex values rejected, NumPy integer counts accepted) and covariance checks that can no longer be masked by large variances elsewhere.

### Links

- **Package repository:** [github.com/alfurka/synloc](https://github.com/alfurka/synloc)
- **PyPI package:** [pypi.org/project/synloc](https://pypi.org/project/synloc/)
- **Model export guide:** [alfurka.github.io/synloc/model_export.html](https://alfurka.github.io/synloc/model_export.html)
- **Paper:** [Generating Synthetic Data with Locally Estimated Distributions for Disclosure Control](https://doi.org/10.1111/anzs.70032)

### Related posts

- [Synloc 1.0.0 released](/2026-05-02-synloc-1-0-0/)
- [Experimenting with Gemini CLI: Upgrading Synloc in Three Hours](/2025-06-26-synloc-update-with-gemini-cli/)
- [A Fast Method to Create Synthetic Data with Python](/2022-10-04-generate-synthetic-data-with-nearest-neighbor-algorithm/)

Have a nice day!
