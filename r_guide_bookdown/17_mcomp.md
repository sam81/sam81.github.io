# Adjusting the *p*-values for Multiple Comparisons



The base R program comes with a number of functions to perform multiple comparisons. Not all the possible procedures are available, and some of them might not be applicable to the object resulting from the specific analysis you're doing. Additional packages might cover your specific needs.

#### The `p.adjust` function

The function `p.adjust` comes with  the base R program, in the package package stats, so you don't need to install anything else on your machine apart from R to get it. This function takes a vector of p-values as an argument, and returns a vector of adjusted p-values according to one of the following methods:

- `holm`
- `hochberg`
- `hommel`
- `bonferroni`
- `BH`
- `BY`
- `fdr`
- `none`

So, if for example you've run 3 *t*-test after a one-way ANOVA with 3 groups, to compare each mean group's mean with the others, and you want to correct the resulting *p*-values with the Bonferroni procedure, you can use `p.adjust` as follows:


```r
ps = c(0.001, 0.0092, 0.037) #p values from the t-tests
p.adjust(ps, method="bonferroni")
```

```
## [1] 0.0030 0.0276 0.1110
```

the last line of the example gives the p-values corrected for the number of comparisons made (3 in our case), using the Bonferroni method. As you can see from the output, the first 2 values would still be significant after the correction, while the last one would not. 

You can use any of the procedures listed above, instead of the Bonferroni procedure, by setting it in the `method` option. If you don't set this option at all you get the Holm procedure by default. Look up the Reference Manual for further information on these methods.

The method `none` returns the p-values without any adjustment.


#### The `mt.rawp2adjp` function

The package multtest contains a function very similar to `p.adjust`, and offers some other correction methods.

```r
library(multtest)
ps = c(0.001, 0.0092, 0.037) #p values from the t-tests
mt.rawp2adjp(ps, proc="Bonferroni")
```

```
## $adjp
##        rawp Bonferroni
## [1,] 0.0010     0.0030
## [2,] 0.0092     0.0276
## [3,] 0.0370     0.1110
## 
## $index
## [1] 1 2 3
## 
## $h0.ABH
## NULL
## 
## $h0.TSBH
## NULL
```

or, if you want to see the adjusted p-values with more than one method at once:


```r
library(multtest)
ps <- c(0.001, 0.0092, 0.037) #p values from the t-tests
mt.rawp2adjp(ps, proc=c("Bonferroni", "Holm", "Hochberg", "SidakSS"))
```

```
## $adjp
##        rawp Bonferroni   Holm Hochberg     SidakSS
## [1,] 0.0010     0.0030 0.0030   0.0030 0.002997001
## [2,] 0.0092     0.0276 0.0184   0.0184 0.027346859
## [3,] 0.0370     0.1110 0.0370   0.0370 0.106943653
## 
## $index
## [1] 1 2 3
## 
## $h0.ABH
## NULL
## 
## $h0.TSBH
## NULL
```
