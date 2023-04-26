---
layout: post
title: "Creating Sparse Adjacency Matrix from Group Membership with igraph - R Programming"
subtitle: "Block-diagonal matrix with data.table package"
tags: [blog, R, Matrix, sparseMatrix, igraph, block-diagonal, network]
image: /img/brain-neural.png
---

It took me days to come up with an efficient solution to create an adjacency matrix from group membership. Consider the following data:

```
  id group
1  1     5
2  2     5
3  3     5
4  4     5
5  5     4
6  6     2
```

`id` is a unique individual identifier. How to create an adjacency matrix from group membership?

My solution requires `igraph` and `data.table` packages. The latter is not a requirement but works faster (especially if the matrix is large). 

Solutions with and without `data.table` can be found below:

```r
library(igraph)
library(Matrix)
library(data.table)

n = 50
id <- 1:n 
group <- sample(1:round(n/10), size = n, replace = T)
df <- data.frame(id, group)
head(df)

# id group
# 1  1     5
# 2  2     5
# 3  3     5
# 4  4     5
# 5  5     4
# 6  6     2




###########################################
#### The first method without data.table
###########################################

edges = data.frame(matrix(ncol = 2, nrow = 0))
for (i in unique(df$group)) {
  edges = rbind(edges, t(combn(df[df$group == i, 'id'], 2)))
}

g = graph_from_edgelist(as.matrix(edges), directed = F)
g = get.adjacency(g)
g[1:20, 1:20]

# 20 x 20 sparse Matrix of class "dgCMatrix"
# 
# [1,] . . . . . . . . . . . 1 . 1 . . . . . .
# [2,] . . 1 . . . . 1 1 . . . . . . 1 . 1 . .
# [3,] . 1 . . . . . 1 1 . . . . . . 1 . 1 . .
# [4,] . . . . 1 1 1 . . . . . 1 . 1 . . . . .
# [5,] . . . 1 . 1 1 . . . . . 1 . 1 . . . . .
# [6,] . . . 1 1 . 1 . . . . . 1 . 1 . . . . .
# [7,] . . . 1 1 1 . . . . . . 1 . 1 . . . . .
# [8,] . 1 1 . . . . . 1 . . . . . . 1 . 1 . .
# [9,] . 1 1 . . . . 1 . . . . . . . 1 . 1 . .
# [10,] . . . . . . . . . . 1 . . . . . . . . .
# [11,] . . . . . . . . . 1 . . . . . . . . . .
# [12,] 1 . . . . . . . . . . . . 1 . . . . . .
# [13,] . . . 1 1 1 1 . . . . . . . 1 . . . . .
# [14,] 1 . . . . . . . . . . 1 . . . . . . . .
# [15,] . . . 1 1 1 1 . . . . . 1 . . . . . . .
# [16,] . 1 1 . . . . 1 1 . . . . . . . . 1 . .
# [17,] . . . . . . . . . . . . . . . . . . 1 1
# [18,] . 1 1 . . . . 1 1 . . . . . . 1 . . . .
# [19,] . . . . . . . . . . . . . . . . 1 . . 1
# [20,] . . . . . . . . . . . . . . . . 1 . 1 .


###########################################
#### Second method with data.table (FASTER)
###########################################

city_list = unique(df$group)
edges2 = list()
for (i in 1:length(city_list)) {
  edges2[[i]] = data.table(t(combn(df[df$group == city_list[i], 'id'], 2)))
}
edges2 = rbindlist(edges2)


g2 = graph_from_edgelist(as.matrix(edges2), directed = F)
g2 = get.adjacency(g2)
g2[1:20, 1:20]

# 20 x 20 sparse Matrix of class "dgCMatrix"
# 
# [1,] . 1 1 1 . . . . 1 1 . . 1 . . 1 1 . . .
# [2,] 1 . 1 1 . . . . 1 1 . . 1 . . 1 1 . . .
# [3,] 1 1 . 1 . . . . 1 1 . . 1 . . 1 1 . . .
# [4,] 1 1 1 . . . . . 1 1 . . 1 . . 1 1 . . .
# [5,] . . . . . . . . . . . . . . . . . . . 1
# [6,] . . . . . . . 1 . . . . . 1 1 . . 1 1 .
# [7,] . . . . . . . . . . 1 1 . . . . . . . .
# [8,] . . . . . 1 . . . . . . . 1 1 . . 1 1 .
# [9,] 1 1 1 1 . . . . . 1 . . 1 . . 1 1 . . .
# [10,] 1 1 1 1 . . . . 1 . . . 1 . . 1 1 . . .
# [11,] . . . . . . 1 . . . . 1 . . . . . . . .
# [12,] . . . . . . 1 . . . 1 . . . . . . . . .
# [13,] 1 1 1 1 . . . . 1 1 . . . . . 1 1 . . .
# [14,] . . . . . 1 . 1 . . . . . . 1 . . 1 1 .
# [15,] . . . . . 1 . 1 . . . . . 1 . . . 1 1 .
# [16,] 1 1 1 1 . . . . 1 1 . . 1 . . . 1 . . .
# [17,] 1 1 1 1 . . . . 1 1 . . 1 . . 1 . . . .
# [18,] . . . . . 1 . 1 . . . . . 1 1 . . . 1 .
# [19,] . . . . . 1 . 1 . . . . . 1 1 . . 1 . .
# [20,] . . . . 1 . . . . . . . . . . . . . . .
```
