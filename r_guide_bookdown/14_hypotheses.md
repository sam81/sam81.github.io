# Hypothesis testing



\BeginKnitrBlock{rmdwarning}<div class="rmdwarning">This is not a statistics textbook. The examples are simply meant to show the syntax to perform certain tests in R, the correct execution of tests and the interpretation of their results depend on good theoretical knowledge of the statistical tests and their assumptions.</div>\EndKnitrBlock{rmdwarning}

## $\chi^2$ test

### Testing hypotheses about the distribution of a categorical variable

The $\chi^2$ test can be used to verify hypotheses about the distribution of a categorical variable. For example we might test whether a given population of participants, say guitar players, follows the pattern of handedness found in the general population, known to have a distribution of $80\%$ right-handed, $15\%$ left-handed, and $5\%$ ambidextrous individuals (all the data in this example are made up). The handedness frequencies for a hypothetical sample of guitar players are given in Table \@ref(tab:hand)

Right Left   Both   Total
----- -----  -----  -----
93    18     2      113

Table: (\#tab:hand) Table of handedness frequencies for a sample of guitar players.

we can run the test with the following command:

```r
chisq.test(x=c(93,18,2), p=c(0.8,0.15,0.05))
```

```
## 
## 	Chi-squared test for given probabilities
## 
## data:  c(93, 18, 2)
## X-squared = 2.4978, df = 2, p-value = 0.2868
```
we give the function a vector `x` with the handedness counts for each category in our sample of guitar players, and a vector `p` of the expected frequencies for each category in the general population. R returns the value of the $\chi^2$ statistics and its associated *p*-value. In our case, assuming $\alpha=0.05$, the *p*-value indicates that the $\chi^2$ statistics is not significant, , therefore we do not reject the null hypothesis that the distribution of handedness in the population of guitar players differs from that of the general population.


#### Testing hypotheses about the association between two categorical variables

The $\chi^2$ test can also be used to verify a possible association between two categorical variables, for example sex and cosmetic products usage (classified as "high", "medium" or "low"). A contingency table showing hypothetical data on cosmetic usage for a sample of $44$ males and $67$ females is shown in Table \@ref(tab:cosmetic).

            High  Medium   Low  Total
----------- ----  ------   ---- -----
Males       $6$   $11$     $27$ $44$
Females     $25$  $30$     $12$ $67$
Total       $31$  $41$     $39$ $111$

Table: (\#tab:cosmetic) Cosmetic usage by sex.

The formula for the $\chi^2$ statistics is
\begin{equation}
  \chi^2=\sum\frac{(f_e - f_o)^2}{f_e}
  (\#eq:chisq)
  \end{equation}
  
where $f_e$ are the expected frequencies and $f_o$ are the observed frequencies. In order to get the statistics and perform a test of significance on it with R we can first arrange the data in a matrix:


```r
cosm = c(6, 11, 27, 25, 30, 12)
cosm = matrix(cosm, nrow=2, byrow=TRUE)
```

then we can pass the matrix to the `chisq.test()` function to test the null hypothesis that there is no association between sex and cosmetic usage:


```r
chisq.test(cosm)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  cosm
## X-squared = 22.416, df = 2, p-value = 1.357e-05
```

the *p*-value tells us that the null hypothesis does not hold, there is indeed an association between sex and degree of cosmetic usage. If we store the results of the test in a variable, we'll be able to get the matrices of the expected frequencies and residuals:


```r
cosmtest = chisq.test(cosm)
cosmtest$expect ## expected frequencies under a true null hypothesis
```

```
##          [,1]     [,2]     [,3]
## [1,] 12.28829 16.25225 15.45946
## [2,] 18.71171 24.74775 23.54054
```

If we have only a few observations and want to use the Yate's correction for continuity we have to set the optional argument `correct` to `TRUE`:


```r
chisq.test(x=mydata, correct=TRUE)
```

Often we might not have the data already tabulated in a contingency table, however if we have a series of observations classified according to two categorical variables we can easily get a contingency table with the `table()` function. The next example uses the data in the file `hair_eyes.txt`, that lists the hair and eyes colors for 260 females. We will first build our contingency table from that:


```r
dat = read.table("datasets/hair_eyes.txt", header=TRUE)
tabdat = table(dat$hair, dat$eyes)
```

now we can perform the $\chi^2$ test directly on the table to test for an association between hair and eyes color:


```r
chisq.test(tabdat)
```

```
## Warning in chisq.test(tabdat): Chi-squared approximation may be incorrect
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  tabdat
## X-squared = 142.9, df = 9, p-value < 2.2e-16
```

the $\chi^2$ statistics is significant, suggesting an association between hair and eyes color.


## Student's *t*-test

### One sample *t*-test

A one-sample *t*-test can be used to test if a group mean is significantly different from a given, known, population mean ($\mu$). The following example simulates some data and runs a one-sample *t*-test of the hypothesis that the group mean is equal to a known population mean of 18:

```r
set.seed(938) #set random seed
gr1 = rnorm(20, mean=20, sd=4) #simulate data
t.test(gr1, mu = 18)
```

```
## 
## 	One Sample t-test
## 
## data:  gr1
## t = 3.0375, df = 19, p-value = 0.006774
## alternative hypothesis: true mean is not equal to 18
## 95 percent confidence interval:
##  18.65846 21.57681
## sample estimates:
## mean of x 
##  20.11763
```

For the example we simulated data from a normal distribution with a mean of 20, so unsurprisingly the null hypothesis that the group mean is equal to 18 is rejected at the 0.05 alpha level. By default the test is run as a two-sided test, but this can be changed by passing the `alternative` argument (valid options for this argument are: `two-sided`, `less`, `greater`). For example, to test the hypothesis that the group mean is > 18 we would call the function as follows:

```r
t.test(gr1, mu=18, alternative="greater")
```

```
## 
## 	One Sample t-test
## 
## data:  gr1
## t = 3.0375, df = 19, p-value = 0.003387
## alternative hypothesis: true mean is greater than 18
## 95 percent confidence interval:
##  18.91215      Inf
## sample estimates:
## mean of x 
##  20.11763
```

By default the output reports the bounds of a 95% confidence interval on the group mean. The confidence level of this interval can be changed via the `conf.level` argument. For example, to print out a 99% confidence interval:


```r
t.test(gr1, mu = 18, conf.level=0.99)
```

```
## 
## 	One Sample t-test
## 
## data:  gr1
## t = 3.0375, df = 19, p-value = 0.006774
## alternative hypothesis: true mean is not equal to 18
## 99 percent confidence interval:
##  18.12310 22.11216
## sample estimates:
## mean of x 
##  20.11763
```

### Two-sample *t*-test

A two-sample *t*-test can be used to test whether there is a significant difference between the means of two groups. In the following example we'll simulate data for the two groups and run the test:

```r
gr1 = rnorm(25, mean=10, sd=2)
gr2 = rnorm(25, mean=8, sd=2)
t.test(gr1, gr2)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  gr1 and gr2
## t = 3.3003, df = 46.562, p-value = 0.001858
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  0.7978221 3.2906509
## sample estimates:
## mean of x mean of y 
##  9.821537  7.777301
```

as for the one-sample *t* test described in the previous section it is possible to run either a two-sided (the default) or a directional one-sided test via the `alternative` argument, and it is also possible to set the confidence level for the confidence interval reported via the `conf.level` argument. An additional argument that can be passed for a two-sample *t*-test is `var.equal`, if this is set to `TRUE`, then the variances of the two groups are treated as being equal, and the pooled variance is used to estimate the variance otherwise the Welch (or Satterthwaite) approximation to the degrees of freedom is used [@DerrickEtAl2016]. One way to decide whether to treat the variances of the two groups as equal is by running the  Levene test for homogeneity of variances which is described in Section \@ref(levenetest).

### Paired *t*-test
A paired sample *t*-test can be used to test whether the means of the same group of participants differ between two conditions. We'll simulate again some data and run the test:

```r
base = rnorm(30, mean=0, sd=5)
cnd1 = base + rnorm(30, mean=-2, sd=2.5)
cnd2 = cnd1 + rnorm(30, mean=2, sd=2.5)
t.test(cnd1, cnd2, paired=TRUE)
```

```
## 
## 	Paired t-test
## 
## data:  cnd1 and cnd2
## t = -4.4552, df = 29, p-value = 0.0001149
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -2.995939 -1.110716
## sample estimates:
## mean of the differences 
##               -2.053328
```

as for the one-sample *t* test described previously it is possible to run either a two-sided (the default) or a directional one-sided test via the `alternative` argument, and it is also possible to set the confidence level for the confidence interval reported via the `conf.level` argument.

A paired *t*-test could also be calculated as a one-sample *t*-test on the difference between the scores in the two conditions:

```r
t.test(cnd1-cnd2, mu=0)
```

```
## 
## 	One Sample t-test
## 
## data:  cnd1 - cnd2
## t = -4.4552, df = 29, p-value = 0.0001149
## alternative hypothesis: true mean is not equal to 0
## 95 percent confidence interval:
##  -2.995939 -1.110716
## sample estimates:
## mean of x 
## -2.053328
```

## The Levene test for homogeneity of variances {#levenetest}

The Levene test can be used to test if the variances of two samples are significantly different from each other. This procedure is important because some statistical tests, such as the two independent sample *t*-test, are based on the assumption that the variances of the samples being tested are equal. In R the function to perform the Levene test is contained in the `car` package, so in order to call it the package must be installed and the `car` library has to be loaded with the command:

```r
library(car)
```

```
## Carico il pacchetto richiesto: carData
```

We'll first simulate some data:

```r
y1 = rnorm(25, mean=0, sd=1)
y2 = rnorm(25, mean=0, sd=2)
y = c(y1, y2)
grps = factor(rep(c("A", "B"), each=25))
```

we need a vector (`y` in our case) with the values of the dependent variable that we are measuring, and another vector (`grps` in our case) defining the group to which a given observation belongs to. We can then run the test with:

```r
leveneTest(y, grps)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##       Df F value   Pr(>F)   
## group  1   9.787 0.002986 **
##       48                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

We'll present another example with three groups. Let's imagine that in a verbal memory task  we have the measures (number of words recalled) of three groups that have been given three different treatments (e.g. three different rehearsal procedures). We want to see if there are significant differences in the variance between the groups. The data are stored in a text file `verbal.txt`, as in Table \@ref(tab:levene), but with no header indicating the column names.

Procedure 1  Procedure 2  Procedure 3
-----------  -----------  -----------
12           13           15
9            12           17
9            11           15
8            7            13
7            14           16 
12           10           17
8            10           12 
7            10           16
10           12           14
7            9            15

Table: (\#tab:levene) Data for the Levene test example.

First we will create the vector with the values of the dependent variables, by reading in the file with the `scan` function:

```r
values = scan("datasets/verbal.txt")
```

Now we need to create a second vector defining the group to which a given observation on the data vector belongs to. We will use the `rep` function to do this, and for convenience we'll give the labels `1`, `2` and `3` to the three groups:

```r
groups = factor(rep(1:3, 10))
```

Now the data are in a proper format for using the `leveneTest` function. This format can be called the "one row per observation" format. You can find more info on this format in Section \@ref(onerowperobservation). To perform the test we can use the following commands:

```r
leveneTest(values, groups)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##       Df F value Pr(>F)
## group  2  0.3391 0.7154
##       27
```

As you can see we have an *F* value with its associated *p* value, if the *p* value is significant, then we reject the null hypothesis of equal variances between the groups. In our case below the *p* value is greater than 0.05 so we cannot reject the null hypothesis that the variances in the three groups are equal.





 
