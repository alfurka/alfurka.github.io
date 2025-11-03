---
layout: post
title: "Working Paper: The Impact of LLMs on Academic Writing"
subtitle: "Evidence from arXiv, 2018–2024"
date: 2025-11-03
tags: [llm, academic-writing, arxiv, readability, lexical-diversity, science-of-science, computer-science, policy, equity, research, ai, chatgpt, blog]
---

I’m excited to share a new working paper co-authored with **Burak Dalaman** and [Nathan Kettlewell](https://sites.google.com/site/nrkettlewell/). We study how large language models (LLMs) changes academic writing, with a simple idea that unlocks a clean comparison between **native** and **non-native** English speaking authors. While studying the impact of LLMs, we also happen to use LLMs to infer authors’ likely English language background from names. This lets us split abstracts into **native-authored** and **non-native-authored** groups and track differences over time.

- Data: **~1.25M arXiv abstracts** by **~1.04M authors**, **2018–2024**.  
- Outcomes: **lexical diversity** (e.g., type-token ratio, TTR) and **writing complexity** (readability metrics).  
- Shock window: the **late-2022** release of ChatGPT.

## What we find

1. **Bigger shifts for non-native authors.** After late 2022, non-native-authored abstracts move **more** on both lexical diversity and writing complexity than native-authored ones.  
2. **Gap closes.** For example, The the lexical diversity (TTR) gap between groups narrows markedly through 2023–2024.

![Lexical diversity over time (TTR) — the native vs non-native gap narrows after late 2022.](/img/ttr_tsplot.webp)
*Example: TTR time series shows convergence in lexical diversity post-ChatGPT.*

3. **Computer science leads.** Changes are **largest in CS** categories. Our interpretation is: CS authors tend to adopt new AI tooling faster. If true, broader fields may show delayed effects as adoption diffuses.  
4. **Adoption is slow.** Even for a relatively easy use case (AI-assisted phrasing and editing) uptake among academics looks **gradual**, not instantaneous.

We will update the data and the reuslts soon. You can find the paper [here](https://www.iza.org/publications/dp/18215/bridging-language-barriers-the-impact-of-large-language-models-on-academic-writing). Let us know if you have any comments or questions: `alifurkan.kalay@mq.edu.au`

Cheers! 
