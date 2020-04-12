# Tidyverse {#tidyverse}



Tidyverse is a collection of R packages that used in conjuction significantly change the way of working with R.

`ggplot2` and `dplyr` are probably the most popular packages in `tidyverse`. When you start using `ggplot2` it becomes almost essential to use `dplyr` too because `ggplot2` is heavily based on dataframes or tibbles as input data. Before covering `dplyr` we will explain what tibbles are in the next section because `dplyr` returns tibbles as objects.

## Tibbles

Tibbles are a modern take on dataframes (which we introduced in Section \@ref(datamanip)). Tibbles are in fact very similar to dataframes, but if you're used to dataframes, there are a few things that may surprise you when you start using tibbles. 

Tibbles can be constructed with the `tibble` function, which is similar to the data.frame function for constructing dataframes:


```r
library(tibble)
dat = tibble(x=rnorm(20), y=rep(c("A", "B"), each=10))
```

They can be easily converted to dataframes with the `as.data.frame` function if needed:


```r
dat_frame = as.data.frame(dat)
```

and dataframes can also easily converted to tibbles with the `as_tibble` function:


```r
dat2 = as_tibble(dat_frame)
```

One feature of tibbles that may surprise you is the fact that, by default, when you use a print method on a tibble only a limited number of rows and columns are shown. This is for me a very annoying thing, because when I load a dataframe I want to see **all** the columns with the variable names (typically I do this with `head(df)`). Luckily you can change these defaults by specifying the `tibble.print_max` and `tibble.width` options. You can do this in your `.Rprofile` file in your home directory so that your preference sticks across sessions:

```r
## This goes in .Rprofile in ~/
.First <- function(){
    .libPaths(c((.libPaths()), "~/.R_library/"))
    options(tibble.print_max = Inf)
    options(tibble.width = Inf)
}
```

Another aspect of tibbles that may surprise dataframe users is their behavior *re* subsetting. For example, you might expect that the following returns a vector:


```r
dat[dat$y=="A", "x"]
```

```
## # A tibble: 10 x 1
##          x
##      <dbl>
##  1  0.465 
##  2  0.447 
##  3  1.08  
##  4 -0.451 
##  5 -0.600 
##  6  0.741 
##  7 -0.0313
##  8  0.445 
##  9 -0.220 
## 10  1.32
```

but it returns a 1-column tibble instead. Furthermore trying `as.numeric(dat[dat$y=="A", "x"])` to convert it to a vector doesn't work. The solution is to use the `pull` function from `dplyr`:


```r
library(dplyr)
pull(dat[dat$y=="A", ], x)
```

```
##  [1]  0.46466777  0.44677708  1.08058815 -0.45116775 -0.60039529  0.74099766
##  [7] -0.03128845  0.44525450 -0.21962249  1.31834134
```

More succintly, if, for example, you wanted to run a $t$-test on the groups indicated by the `y` variable, you could do the following through `dplyr`:


```r
t.test(dat %>% filter(y=="A") %>% pull(x),
       dat %>% filter(y=="B") %>% pull(x))
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  dat %>% filter(y == "A") %>% pull(x) and dat %>% filter(y == "B") %>% pull(x)
## t = 1.0118, df = 14.1, p-value = 0.3287
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -0.4687659  1.3071513
## sample estimates:
##   mean of x   mean of y 
##  0.31941525 -0.09977749
```



