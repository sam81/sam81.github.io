# Hypothesis Testing



## $\chi^2$ test

### Testing Hypotheses about the Distribution of a Categorical Variable

You can use the $\chi^2$ test to verify hypotheses about the distribution of a categorical variable, for example you might want to know whether, as far as handedness is concerned, your experimental sample is representative of the general population and follows a distribution of $80\%$ right-handed, $15\%$ left-handed, and $5\%$ using both hands indifferently (fake data). The frequencies for your sample are given in Table \@ref(tab:hand)

Right Left   Both   Total
----- -----  -----  -----
93    18     2      113

Table: (\#tab:hand) Handedness frequencies for an experimental sample.

in R you can use the `chisq.test()` function to see if the distribution of your sample differs from that of the general population:

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
you give the function a vector `x` with the frequencies for each category in your sample, and a vector `p` of the theoretical probability for each category, R returns the value of the $\chi^2$ statistics and its associated *p*-value. In our case the *p*-value indicates that the $\chi^2$ statistics is not significant, assuming $\alpha=0.05$, so we cannot reject the null hypothesis that the distribution for the handedness variable in our sample differs from that of the general population.


#### Testing Hypotheses about the Association between Two Categorical Variables

The $\chi^2$ test can also be used to verify a possible association between two categorical variables, for example sex and cosmetic products usage (classified as "high", "medium" or "low"). In this case the data are best summarised by a contingency table as Table \@ref(tab:cosmetic) which presents the data on cosmetic usage for a sample of $44$ males and $67$ females.

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
  
where $f_e$ are the expected frequencies and $f_o$ are the observed frequencies, to get the statistics and perform a test of significance on it with
R you can first arrange the data in a matrix:


```r
cosm <- c(6,11,27,25,30,12)
cosm <- matrix(cosm, nrow=2, byrow=TRUE)
```

then you can directly give the matrix to the `chisq.test()` function to test the null hypothesis that there is no association between sex and cosmetic usage:


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

the *p*-value tells us that the null hypothesis does not hold, there is indeed an association between sex and degree of cosmetic usage. If you store the results of the test in a variable, you'll be able to get the matrices of the expected frequencies and residuals:


```r
cosmtest <- chisq.test(cosm)
cosmtest$expect ## expected frequencies under a true null hypothesis
```

```
##          [,1]     [,2]     [,3]
## [1,] 12.28829 16.25225 15.45946
## [2,] 18.71171 24.74775 23.54054
```

If you have only a few observations and want to use the Yate's correction for continuity you have to set the optional argument `correct` to true


```r
chisq.test(x=mydata, correct=TRUE)
```

Very often you don't have your data already tabulated in a contingency table, however if you have a series of observations classified according to two categorical variables you can easily get a contingency table with the `table()` function. The next example uses the data in the file `hair_eyes.txt`, that lists the hair and eyes colours for 260 females. We will first build our contingency table from that:


```r
dats <- read.table("datasets/hair_eyes.txt", header=TRUE)
tabdats <- table(dats$hair, dats$eyes)
```

now we can perform the $\chi^2$ test directly on the table to see if there is an association between hair and eyes colour:


```r
chisq.test(tabdats)
```

```
## Warning in chisq.test(tabdats): Chi-squared approximation may be incorrect
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  tabdats
## X-squared = 142.9, df = 9, p-value < 2.2e-16
```

the $\chi^2$ statistics is highly significant, so there clearly is an association between hair and eyes colour.


## Student's *t* test

### One sample *t*-test


To test if a group's mean is different from a given population's mean ($\mu$), you can run a simple t-test in R. The syntax is as follows:


```r
t.test(gr1, mu = 17)
```

where `gr1` is the vector containing the data of the group, and mu is an optional argument (If you leave it out it defaults to 0) specifying the population's mean.

- mu = 0 [Default] or another value. mu is the value specifying the population's mean.

- `alternative` =   `two.sided`  [default] or `less`  or `greater`. This option allows choosing whether the test should be two-tailed or one-tailed. In particular the alternative hypothesis $H_1$ becomes:

    * `two.sided`, true mean of the group is not equal to mu

    *  `less`, true mean of the group is lesser than mu

    * `greater`, true mean of the group is greater than mu


- `conf.level` = 0.95 [Default] or another value. This option sets the confidence level for the estimation of the confidence interval.


### Two samples t-test

With R you can easily run a t test to find out whether the difference between the means of two groups is significant. The syntax is as follows: 

```r
t.test(gr1, gr2)
```
where `gr1` is the data vector of the first group and `gr2` is the data vector of the second group. The following options can also be selected: 

The following options can also be selected: 

- mu = 0 [Default] or another value. mu is the value given by the difference between the means under $H_0$, in other words it is the value against which you test the alternative hypothesis.

- `alternative` =   `two.sided`  [default] or  `less`  or `greater`. This option allows choosing whether the test should be two-tailed or one-tailed. In particular the alternative hypothesis $H_1$ becomes:


    * `two.sided`, true mean of the group is not equal to mu

    * `less`, true mean of the group is lesser than mu

    * `greater`, true mean of the group is greater than mu

- `paired = FALSE` [Default] or `TRUE`. If the TRUE option is selected, then a *paired* *t*-test is performed, note that in this case the number of observations has to be equal in the two groups. Missing values are removed if paired is TRUE.

A shortcut to choose a paired t-test is:

```r
t.test(gr1-gr2)
```

- `var.equal = FALSE` [Default] or `TRUE`

Tells whether the variances of the two groups should be treated as equal. If TRUE is selected, then the pooled variance is used to calculate the variance. If the option is left to default or FALSE is selected, then the Welch (or Satterthwaite) approximation to the degrees of freedom is used.

- `conf.level` = 0.95 [Default] or another value

this option sets the confidence level for the estimation of the confidence interval.

##The Levene Test for Homogeneity of Variances

The Levene test can be used to test if the variances of two samples are equal or not. This procedure is important because some statistical tests, such as the two independent sample t test, are based on the assumption that the variances of the samples being tested are equal. In R the function to perform the Levene test is contained in the `car` package, so in order to call it the package must be installed and the `car` library has to be loaded with the command:


```r
library(car)
```

```
## Carico il pacchetto richiesto: carData
```

The syntax to run a Levene test is:

```r
leveneTest(y, group)
```

where `y` is a vector with the values of the dependent variable we are measuring, and `group` is another vector defining the group to which a given observation belongs to. With an example this will be clearer. Let's imagine that in a verbal memory task  we have the measures (number of words recalled) of three groups that have been given three different treatments (three different rehearsal procedures). We want to see if the variances of the three groups are equal. The data are stored in a text file `verbal.txt`, as in the table \@ref(tab:levene), but with no header indicating the column names.

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
values <- scan("datasets/verbal.txt")
```

Now we need to create the second vector defining the group to which a given observation on the data vector belongs to. We will use the `rep` function to do this, and for convenience will give the labels `1`, `2` and `3` to the three groups:


```r
groups <- as.factor (rep(1:3, 10))
```

Notice that we have defined this vector as a factor, this was necessary because we were using  numerical labels, it wouldn't have been if we had used a character or a string labels. Now the data are in a proper format for using the `leveneTest` function. This format can be called "one row per observation", and we will encounter it often later when we talk about ANOVA, you can find more examples and clarifications on this format there. To perform the text we can use the following commands:


```r
leveneTest(values, groups)
```

```
## Levene's Test for Homogeneity of Variance (center = median)
##       Df F value Pr(>F)
## group  2  0.3391 0.7154
##       27
```

As you can see we have an *F* value with its associated *p* value, if the *p* value is significant, then the variances in the groups are not equal. In our case below the *p* value is greater than 0.05 so we cannot reject the null hypothesis that the variances in the three groups are equal.





 
