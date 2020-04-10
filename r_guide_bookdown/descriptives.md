# Descriptive



## Tables

## The `scale` Function

The `scale` function can be used to easily transform your data into $z$ scores. Here's a silly example

```r
a = c(1,2,3)
scale(a)
```

```
##      [,1]
## [1,]   -1
## [2,]    0
## [3,]    1
## attr(,"scaled:center")
## [1] 2
## attr(,"scaled:scale")
## [1] 1
```

