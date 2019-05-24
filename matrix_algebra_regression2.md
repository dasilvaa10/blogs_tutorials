---
layout: post
title: "An Algebraic View of Multiple Regression Part 2"
date: 2018-09-15
excerpt: "Regression using matrix algebra with categorical predictors"
tags: [Multiple regression, R, algebra, categorical data]
comments: false
---

### **Back to Business**

In the previous <a href="https://dasilvaa10.github.io//b1/"><b>tutorial</b></a>, we covered how to approach regression from an algebraic viewpoint. However, we only discussed how to implement this framework working with numerical data without any interaction terms. Here, we will expand what was covered in the previous tutorial to include incoporating an interaction term and illustrating a few different ways for working with categorical data. Before starting, we are going to incorporate what we did last tutorial into a function called "my\_lm" that will take in a design matrix and response variable. "my\_lm" will output what we are used to seeing from base r, regression coefficients, standard errors, t and p values etc.

### **A Handy Function**

``` r
my_lm<-function(x,y){
  
XtX<-t(x)%*%x

XtY<-t(x)%*%y  
  
betas<-solve(XtX)%*%XtY
  
hat_matrix<- x%*%solve(XtX)%*%t(x)

predicted<-hat_matrix%*%y  
  
residuals<-(y-predicted)  
  
var_est <- (1/(nrow(x)-ncol(x)))*t(residuals)%*%residuals

ses<-sqrt(diag(as.numeric(var_est)*solve(XtX)))  
  
tvals<-betas/ses

pvals<-sapply(tvals, function (p) 2*pt(abs(-p), nrow(x)-ncol(x), lower=FALSE))  

r2<-sum((predicted-mean(y))^2)/sum((y-mean(y))^2)

adjusted_r2<-1-(((1-r2)*(nrow(x)-1))/(nrow(x)-(ncol(x)-1)-1))

return(round(data.frame(b=betas,se=ses,t_value = tvals,p_value=pvals,r2=c(r2,rep(NA,ncol(x)-1)),adj_r2=c(adjusted_r2,rep(NA,ncol(x)-1))),6))

}
```

### **Incorporating an Interaction Term into the Design Matrix**

Now, we have a function that will take in our response variable and a design matrix. In this post, we are going to create 3 different design matrices. Let's start by setting up a design matrix for an interaction between two numeric predictor variables. To start, we will partition off our response variable (mpg), predictor variables (hp and disp), and create a variable for our intercept term.

``` r
y<-mtcars[,"mpg"]

x<-mtcars[,c("hp","disp")]

x0<-rep(1,nrow(x))
```

Now, we simply need to create our interaction term which we will do by multiplying our 2 predictor variables by each other and adding that term to our design matrix.

``` r
x<-as.matrix(cbind(x0,x,x[,1]*x[,2]))
```

Our design matrix is ready to go. It contains 4 columns: an intercept, disp, hp, and the product of hp \* disp. We can fit a model with our lm function, "my\_lm", and compare the results to "lm" from base r.

``` r
my_lm(x,y)
```

    ##                         b       se   t_value  p_value       r2   adj_r2
    ## x0              39.674263 2.914172 13.614248 0.000000 0.819849 0.800548
    ## hp              -0.097894 0.024745 -3.956175 0.000473       NA       NA
    ## disp            -0.073373 0.014387 -5.100113 0.000021       NA       NA
    ## x[, 1] * x[, 2]  0.000290 0.000087  3.336151 0.002407       NA       NA

``` r
summary(lm(mpg ~ hp * disp, data = mtcars))
```

    ## 
    ## Call:
    ## lm(formula = mpg ~ hp * disp, data = mtcars)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.5153 -1.6315 -0.6346  0.9038  5.7030 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.967e+01  2.914e+00  13.614 7.18e-14 ***
    ## hp          -9.789e-02  2.474e-02  -3.956 0.000473 ***
    ## disp        -7.337e-02  1.439e-02  -5.100 2.11e-05 ***
    ## hp:disp      2.900e-04  8.694e-05   3.336 0.002407 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.692 on 28 degrees of freedom
    ## Multiple R-squared:  0.8198, Adjusted R-squared:  0.8005 
    ## F-statistic: 42.48 on 3 and 28 DF,  p-value: 1.499e-10

### **Categorical Data: Dummy Coding**

Let's get to work on categorical data. First, we will implement a common scheme for dealing with categorical data known as *dummy coding* where we will use 0s and 1s to convey all possible information about level membership for a factor. The first thing we need to do is get our categorical data in that form. Below, I've created a function that will do just that - read in a vector representing some categorical data and output it in dummy coded form. There is undoubtedly a neater way to accomplish this, but I think the funciton below highlights clearly what is going on.

