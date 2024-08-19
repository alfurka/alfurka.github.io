---
layout: post
title: "Opinion: The Leap from Statistical Models to True AI"
subtitle: "AI Innovation and Model Collapse: A New Perspective Post-ChatGPT"
tags: [ai, LLM, ChatGPT, opinion, blog]
image: /img/hoca.jpg
---

After reading the article “[Beyond Model Collapse: Scaling Up with Synthesized Data Requires Reinforcement](https://arxiv.org/abs/2406.07515),” I felt compelled to write this blog post. I have strong opinions about model collapse in large language models, and my views have dramatically changed since the release of ChatGPT. Prior to ChatGPT, I did not even like the use of the term “artificial intelligence” because they are merely statistical models. While this is still true, there has been a significant leap in what LLMs can do compared to traditional machine learning or deep learning algorithms. Therefore, I now consider large language models, such as Claude, Gemini, and ChatGPT, worthy of being referred to as artificial intelligence. They have my approval. :)

My brain burnt a considerable amount of calories thinking about "model collapse" in terms of improving statistical inference robustness and protecting privacy. Model collapse occurs when model-generated data converge towards the most common outputs in the original data, essentially becoming more generic with each iteration. Prior to ChatGPT, I _believed_—and still believe—that we cannot extrapolate information with machine learning or deep learning algorithms; hence, synthetic data cannot _do miracles_. I have been well aware that models would collapse—converge to the mode(s) of the distributions. This is why I did not consider AI worthy of being referred to as "artificial intelligence" prior to LLMs, as they were merely sophisticated correlation machines that could not generate both **new** and **correct** information/data.

Why does this matter? If these models are doomed to collapse, it implies that we still need humans to create/generate new information so that we can train the AI models.

After ChatGPT, my opinion has changed, and I no longer think language models will necessarily collapse. On many occasions, I have seen large language models being creative and correct. The latter, however, has been assessed by me. The article above actually points out that as long as I am there as a verifier, large language models will not collapse. I have a role as a filter. As stated in the abstract, this is the most popular idea nowadays, but it is also scary because I believe it would not be difficult for another large language model to replace me as a verifier. This opens up a series of questions: At what point do humans become unnecessary in the loop?

I do think that AI can actually expand the boundaries of human knowledge on its own. This will become possible, especially after AIs have the opportunity to experiment. Consider an AI researcher, which is modeled to be an economist. This AI researcher develops a new econometric approach to gain statistical efficiency for a certain problem. It goes to the verifier (another AI) to receive feedback—another AI with expertise in testing such things. Then, it comes up with experiment designs to test whether the ideas work or not. It can simply use existing empirical approaches to test the quality of the new approach. This could be done with real datasets and/or with Monte Carlo simulations using artificial data. Then, it tests and responds with the results to the researcher AI: “It does (not) work. This approach is ... in comparison to ...” If it works, then it means the researcher AI invented a new econometric method. If it does not, it will retry. Based on my exposure to many large language models, I have seen many times that they can invent such things.

To my *limited* understanding, the bottlenecks will be CPUs and energy consumption. These types of feedback loops will be costly both in terms of computation and energy. I hope that in the future, it will NOT be cheaper to run an AI than to pay my salary.

**Update**: After publishing this blog post, I discovered a system that resembles a recursive model: Automated Design of Agentic Systems. There's a [GitHub](https://github.com/ShengranHu/ADAS) repository for implementation and a [paper](https://arxiv.org/abs/2408.08435) that explains the recursive model using an `empirical framework` for evaluation.

