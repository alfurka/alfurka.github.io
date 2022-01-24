---
layout: post
title: "Lazy (Deferred) Loading YouTube Videos in Jekyll Posts"
subtitle: A JavaScript for Easy Embedding Youtube Videos 
tags: [lazy-load,youtube,video,jekyll]
image: /img/avatar-icon.png
---

I noticed that my Jekyll posts with YouTube videos open very slow. I checked `pagespeed.web.dev` and got 30/100 score for video-embedded posts. Using the script in this [repository](https://github.com/groupboard/ytdefer), I created a simple method that easily embeds YouTube videos into Jekyll posts. With this lazy-loading script, my posts get 71/100 score. It is easy to implement and use. 

You can download the files from [my repository](https://github.com/alfurka/jekyll-embed-youtube-lazy-load). 

### How to use?

1. Download [ytdefer.min.js](https://github.com/alfurka/jekyll-embed-youtube-lazy-load/blob/main/ytdefer.min.js) from this repository. 
2. Upload `ytdefer.min.js` to your Jekyll repository (server). JavaScripts files are usually contained `assets` folder.  
3. Download `youtube.html` from [here](https://github.com/alfurka/jekyll-embed-youtube-lazy-load) and upload it to your Jekyll server/repository's folder `_includes`. 
4. Add the following line into your posts where you want to embed the video. 

```
{% include youtube.html id='YOUTUBE-VIDEO-ID' %}
```

Example Jekyll post layout embedding a YouTube video (https://www.youtube.com/watch?v=ZzeF6SducfQ):

```
---
layout: post
title: "Example Post"
---
....
CONTENTS 
....

{% include youtube.html id='ZzeF6SducfQ' %}

....
CONTENTS 
....
```

### Remarks 
- Note that you need to change the first line if `ytdefer.min.js` is saved in a different path.
- You can change `div` style for embedding size/style.
