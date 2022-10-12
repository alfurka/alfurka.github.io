---
layout: post
title: "Solution to Memory Leak in R with callr package"
subtitle: An example calling other packages in callr
tags: [memory-leak, memory, R, callr, dplyr]
image: /img/hello_world.jpeg
---

I have been working with huge samples recently. When you work with large samples, memory leak is a common problem. I have been extensively using garbage collector, but it is not helping much. So, you need to write your codes efficiently.

No matter what you do, there are limits. I have been struggling with one of them. I resolved that problem using callr. It will change the way I code with R dramatically from now on.

I often prefer to learn these packages with examples, but I could not find any. So, I here share an example for people who may need it.

```r
p = callr::r_bg(fun = function(n){
  library(dplyr)
  library(data.table)
  df = data.frame(matrix(data = rep(1, n), ncol = 10))
  write.csv(x = df, file = 'xxxxx.csv')
  write.csv(x = data.frame(matrix(data = rep(1, n), ncol = 10)), file = 'xxxxx2.csv')
  df2 = df %>% summarise(x = sum(X1))
  df3 = fread('xxxxx.csv')
  fwrite(df3, file = 'x.csv')
}, args = list(n = 10000000))

p$wait()


print('finished')
```

I use `r_bg` function here to run this process in the background. There are some features of the example above that I needed in my real work. 

- It is possible to load libraries inside the function. 
  - You do not need to do it. You can simply, for example, call functions as follows: `data.table::fread()`. I wanted to do this in this way because I wanted to the pipes of the `dplyr` package (`%>%`). 
- You can pass arguments to the function easily. That means, for instance, you can run this code inside a for loop. 
- You can wait for background process to be finished. 
  - You can also have multiple background works. See [here](https://callr.r-lib.org/#multiple-background-r-processes-and-poll). 