### **Making Dummies**

``` r
make_dummy<-function(vec){

unq<-as.character(unique(vec))
unq<-sort(unq)

dummy_list<-list()

for (i in unq){
  
dummy_list[[i]]<-as.numeric(vec==i)

out<-do.call("cbind",dummy_list)

}
  
 return(out) 
  
}
```

Now we can create an appropriate design matrix representing dummy coded data with our helper function. Our response variable will still be mpg and we will use 2 categorical variables (cyl and gear) as our predictor variables. We'll start by grabbing those variables and running them through our "make\_dummy" function to create a dummy coding scheme for those 2 predictors and take a peek to make sure things are running correctly.

``` r
y<-mtcars[,"mpg"]

cyl_temp<-make_dummy(mtcars[,"cyl"])

gear_temp<-make_dummy(mtcars[,"gear"])

head(gear_temp)
```

    ##      3 4 5
    ## [1,] 0 1 0
    ## [2,] 0 1 0
    ## [3,] 0 1 0
    ## [4,] 1 0 0
    ## [5,] 1 0 0
    ## [6,] 1 0 0

We can now finish our design matrix. Notice that we have made the first level of each group the reference level by dropping the first column. This will immitate the default behavior of r with respect to dummy coding.

``` r
x<-as.matrix(cbind(cyl_temp[,-1],gear_temp[,-1]))
head(x)
```

    ##      6 8 4 5
    ## [1,] 1 0 1 0
    ## [2,] 1 0 1 0
    ## [3,] 0 0 1 0
    ## [4,] 1 0 0 0
    ## [5,] 0 1 0 0
    ## [6,] 1 0 0 0

### **Dummy Results**

An intercept column will complete our design matrix. We can fit a model and compare our output to that of base r.

``` r
x0<-rep(1,nrow(x))

x<-cbind(x0,x)

my_lm(x,y)
```

    ##             b       se   t_value  p_value       r2   adj_r2
    ## x0  25.427899 1.881164 13.517111 0.000000 0.739788 0.701238
    ## 6   -6.655978 1.629136 -4.085589 0.000353       NA       NA
    ## 8  -10.542210 1.957977 -5.384235 0.000011       NA       NA
    ## 4    1.324094 1.927619  0.686907 0.498000       NA       NA
    ## 5    1.500181 1.854852  0.808787 0.425707       NA       NA

``` r
summary(lm(mpg ~ as.factor(cyl) + as.factor(gear), data = mtcars))
```

    ## 
    ## Call:
    ## lm(formula = mpg ~ as.factor(cyl) + as.factor(gear), data = mtcars)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -5.3520 -1.7633 -0.3789  1.7393  7.1480 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)        25.428      1.881  13.517 1.55e-13 ***
    ## as.factor(cyl)6    -6.656      1.629  -4.086 0.000353 ***
    ## as.factor(cyl)8   -10.542      1.958  -5.384 1.09e-05 ***
    ## as.factor(gear)4    1.324      1.928   0.687 0.498000    
    ## as.factor(gear)5    1.500      1.855   0.809 0.425707    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 3.294 on 27 degrees of freedom
    ## Multiple R-squared:  0.7398, Adjusted R-squared:  0.7012 
    ## F-statistic: 19.19 on 4 and 27 DF,  p-value: 1.405e-07

### **Categorical Data: User Defined Contrasts**

We've seen one way (out of many see: simple coding, deviation coding, forward difference coding, helmert coding, etc ) to accomodate caterogical data and incorporate it into a design matrix. Here, we will take a look at an additional scheme for handling categorical data, user-defined custom contrasts. This will allow us some additional control over the manipulation of our factor levels.

Below, we will be using the "warpbreaks" data because yarn is fascinating, and I'm getting tired of "mtcars". We'll be looking at the relationship between breaks (y) and tension (x). We'll start by creating a contrast matrix depicting the differences between levels in tension we want to investigate. Tension has 3 levels which means we will be able to create 2 (k-1, where k = levels of a factor) orthogonal contrasts.

-   contrast 1: will compare "l" to the average of "m" and "h"
-   contrast 2: we will compare "m" to "h"

To ensure that these contasts are orthogonal we will multiply the contrasts in a pairwise fashion and ensure that the sum of the resulting products are equal to 0.

``` r
data("warpbreaks")

lVSmh<-c(1, -.5,-.5)
mVsh<-c(0,1,-1)

sum(lVSmh*mVsh)
```

    ## [1] 0

### **Don't forget to Invert!**

