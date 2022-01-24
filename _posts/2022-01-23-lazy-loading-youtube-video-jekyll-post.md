---
layout: post
title: "Lazy (Deferred) Loading YouTube Videos in Jekyll Posts"
subtitle: A JavaScript for Easy Embedding Youtube Videos 
tags: [lazy-load,youtube,video,jekyll]
image: /img/avatar-icon.png
---

I noticed that my Jekyll posts with YouTube videos open very slow. I checked `pagespeed.web.dev` and got 30/100 score for video-embedded posts. Using the script in this [repository](https://github.com/groupboard/ytdefer), I created a simple method that easily embeds YouTube videos into Jekyll posts. With this lazy-loading script, my posts get 71/100 score. It is easy to implement and use. 

### How to use?

Files and explanations are available in [this repository](https://github.com/alfurka/jekyll-embed-youtube-lazy-load). 

### Remarks 
- Note that you need to change the first line if `ytdefer.min.js` is saved in a different path.
- You can change `div` style for embedding size/style.
