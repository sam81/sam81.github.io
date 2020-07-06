# Frequentist hypothesis tests



<div class="rmdwarning">
<p>This is not a statistics textbook. The examples are simply meant to show the syntax to perform certain tests in R, the correct execution of tests and the interpretation of their results depend on good theoretical knowledge of the statistical tests and their assumptions.</p>
</div>

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


### Testing hypotheses about the association between two categorical variables

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


## Analysis of variance

We'll start with an example showing how we can perform a one-way between-subjects analysis of variance (ANOVA) in R. The example uses a dataset from a fictitious experiment on the effect of three types of memory-boosting drugs. Thirty participants were randomly assigned to one of three groups, each given a different memory-boosting drug (drug "A", "B", or "C"), and afterwards their memory was assessed by counting the number of words they could recall from a list. The data is shown in Table \@ref(tab:drugmem).

Drug A    Drug B     Drug C
--------  ---------  ---------
12        13         15
9         12         17
9         11         15 
8         7          13 
7         14         16 
12        10         17 
8         10         12 
7         10         16 
10        12         14 
7         9          15

Table: (\#tab:drugmem) Number of words recalled from a list for participants treated with drug A, B, or C.

We read in the dataset:

```r
drug_mem = read.table("datasets/drug_memory.csv", header=T, sep=",")
head(drug_mem)
```

```
##   n_words drug
## 1      12    A
## 2      13    B
## 3      15    C
## 4       9    A
## 5      12    B
## 6      17    C
```

the dependent variable is the number of words correctly recalled from the list (`n_words`), while the other column (`drug`) denotes the experimental group. We can have a look at the data through a dotplot:

```r
library(ggplot2)
p = ggplot(drug_mem, aes(drug, n_words)) + geom_point()
p = p + xlab("Drug") + ylab("# words recalled")
p = p + theme_bw()
print(p)
```

<div class="figure">
<img src="14_hypotheses_files/figure-html/drugmem-1.png" alt="Number of words recalled for participants administered different types of memory-boosting drugs." width="326.4" />
<p class="caption">(\#fig:drugmem)Number of words recalled for participants administered different types of memory-boosting drugs.</p>
</div>

the dotplot, shown in Figure \@ref(fig:drugmem), suggests that the drugs have different effects on the number of recalled words. We can run the ANOVA with the `aov` function:

```r
drug_mem_aov = aov(n_words ~ drug, data=drug_mem)
```

the first argument passed to the function is the model formula, which you can read as "explain `n_words` as a function of `drug`"; the other argument, `data`, is the dataframe containing the data. Calling `summary` on the `aov` output gives a pretty printout with various info we need to report the results (degrees of freedom, *F* value, *p*-value, etc...)

```r
summary(drug_mem_aov)
```

```
##             Df Sum Sq Mean Sq F value   Pr(>F)    
## drug         2  194.9   97.43   27.84 2.75e-07 ***
## Residuals   27   94.5    3.50                     
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```
assuming a significance level of 0.05 the results of the test reject the null hypothesis that the three drugs have the same effectiveness on the number of recalled words from a list.

Another function that can run a one-way ANOVA in R is `oneway.test`. Its syntax is very similar to that of `aov`, but it has an additional argument, `var.equal`, to indicate whether homogeneity of variance should be assumed or not. If not, a Welch-type test is run (see `?oneway.test`). For example, if we had reason to suspect non-homogeneous variances across the groups we could have run the test with:

```r
oneway.test(n_words ~ drug, data=drug_mem, var.equal=F)
```

```
## 
## 	One-way analysis of means (not assuming equal variances)
## 
## data:  n_words and drug
## F = 30.598, num df = 2.000, denom df = 17.831, p-value = 1.719e-06
```

The `aov` function is quite flexible, and can handle multiple factors. For example, the syntax to run an ANOVA on a dependent variable `y` with two factors `fct1` and `fct2` stored in a dataframe called `dat` would be:

```r
aov(y ~ fct1*fct2, data=dat)
```

the above would estimate the effects of `fct1`, `fct2`, and their interaction. It would also be possible to estimate only the main effects, without the interaction term, if desired, with the following syntax:

```r
aov(y ~ fct1+fct2, data=dat)
```

The syntax for performing repeated measures ANOVA with `aov` becomes relatively complex because it is necessary to specify appropriate error strata. This is explained in detail in @BaronAndLi2003, and @LiAndBaron2012. The [`ez`](https://cran.r-project.org/web/packages/ez/index.html) package provides an easier interface for running ANOVAs, and for this reason we'll focus on it. 

The function to run ANOVAs through the `ez` package is called `ezANOVA` and we'll first show how to use it by running the one-way between-subjects ANOVA that we ran earlier with `aov`. The `ezANOVA` function requires a subject identifier even for between-subjects designs, so we'll add one to the `drug_mem` data. Each observation came from a different participant, so we'll simply label the participants as P1, P2, P3, etc...:

```r
drug_mem$id = factor(paste0("P", 1:dim(drug_mem)[1]))
```

next we load the `ez` package and run the ANOVA:

```r
library(ez)
ezANOVA(drug_mem, dv=n_words, between=drug, wid=id)
```

```
## Warning: Converting "drug" to factor for ANOVA.
```

```
## $ANOVA
##   Effect DFn DFd       F            p p<.05       ges
## 1   drug   2  27 27.8381 2.746469e-07     * 0.6734247
## 
## $`Levene's Test for Homogeneity of Variance`
##   DFn DFd       SSn  SSd         F         p p<.05
## 1   2  27 0.8666667 34.5 0.3391304 0.7153804
```

the first argument passed to the `ezANOVA` function is the dataframe holding the data, the second argument `dv`, is the name of the dependent variable, the `between` argument is used to pass the names of the between-subject factor(s), and the `wid` argument is used to pass the name of the variable holding the subject identifiers. The syntax for specifying multiple factors is a bit unusual (but not difficult). For example, if in the above example we had a second between-subject factor called `dose`, to use both factors in the analysis we would have written `between=.(drug, dose)`. We'll see a concrete example with the `ToothGrowth` dataset. In the dataset `dose` is a numeric variable; for this example we'll treat it as a factor, and therefore, after loading the dataset, the code below converts it to a factor:

```r
data(ToothGrowth)
ToothGrowth$dose = factor(ToothGrowth$dose)
ToothGrowth$id = factor(paste0("P", 1:dim(ToothGrowth)[1]))
```

again, we added a subject identifier to the dataset, and we're now ready to run the analysis with the code below:

```r
ezANOVA(ToothGrowth, dv=len, between=.(supp, dose), wid=id)
```

```
## Coefficient covariances computed by hccm()
```

```
## $ANOVA
##      Effect DFn DFd         F            p p<.05       ges
## 1      supp   1  54 15.571979 2.311828e-04     * 0.2238254
## 2      dose   2  54 91.999965 4.046291e-18     * 0.7731092
## 3 supp:dose   2  54  4.106991 2.186027e-02     * 0.1320279
## 
## $`Levene's Test for Homogeneity of Variance`
##   DFn DFd    SSn     SSd        F         p p<.05
## 1   5  54 38.926 246.053 1.708578 0.1483606
```

In order to run repeated measures ANOVAs, in which there are one or more within-subject factors we need to list these factors in the `within` argument. For example, the following dataset contains data from a fictitious experiment on the effects of a drug and of alcohol on social interaction in rats. Two species of rats were tested (the between-subject factor `group`). Each rat was tested with or without the drug, and with or without alcohol. The dependent measure was the number of social interactions recorded by the experimenter. The fictitious data are in the `socialint_drug_alcohol_species.csv` dataset:

```r
dat = read.table("datasets/socialint_drug_alcohol_species.csv",
                 header=T, sep=",", stringsAsFactor=T)
```

next we run the ANOVA with the code below:

```r
ezANOVA(dat, dv=socialint,
        between=.(group),
        within=.(drug, alcohol),
        wid=id,
        type="3")
```

```
## Warning: Converting "id" to factor for ANOVA.
```

```
## $ANOVA
##               Effect DFn DFd            F            p p<.05          ges
## 2              group   1  14 4.615385e-01 5.079831e-01       2.290076e-02
## 3               drug   1  14 8.104819e+01 3.376241e-07     * 3.848618e-01
## 5            alcohol   1  14 2.276577e+01 2.982114e-04     * 1.903005e-01
## 4         group:drug   1  14 6.831325e+00 2.042593e-02     * 5.009276e-02
## 6      group:alcohol   1  14 6.306306e-02 8.053669e-01       6.506181e-04
## 7       drug:alcohol   1  14 1.600000e+01 1.316049e-03     * 4.000000e-02
## 8 group:drug:alcohol   1  14 4.437343e-31 1.000000e+00       1.155558e-33
```

note that we added a `type` argument, this specifies the sum of squares type to be used for the analysis. In balanced designs, in which you have equal numbers of observations per cell this won't make a difference, but for unbalanced designs it does. There is some controversy over the most appropriate sum of squares type to use, and the most appropriate choice may depend on the goals and the context of the sudy/analysis. You can read more about the different sum of square types [here](http://md.psych.bio.uni-goettingen.de/mv/unit/lm_cat/lm_cat_unbal_ss_explained.html).
