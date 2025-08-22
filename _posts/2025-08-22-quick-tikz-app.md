---
layout: post
title: "A tiny client-side TikZ previewer with TikZJax"
subtitle: "Quick Tikz Preview Tool"
date: 2025-08-22
tags: [latex, tikz, tikzjax, github-pages, tooling, ai, chatgpt, qwen, gemini, blog]
---

I like using TikZ in my papers. My workflow is: draft the diagram with ChatGPT, fix bugs by hand, and paste the final LaTeX. The slow part is compiling just to see if a small change worked. I wanted a fast visual check.

I noticed **TikZJax**, which renders TikZ to SVG in the browser. Its demo didn’t work when I checked, so I used this opportunity to experiment with a few coding agents to create a self-contained static html page that renders locally and on GitHub Pages.

## The quick build

Goal: a **static HTML** page that takes TikZ code, renders it client-side, and lets me download the SVG.

## Agents I tried

- **Qwen Coder Plus (CLI)** and **Gemini 2.5 Pro (CLI)** both failed the one-shot build. They also defaulted to CDN assets instead of saving JS/CSS/fonts locally. After guiding them and manually downloading files, I finished the page.
- **ChatGPT (GPT-5 Thinking)** produced a rough HTML skeleton, but not a clean one-shot solution.

Takeaway: these models help, but they still need precise instructions and manual fixes. For small tasks I expect to keep using Qwen CLI due to its speed and the fact that i prefer to run something on my computer. I only use chatgpt via my browser. Gemini unfortunatelly stuck with thinking quite often and does not generate the most brilliant outcome. 

Most importantly, to my surprise, coding agents still lack autonomy. Well, i hope they will remain at this level quite long. 

## How to use [Quick Tikz](https://alfurka.github.io/quick-tikz/)

1. Open the page.  
2. Paste TikZ (and package directives supported by TikZJax).  
3. Click **Render**.  
4. If it looks right, click **Save as SVG** and drop it into the paper.

### Notes

- This is for **quick preview**, not full LaTeX builds. Unsupported packages won’t render.  

I hope you find it useful. Have a nice day! 
