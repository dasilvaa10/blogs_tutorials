---
layout: post
title: "X'X, covariance, correlation, and cosine matrices"
date: 2018-10-24
excerpt: "Caclulations for a few different association matrices"
tags: [correlation, covariance, cosine, sum of sqaures]
comments: false
---



Ever been in a scenario where you needed to come up with pairwise correlation, covariance, or cosine matrices for data on the fly without the help of a function? Probably not. Even so, it's worth taking a look at how these matrices could be calculated as there are some neat commonalities in their respective calculations. Playing a role in it all is the <a href="https://www.codecogs.com/eqnedit.php?latex=X^{T}X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X^{T}X" title="X^{T}X" /></a> matrix, an old friend we met <a href="https://dasilvaa10.github.io//b1/"><b> a few posts back</b></a> when illustrating multiple regression from an algebraic viewpoint. Here, you can think of <a href="https://www.codecogs.com/eqnedit.php?latex=X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X" title="X" /></a> as some data you've collected where every column is a different measure and every row a different subject.

-   Taking the data <a href="https://www.codecogs.com/eqnedit.php?latex=X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X" title="X" /></a> as is and multiplying its transpose by itself <a href="https://www.codecogs.com/eqnedit.php?latex=X^{T}X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X^{T}X" title="X^{T}X" /></a> results in the sum of squares cross products matrix (SSCP) where SS fall on the diagonal and cross proudcts on the off diagonal.

-   Centering <a href="https://www.codecogs.com/eqnedit.php?latex=X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X" title="X" /></a>, multiplying its transpose by itself <a href="https://www.codecogs.com/eqnedit.php?latex=X^{T}X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X^{T}X" title="X^{T}X" /></a> and dividing by n - 1 (where n = \# of rows in X) results in the variance covariance matrix with variances on the diagonal and covariances on the off diagonal.

-   Standardizing <a href="https://www.codecogs.com/eqnedit.php?latex=X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X" title="X" /></a>, multiplying its transpose by itself <a href="https://www.codecogs.com/eqnedit.php?latex=X^{T}X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X^{T}X" title="X^{T}X" /></a> and dividing by n - 1 (where n = \# of rows in X) results in the pearson correlation between variable pairs.

-   Unit-scaling <a href="https://www.codecogs.com/eqnedit.php?latex=X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X" title="X" /></a> and multiplying its transpose by itself <a href="https://www.codecogs.com/eqnedit.php?latex=X^{T}X" target="_blank"><img src="https://latex.codecogs.com/gif.latex?X^{T}X" title="X^{T}X" /></a> results in the cosine similarity between variable pairs.

Below, we will illustrate what was just discussed by creating a function that outputs one of the aforementiond matrices based on user input.

### **Our function**

``` r
my_fun<-function(x,type =c("ss","cov","cor","cosine")){
  type<-match.arg(type)
  out<-matrix(rep(0,dim(x)[2]*dim(x)[2]),nrow=dim(x)[2])
  if (type == "cov"){
    x<-scale(x,center=TRUE,scale=FALSE)
    out<-t(x)%*%(x)/(nrow(x)-1)
  } else if (type =="cor"){
    x<-scale(x,center=TRUE,scale=TRUE)
    out<-t(x)%*%(x)/(nrow(x)-1)
  } else if (type == "cosine"){
    x<-apply(x, 2,function(x) x / sqrt(sum(x^2)) )
    out<-t(x)%*%(x)
  } else {
     out<-t(x)%*%(x)
  }
  return(out)   
}
```

### **Create some data**

Now, that we have our function let's make some data to apply it to.

``` r
dat<-cbind(replicate(4,sample(1:20,100,rep=TRUE)))
```

### **SSCP**

We can now compare the output of our function to the complement functions in base R. Starting with the <a href="https://www.codecogs.com/eqnedit.php?latex=SSCP" target="_blank"><img src="https://latex.codecogs.com/gif.latex?SSCP" title="SSCP" /></a> matrix.

``` r
crossprod(dat)
```

    ##       [,1]  [,2]  [,3]  [,4]
    ## [1,] 12899 10736 11949 10116
    ## [2,] 10736 13914 11880 10378
    ## [3,] 11949 11880 17581 12463
    ## [4,] 10116 10378 12463 13222

``` r
my_fun(dat, type = "ss")
```

    ##       [,1]  [,2]  [,3]  [,4]
    ## [1,] 12899 10736 11949 10116
    ## [2,] 10736 13914 11880 10378
    ## [3,] 11949 11880 17581 12463
    ## [4,] 10116 10378 12463 13222

### **Covariance**

``` r
cov(dat)
```

    ##            [,1]       [,2]      [,3]       [,4]
    ## [1,] 31.8920202  6.1553535  2.157273  0.4909091
    ## [2,]  6.1553535 34.2145455 -3.223636 -0.8808081
    ## [3,]  2.1572727 -3.2236364 34.785758  3.3858586
    ## [4,]  0.4909091 -0.8808081  3.385859 28.4646465

``` r
my_fun(dat, type = "cov")
```

    ##            [,1]       [,2]      [,3]       [,4]
    ## [1,] 31.8920202  6.1553535  2.157273  0.4909091
    ## [2,]  6.1553535 34.2145455 -3.223636 -0.8808081
    ## [3,]  2.1572727 -3.2236364 34.785758  3.3858586
    ## [4,]  0.4909091 -0.8808081  3.385859 28.4646465

### **Correlation**

``` r
cor(dat)
```

    ##            [,1]        [,2]        [,3]        [,4]
    ## [1,] 1.00000000  0.18634022  0.06476842  0.01629323
    ## [2,] 0.18634022  1.00000000 -0.09344153 -0.02822429
    ## [3,] 0.06476842 -0.09344153  1.00000000  0.10760072
    ## [4,] 0.01629323 -0.02822429  0.10760072  1.00000000

``` r
my_fun(dat, type = "cor")
```

    ##            [,1]        [,2]        [,3]        [,4]
    ## [1,] 1.00000000  0.18634022  0.06476842  0.01629323
    ## [2,] 0.18634022  1.00000000 -0.09344153 -0.02822429
    ## [3,] 0.06476842 -0.09344153  1.00000000  0.10760072
    ## [4,] 0.01629323 -0.02822429  0.10760072  1.00000000

### **Cosine similarity**

We'll load the library "philentropy" to check our work here as it contains many useful distance functions. Note that we are transposing our data as the default behavior of this function is to make pairwise comparisons of all *rows*.

``` r
library(philentropy)
```

    ## Warning: package 'philentropy' was built under R version 3.4.3

``` r
distance(t(dat), method = "cosine")
```

    ##           v1        v2        v3        v4
    ## v1 1.0000000 0.8013800 0.7934723 0.7746084
    ## v2 0.8013800 1.0000000 0.7595715 0.7651368
    ## v3 0.7934723 0.7595715 1.0000000 0.8174331
    ## v4 0.7746084 0.7651368 0.8174331 1.0000000

``` r
my_fun(dat, type = "cosine")
```

    ##           [,1]      [,2]      [,3]      [,4]
    ## [1,] 1.0000000 0.8013800 0.7934723 0.7746084
    ## [2,] 0.8013800 1.0000000 0.7595715 0.7651368
    ## [3,] 0.7934723 0.7595715 1.0000000 0.8174331
    ## [4,] 0.7746084 0.7651368 0.8174331 1.0000000

Now you're prepared if you're ever in a weird scenario needing to come up with these measures on the fly!  More importantly, you now hopefully have a more intiuitive understanding of how a few types of pariwise associations could be derived.