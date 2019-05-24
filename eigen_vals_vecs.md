---
layout: post
title: "Where do eigenvalues and vectors come from?"
date: 2018-10-18
excerpt: "An explanation of how eigenvalues and eigenvectors are derived"
tags: [pca,fa,matrix algebra, eigen]
comments: false
---

Social science researchers usually hear about eigenvalues and eigenvectors somewhere in their training, typically in the context dimension reduction (principal component analysis) or personality survey creation (factor analysis). In my experience, eigenvalues and eigenvectors are often described minimally as "some property a matrix has" which isn't terribly useful information as matrices have all sorts of properties. As such, I think people are often left wondering where in the world these things come from. The point of this post is to take a simple example and demonstrate how eigenvalues and eigen vectors are derived.

### **Definition:**

In short, by way of stretching or compressing, eigenvectors are the axes along which a linear transformation acts. The factors by which this transformation occurs are eigenvalues.<a href="https://www.codecogs.com/eqnedit.php?latex=^1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?^1" title="^1" /></a> 

<a href="https://www.codecogs.com/eqnedit.php?latex=A" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A" title="A" /></a> is a nXn matrix (often a correlation or variance-covariance matrix), <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda}" title="{\lambda}" /></a> is an *eigenvalue* of <a href="https://www.codecogs.com/eqnedit.php?latex=A" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A" title="A" /></a>, and <a href="https://www.codecogs.com/eqnedit.php?latex=x" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x" title="x" /></a> is a vector (*eigenvector*) of<a href="https://www.codecogs.com/eqnedit.php?latex=A" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A" title="A" /></a> in relation to <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda}" title="{\lambda}" /></a> whereby:

<a href="https://www.codecogs.com/eqnedit.php?latex=Ax" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Ax" title="Ax" /></a> = <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda&space;x}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda&space;x}" title="{\lambda x}" /></a>

We can reform the above equation as:

<a href="https://www.codecogs.com/eqnedit.php?latex=(A&space;-&space;{\lambda&space;I})x&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?(A&space;-&space;{\lambda&space;I})x&space;=&space;0" title="(A - {\lambda I})x = 0" /></a> where <a href="https://www.codecogs.com/eqnedit.php?latex=I" target="_blank"><img src="https://latex.codecogs.com/gif.latex?I" title="I" /></a> equals a nxn identity matrix

### **Setup**

For this example, we will work with the following 2x2 matrix:

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;4&space;&&space;6\\&space;3&space;&&space;1&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;4&space;&&space;6\\&space;3&space;&&space;1&space;\end{bmatrix}" title="\begin{bmatrix} 4 & 6\\ 3 & 1 \end{bmatrix}" /></a>

As we are working with a 2x2 matrix the <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda&space;I}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda&space;I}" title="{\lambda I}" /></a> will take the following form:

<a href="https://www.codecogs.com/eqnedit.php?latex={\lambda&space;I}&space;=&space;\begin{bmatrix}&space;{\lambda}&space;&&space;0&space;\\&space;0&space;&&space;{\lambda}&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda&space;I}&space;=&space;\begin{bmatrix}&space;{\lambda}&space;&&space;0&space;\\&space;0&space;&&space;{\lambda}&space;\end{bmatrix}" title="{\lambda I} = \begin{bmatrix} {\lambda} & 0 \\ 0 & {\lambda} \end{bmatrix}" /></a>

### **Finding Eigenvalues**

We'll take our matrix<a href="https://www.codecogs.com/eqnedit.php?latex=A" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A" title="A" /></a> and plug it in to the framework above:

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;4&space;&6&space;\\&space;3&1&space;\end{bmatrix}&space;-&space;\begin{bmatrix}&space;{\lambda}&space;&0&space;\\&space;0&{\lambda}&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;4&space;-&space;{\lambda}&space;&&space;6&space;\\&space;3&space;&&space;1&space;-&space;{\lambda}&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;4&space;&6&space;\\&space;3&1&space;\end{bmatrix}&space;-&space;\begin{bmatrix}&space;{\lambda}&space;&0&space;\\&space;0&{\lambda}&space;\end{bmatrix}&space;=&space;\begin{bmatrix}&space;4&space;-&space;{\lambda}&space;&&space;6&space;\\&space;3&space;&&space;1&space;-&space;{\lambda}&space;\end{bmatrix}" title="\begin{bmatrix} 4 &6 \\ 3&1 \end{bmatrix} - \begin{bmatrix} {\lambda} &0 \\ 0&{\lambda} \end{bmatrix} = \begin{bmatrix} 4 - {\lambda} & 6 \\ 3 & 1 - {\lambda} \end{bmatrix}" /></a>

