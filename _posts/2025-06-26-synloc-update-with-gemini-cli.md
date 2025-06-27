---
layout: post
title: "Experimenting with Gemini CLI: Upgrading Synloc in Three Hours"
subtitle: "Synloc 0.2 Update: Less dependencies and Parallisation"
tags: [synloc, gemini-cli, python, synthetic-data, ai, blog]
image: /img/avatar-icon.png
---

I had been meaning to update my Python package ***synloc*** for a while, aiming for two main improvements: **parallelisation** and **fewer dependencies**.

I hadn't tackled it because of my workload, but then Google released **[Gemini CLI](https://github.com/google-gemini/gemini-cli)**. It's basically Google’s answer to Claude Code and a competitor to other AI coding agents like Cursor and Copilot.

Google made it free with very generous limits, so before the free tier disappears I decided to experiment with it. I’d actually been planning to purchase Claude Code or Cursor for a while but wasn’t sure. I’ve used Copilot since day one and rely on its autocompletion,even in my LaTeX documents. In fact, Copilot handled most of the documentation and simple tasks in the first *synloc* release, which I released before ChatGPT. So, I’m very open to AI editors; I just haven’t had many projects where I could use them. Most of my coding happens inside the Australian Bureau of Statistics cloud environment called **DataLab**, which is an offline server, so I can’t harness these AI tools as much as I’d like.

As a test, I used Gemini CLI to update *synloc*. In one word, it was **efficient**. I finished everything in three hours. I hit the request limit fairly quickly and had to do some manual work in the last 30 minutes or so.

### synloc 0.2 highlights

1. **Dependency reduction**
   Two rarely maintained libraries were replaced with lightweight, actively supported alternatives. Installation is faster and version conflicts are less likely.

2. **Native parallelism**
   Core resampling and covariance‑estimation routines now accept an `n_jobs` argument, distributing workloads across available CPU cores.Quick demo:

```python
from synloc import LocalCov

# use every core
resampler = LocalCov(data=df, K=30, n_jobs=-1)
synthetic = resampler.fit()

# or with a very specific number of cpu: 4

resampler = LocalCov(data=df, K=30, n_jobs=4)
synthetic = resampler.fit()
```

3. **Constrained k‑means clustering**
   Because no well‑maintained Python implementation exists, I built a custom wrapper around *scikit‑learn*’s k‑means to enforce cluster‑size constraints—essential for balanced synthetic samples.

The third point is especially important. Constrained k‑means is an interesting optimisation problem. When I released *synloc* v0.1, the only option I found was the [`k‑means‑constrained`](https://pypi.org/project/k-means-constrained/) package. It’s efficient and convenient, but it drags in a lot of dependencies.

I asked Gemini CLI to create a simpler version of the algorithm. It failed :)

So I moved the problem to **AI Studio**, provided detailed instructions, and, with Gemini 2.5 Pro, eventually got a compact, dependency‑free implementation. Gemini CLI struggled with this complex task, and while the optimisation problem is tough, writing a straightforward algorithm with clear steps shouldn’t be *that* hard. I was a bit disappointed.

### Final thoughts

I couldn’t have updated the package without Gemini CLI. Doing it all manually would have taken a few days. Even the build‑test‑publish pipeline, which I’m still learning, was smoother with Gemini’s guidance.

Next, I’m thinking of trying Claude Code; its pay‑as‑you‑go model suits my sporadic coding sessions.

This post isn’t sponsored—just sharing my experience. Have a nice day!


Future work will examine GPU acceleration and broader support for non-parametric distributions. For now, these enhancements position **Synloc** as a more efficient and maintainable solution for synthetic-data generation.

[1]: https://github.com/google-gemini/gemini-cli?utm_source=chatgpt.com "GitHub - google-gemini/gemini-cli: An open-source AI agent that brings ..."
