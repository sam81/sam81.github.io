# Correlation and regression



The function `cor` can be used to compute Pearson's $r$ correlation coefficient. For example, using the `iris` dataset we can compute the correlation between petal length and petal width for the setosa iris species as follows:

```r
data(iris)
iris_setosa = iris[iris$Species == "setosa",]
cor(iris_setosa$Petal.Length, iris_setosa$Petal.Width)
```

```
## [1] 0.33163
```

Kendall's $\tau$, or Spearman's $\rho$ rank correlation coefficients can be obtained with the same function by changing the `method` argument from its default value (`pearson`) to `kendall`, or `spearman`, e.g.:

```r
cor(iris_setosa$Petal.Length, iris_setosa$Petal.Width, method="kendall")
```

```
## [1] 0.2217029
```

A significance test on the correlation coefficient can be obtain with the `cor.test` function:

```r
cor.test(iris_setosa$Petal.Length, iris_setosa$Petal.Width)
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  iris_setosa$Petal.Length and iris_setosa$Petal.Width
## t = 2.4354, df = 48, p-value = 0.01864
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.05870091 0.55842995
## sample estimates:
##     cor 
## 0.33163
```

If we pass a matrix or dataframe to the `cor` function, it will compute the correlations between each pair of columns of the matrix/dataframe:

```r
cor(iris_setosa[, 1:4])
```

```
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length    1.0000000   0.7425467    0.2671758   0.2780984
## Sepal.Width     0.7425467   1.0000000    0.1777000   0.2327520
## Petal.Length    0.2671758   0.1777000    1.0000000   0.3316300
## Petal.Width     0.2780984   0.2327520    0.3316300   1.0000000
```

in the code above we selected columns 1-4 to remove the non-numerical column specifying the iris species. The `cor.test` function, however, cannot be passed a matrix or dataframe as input. The `rcorr` function in the [`Hmisc`](https://cran.r-project.org/web/packages/Hmisc/index.html) package can be used to compute correlations and significance tests between all the columns of a matrix:

```r
corr_mat = Hmisc::rcorr(as.matrix(iris_setosa[,1:4]))
corr_mat$r # correlation coefficients
```

```
##              Sepal.Length Sepal.Width Petal.Length Petal.Width
## Sepal.Length    1.0000000   0.7425467    0.2671758   0.2780984
## Sepal.Width     0.7425467   1.0000000    0.1777000   0.2327520
## Petal.Length    0.2671758   0.1777000    1.0000000   0.3316300
## Petal.Width     0.2780984   0.2327520    0.3316300   1.0000000
```

```r
corr_mat$P # corresponding p-values
```

```
##              Sepal.Length  Sepal.Width Petal.Length Petal.Width
## Sepal.Length           NA 6.709842e-10   0.06069778  0.05052644
## Sepal.Width  6.709842e-10           NA   0.21697892  0.10382114
## Petal.Length 6.069778e-02 2.169789e-01           NA  0.01863892
## Petal.Width  5.052644e-02 1.038211e-01   0.01863892          NA
```

note that `rcorr` can compute only Pearson's $r$ or Spearman's $\rho$ correlation coefficients, by passing an argument named `type` (rather than `method`, as for the `cor` function).

Functions to compute partial, and semi-partial correlations are available in the [`ppcor`](https://cran.r-project.org/web/packages/ppcor/) package.

## Linear regression

Regression models are expressed in R with a symbolic syntax that is clean and convenient. If you have a dependent variable `rts` and you want to check the influence of an independent variable `age` on it, this would be expressed as:

```r
rtfit = lm(rts ~ age)
plot(rts ~ age)
abline(rtfit)
```
if you want to add another predictor `score`

```r
rtfit2 = lm(rts ~ age + score)
```