Next, we need to find the determinant of <a href="https://www.codecogs.com/eqnedit.php?latex=A&space;-&space;{\lambda&space;I}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A&space;-&space;{\lambda&space;I}" title="A - {\lambda I}" /></a> and set it equal to 0:

<a href="https://www.codecogs.com/eqnedit.php?latex=(4&space;-&space;{\lambda&space;})(1-{\lambda})&space;-18=" target="_blank"><img src="https://latex.codecogs.com/gif.latex?(4&space;-&space;{\lambda&space;})(1-{\lambda})&space;-18=" title="(4 - {\lambda })(1-{\lambda}) -18=" /></a> <br />

<a href="https://www.codecogs.com/eqnedit.php?latex={\lambda&space;}^2&space;-5&space;{\lambda}&space;-&space;14&space;=" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda&space;}^2&space;-5&space;{\lambda}&space;-&space;14&space;=" title="{\lambda }^2 -5 {\lambda} - 14 =" /></a> <br />

<a href="https://www.codecogs.com/eqnedit.php?latex=({\lambda&space;}&space;&plus;&space;2)({\lambda}&space;-&space;7)&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?({\lambda&space;}&space;&plus;&space;2)({\lambda}&space;-&space;7)&space;=&space;0" title="({\lambda } + 2)({\lambda} - 7) = 0" /></a>

So, <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda&space;}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda&space;}" title="{\lambda }" /></a> OR our *eigenvalues* = -2,7

### **Finding our first eigenvector**

We'll start by returning to the <a href="https://www.codecogs.com/eqnedit.php?latex=A&space;-&space;{\lambda&space;I&space;}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A&space;-&space;{\lambda&space;I&space;}" title="A - {\lambda I }" /></a> matrix we created early and plug in the corresponding eigenvalue

Let's being with <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda}" title="{\lambda}" /></a> = 7

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;-3&space;&&space;6&space;\\&space;0&&space;0&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;-3&space;&&space;6&space;\\&space;0&&space;0&space;\end{bmatrix}" title="\begin{bmatrix} -3 & 6 \\ 0& 0 \end{bmatrix}" /></a>

We can use a bit of row reduction to reduce this matrix. It looks like we can perform the following operation: <br />

<a href="https://www.codecogs.com/eqnedit.php?latex=1(R_1)&space;&plus;&space;R_2&space;\rightarrow&space;R_2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?1(R_1)&space;&plus;&space;R_2&space;\rightarrow&space;R_2" title="1(R_1) + R_2 \rightarrow R_2" /></a>

After Reducing we are left with the following matrix: <br />

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;-3&space;&&space;6&space;\\&space;0&&space;0&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;-3&space;&&space;6&space;\\&space;0&&space;0&space;\end{bmatrix}" title="\begin{bmatrix} -3 & 6 \\ 0& 0 \end{bmatrix}" /></a>

We now have the following equation: <a href="https://www.codecogs.com/eqnedit.php?latex=-3_{x1}&space;&plus;&space;6_{x2}&space;=0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?-3_{x1}&space;&plus;&space;6_{x2}&space;=0" title="-3_{x1} + 6_{x2} =0" /></a>

From here we can set <a href="https://www.codecogs.com/eqnedit.php?latex=x_{2}&space;=&space;1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x_{2}&space;=&space;1" title="x_{2} = 1" /></a> and solve

<a href="https://www.codecogs.com/eqnedit.php?latex=6&space;=&space;3x_{1}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?6&space;=&space;3x_{1}" title="6 = 3x_{1}" /></a> thus:

<a href="https://www.codecogs.com/eqnedit.php?latex=x_{1}&space;=&space;2" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x_{1}&space;=&space;2" title="x_{1} = 2" /></a>

So or first eigenvector is <br />

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;2&space;\\&space;1&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;2&space;\\&space;1&space;\end{bmatrix}" title="\begin{bmatrix} 2 \\ 1 \end{bmatrix}" /></a>

We can normalize this vector <a href="https://www.codecogs.com/eqnedit.php?latex=x" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x" title="x" /></a> by dividing <a href="https://www.codecogs.com/eqnedit.php?latex=x" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x" title="x" /></a> by the square root of <a href="https://www.codecogs.com/eqnedit.php?latex=x^{t}x" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x^{t}x" title="x^{t}x" /></a>; that is, <a href="https://www.codecogs.com/eqnedit.php?latex=x/\sqrt{x^{t}x}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x/\sqrt{x^{t}x}" title="x/\sqrt{x^{t}x}" /></a>. We can compare what we've calculated to base R with the "eigen" function.

