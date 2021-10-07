Unsupervised Learning
================
Collin Davies
10/6/2021

## P1

Summary: Suppose we want to fit a linear regression, but the number of
variables is much larger than the number of observations. In some cases,
we may improve the fit by reducing the dimension of the features before.

In this problem, we use a data set with n = 300 and p = 200, so we have
more observations than variables, but not by much. Load the data x, y,
x.test, and y.test from 10.R.RData.

First, concatenate x and x.test using the rbind functions and perform a
principal components analysis on the concatenated data frame (use the
“scale=TRUE” option). To within 10% relative error, what proportion of
the variance is explained by the first five principal components?

Task 1: Load the 10.R.RData dataset.

``` r
load("10.R.RData")
```

Task 2: Bind concatenate x and xtest.

``` r
var = rbind(x, x.test)
```

Task 3: Perform principal component analysis on the concatenated data
frame (normalize the response).

``` r
pca.out = prcomp(var, scale=TRUE)
```

Task 4: Return the portion of the variance in the dataset explained by
the first five components.

``` r
sum(pca.out$sdev[0:5]^2) / sum(pca.out$sdev^2)
```

    ## [1] 0.3498565

## P2

Summary: The previous answer suggests that a relatively small number of
“latent variables” account for a substantial fraction of the features’
variability. We might believe that these latent variables are more
important than linear combinations of the features that have low
variance.

We can try forgetting about the raw features and using the first five
principal components (computed on rbind(x,x.test)) instead as
low-dimensional derived features. What is the mean squared test error if
we regress y on the first five principal components, and use the
resulting model to predict y.test?

Task 1: Regress y on the first five principal components.

``` r
xols <- pca.out$x[1:300, 1:5]
```

Task 2: Use xols to predict y.test.

``` r
fit0 <- lm(y ~ xols)
```

Task 3: Return the mean squared test error of y.test using xols.

``` r
sigma(fit0)
```

    ## [1] 1.056433

## P3

Summary: Now, try an OLS linear regression of y on the matrix x. What is
the mean squared prediction error if we use the fitted model to predict
y.test from x.test?

Task 1: Fit an OLS linear regression of y on the matrix x.

``` r
fit1 <- lm(y ~., x)
```

Task 2: Use the fitted model to predict y.test from x.test.

``` r
yhat = predict(fit1, x.test)
```

Task 3: Return the mean squared test error of y.test from x.test.

``` r
mean((yhat-y.test)^2)
```

    ## [1] 3.657197
