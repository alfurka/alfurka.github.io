---
layout: post
title: "Reducing Matrix Computation Time in R"
subtitle: Using sparseMatrix from Matrix package 
tags: [R, Matrix, sparseMatrix, Computation, package]
image: /img/data.png
---

In order to increase computation time, I transformed loops into matrix operations in an algorithm. Nevertheless, my matrices were extremely large, and thus computation was slower than I expected. I was using the `%*%` operator in `R` to do matrix multiplication. I found out that it is not possible to achieve dramatically faster computations with other background programming languages (e.g., using `Rcpp`or `JuliaCall`). I tried and failed. 

I soon realized that my large matrices were extremely sparse. It might be obvious to many people but I did not know the `sparse matrix` concept. The sparsity can be used to minimize memory usage and increase computation speed. For example, assume that you have a 1000x1000 matrix consisting of zeros and ones. The ratio of ones is 20 percent. 

```r
n = 1000
set.seed(1)
X = matrix(sample(0:1, size = n * n, 
                  replace = T, 
                  prob = c(0.9,0.1)), 
           ncol = n )
```

You can define that matrix as a `sparse matrix` using `Matrix` package as follows:

```r
X.s = Matrix::Matrix(X, sparse = T)
```

The first thing you will notice that the difference between memory. `X` uses approximately 4Mb memory and `X.s` uses approximately 1.2 Mb memory. If you try to do computation with a sparse matrix, you will see that the computation time dramatically decreases. 

### Results with a sparse matrix

```r
system.time(X.s %*% X.s)
```

```
   user  system elapsed 
  0.111   0.000   0.112
```
### Results with a regular matrix

```r
system.time(X %*% X)
```

```
   user  system elapsed 
  0.998   0.000   0.999 
```

The efficiency gains are even larger if you have more sparse or larger matrices. For example, if we increase the sparsity ratio to 10 percent, the sparse matrix computation reduces by half whereas no change in a regular matrix operation:

```r
n = 1000
set.seed(1)
X = matrix(sample(0:1, size = n * n, 
                  replace = T, 
                  prob = c(0.9,0.1)), 
           ncol = n )
X.s = Matrix::Matrix(X, sparse = T)
```

### Results with a sparse matrix

```r
system.time(X.s %*% X.s)
```

```
   user  system elapsed 
  0.057   0.000   0.058
```

### Results with a regular matrix

```r
system.time(X %*% X)
```

```
   user  system elapsed 
  0.992   0.004   0.996 
```