``` r
v<-matrix(c(2,1),nrow=2)

print(v/as.numeric(sqrt(t(v)%*%v)))
```

    ##           [,1]
    ## [1,] 0.8944272
    ## [2,] 0.4472136

``` r
A<-matrix(c(4,6,3,1),nrow=2,byrow=TRUE)

eigen(A)$vectors[1:2]
```

    ## [1] 0.8944272 0.4472136

Also, remember, <a href="https://www.codecogs.com/eqnedit.php?latex=Ax&space;=&space;{\lambda&space;x}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Ax&space;=&space;{\lambda&space;x}" title="Ax = {\lambda x}" /></a>. Again, we can take what we've calculated and make sure these two quantities are indeed equal.

``` r
A%*%matrix(c(2,1),nrow=2)
```

    ##      [,1]
    ## [1,]   14
    ## [2,]    7

``` r
7*matrix(c(2,1),nrow=2)
```

    ##      [,1]
    ## [1,]   14
    ## [2,]    7

### **Eigenvector Number 2**

We will simply repeat the steps above with our second eigenvalue. We'll move on to <a href="https://www.codecogs.com/eqnedit.php?latex={\lambda}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?{\lambda}" title="{\lambda}" /></a> = −2

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;6&space;&6&space;\\&space;3&&space;3&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;6&space;&6&space;\\&space;3&&space;3&space;\end{bmatrix}" title="\begin{bmatrix} 6 &6 \\ 3& 3 \end{bmatrix}" /></a>

Again, we will need to do some row reduction:

<a href="https://www.codecogs.com/eqnedit.php?latex=-.5(R_{1})&plus;R_{2}\rightarrow&space;R_{2}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?-.5(R_{1})&plus;R_{2}\rightarrow&space;R_{2}" title="-.5(R_{1})+R_{2}\rightarrow R_{2}" /></a>

After reduction: <br />

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;6&space;&6&space;\\&space;0&&space;0&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;6&space;&6&space;\\&space;0&&space;0&space;\end{bmatrix}" title="\begin{bmatrix} 6 &6 \\ 0& 0 \end{bmatrix}" /></a>

Multiply row 1 by 1/6:

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;1&space;&1&space;\\&space;0&&space;0&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;1&space;&1&space;\\&space;0&&space;0&space;\end{bmatrix}" title="\begin{bmatrix} 1 &1 \\ 0& 0 \end{bmatrix}" /></a>

We now have the following equation: <a href="https://www.codecogs.com/eqnedit.php?latex=1x_{1}&space;&plus;&space;1x_{2}&space;=0" target="_blank"><img src="https://latex.codecogs.com/gif.latex?1x_{1}&space;&plus;&space;1x_{2}&space;=0" title="1x_{1} + 1x_{2} =0" /></a>

From here we can set <a href="https://www.codecogs.com/eqnedit.php?latex=x_{2}&space;=1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x_{2}&space;=1" title="x_{2} =1" /></a> and solve

<a href="https://www.codecogs.com/eqnedit.php?latex=1x_{1}&space;=&space;-1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?1x_{1}&space;=&space;-1" title="1x_{1} = -1" /></a> thus:

<a href="https://www.codecogs.com/eqnedit.php?latex=x_{1}&space;=&space;-1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?1x_{1}&space;=&space;-1" title="1x_{1} = -1" /></a>

So or second eigenvector is <br />
 <a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;-1\\1&space;\end{bmatrix}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;-1\\1&space;\end{bmatrix}" title="\begin{bmatrix} -1\\1 \end{bmatrix}" /></a>

We can perform the same set of checks we did above to ensure our calculations are correct.

``` r
v<-matrix(c(-1,1),nrow=2)

print(v/as.numeric(sqrt(t(v)%*%v)))
```

    ##            [,1]
    ## [1,] -0.7071068
    ## [2,]  0.7071068

``` r
eigen(A)$vectors[3:4]
```

    ## [1] -0.7071068  0.7071068

Now we can compare what we've done to the "eigen" function in base R.

``` r
A%*%matrix(c(-1,1),nrow=2)
```

    ##      [,1]
    ## [1,]    2
    ## [2,]   -2

``` r
-2*matrix(c(-1,1),nrow=2)
```

    ##      [,1]
    ## [1,]    2
    ## [2,]   -2

Hopefully, you now have a basic understanding of where eigenvectors and eigenvalues come from, and they no longer seem like they just pop out of thin air!

1.<https://math.stackexchange.com/questions/23312/what-is-the-importance-of-eigenvalues-eigenvectors>