At this point, we have an initial contrast matrix, but before moving on we need to take the inverse of it to ensure out beta coefficients accurately represent the differences in estimates we have supplied. We can do this in base r, assuming we have a square matrix, as shown below. The ones are a constant representing the intercept term.

``` r
mat<-solve(rbind(c(1,1,1),lVSmh,mVsh))
```

With a final contrast matrix, we can move on and incorporate that scheme into our design matrix. Instead of using the "make\_dummy" function, we will use the "model matrix" command to apply our inverted contrasts to our design matrix. Finally, we can use our "my\_lm" function and compare it to "lm".

``` r
y<-warpbreaks[,"breaks"]

x<-warpbreaks[,"tension"]

x0<-rep(1,length(x))

x<-apply(model.matrix(~x+0),2,function(x) as.numeric(x))
colnames(x)<-sub("x","",colnames(x))

x<-x%*%mat[,-1]

x<-as.matrix(cbind(x0,x))

head(x)
```

    ##      x0     lVSmh mVsh
    ## [1,]  1 0.6666667    0
    ## [2,]  1 0.6666667    0
    ## [3,]  1 0.6666667    0
    ## [4,]  1 0.6666667    0
    ## [5,]  1 0.6666667    0
    ## [6,]  1 0.6666667    0

``` r
my_lm(x,y)
```

    ##               b       se   t_value  p_value       r2   adj_r2
    ## x0    28.148148 1.616742 17.410415 0.000000 0.220329 0.189754
    ## lVSmh 12.361111 3.429628  3.604214 0.000711       NA       NA
    ## mVsh   4.722222 3.960193  1.192422 0.238614       NA       NA

``` r
contrasts(warpbreaks$tension)<-mat[,-1]

summary(lm(breaks ~ tension, data = warpbreaks))
```

    ## 
    ## Call:
    ## lm(formula = breaks ~ tension, data = warpbreaks)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -22.389  -8.139  -2.667   6.333  33.611 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)    28.148      1.617  17.410  < 2e-16 ***
    ## tensionlVSmh   12.361      3.430   3.604 0.000711 ***
    ## tensionmVsh     4.722      3.960   1.192 0.238614    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 11.88 on 51 degrees of freedom
    ## Multiple R-squared:  0.2203, Adjusted R-squared:  0.1898 
    ## F-statistic: 7.206 on 2 and 51 DF,  p-value: 0.001753

### **A Few Quick Checks**

In addition, we can also use the "model.matrix" command to compare the design matrix from r to our own.

``` r
model.matrix(lm(breaks ~ tension, data = warpbreaks))[1:10,]
```

    ##    (Intercept) tensionlVSmh tensionmVsh
    ## 1            1    0.6666667         0.0
    ## 2            1    0.6666667         0.0
    ## 3            1    0.6666667         0.0
    ## 4            1    0.6666667         0.0
    ## 5            1    0.6666667         0.0
    ## 6            1    0.6666667         0.0
    ## 7            1    0.6666667         0.0
    ## 8            1    0.6666667         0.0
    ## 9            1    0.6666667         0.0
    ## 10           1   -0.3333333         0.5

``` r
x[1:10,]
```

    ##       x0      lVSmh mVsh
    ##  [1,]  1  0.6666667  0.0
    ##  [2,]  1  0.6666667  0.0
    ##  [3,]  1  0.6666667  0.0
    ##  [4,]  1  0.6666667  0.0
    ##  [5,]  1  0.6666667  0.0
    ##  [6,]  1  0.6666667  0.0
    ##  [7,]  1  0.6666667  0.0
    ##  [8,]  1  0.6666667  0.0
    ##  [9,]  1  0.6666667  0.0
    ## [10,]  1 -0.3333333  0.5

Finally, we can double check to make sure the our beta coefficients are representing what they should be.

``` r
cat("mean(l)-(mean(m)+mean(h))/2:",aggregate(breaks ~ tension, data = warpbreaks, mean)[1,2]-((aggregate(breaks ~ tension, data = warpbreaks, mean)[2,2]+aggregate(breaks ~ tension, data = warpbreaks, mean)[3,2])/2),"result from my_lm:",my_lm(x,y)[2,1])
```

    ## mean(l)-(mean(m)+mean(h))/2: 12.36111 result from my_lm: 12.36111

``` r
cat("mean(m)-mean(h):",aggregate(breaks ~ tension, data = warpbreaks, mean)[2,2] - aggregate(breaks ~ tension, data = warpbreaks, mean)[3,2],"result from my_lm:",my_lm(x,y)[3,1])
```

    ## mean(m)-mean(h): 4.722222 result from my_lm: 4.722222

Hopefully, you now have a better intuition of how categorical data is incorporated into a regression model and a better understanding of the algebra underlying regression!

<br> <br>