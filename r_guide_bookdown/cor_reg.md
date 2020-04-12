# Correlation and Regression

Let's imagine we want to see if there is a correlation between a manual and an oculomotor reaction time (RT) task. The manual RT task requires to press one of two buttons depending on the colour taken by the fixation point after a variable delay. The oculomotor RT task requires to make a saccade towards the right or the left depending on the colour taken by the fixation point. The mean RTs in milliseconds, for 37 subjects are in the file `m_o_rt.txt`. We read in the data, and then calculate the correlation for the sample:

```r
dat = read.table("datasets/m_o_rt.txt", header=TRUE)
cor(dat$man, dat$ocul)
```

```
## [1] 0.7922632
```

```r
cor(dat$man, dat$ocul)^2 ## R squared
```

```
## [1] 0.627681
```
the correlation between the two measures in the sample is high, and it would account for ~60% of the variance. We need also to test if this correlation could be extended to the population:

```r
cor.test(dat$man, dat$ocul)
```

```
## 
## 	Pearson's product-moment correlation
## 
## data:  dat$man and dat$ocul
## t = 8.9956, df = 48, p-value = 7.197e-12
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.6593095 0.8771727
## sample estimates:
##       cor 
## 0.7922632
```
the *p*-value tells us that we can extend our finding in the population, so people with a quick hand would also have a quick eye.


## Linear Regression

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


