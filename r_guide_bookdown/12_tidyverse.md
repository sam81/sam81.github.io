# Tidyverse {#tidyverse}



Tidyverse is a collection of R packages that used in conjuction significantly change the way of working with R.

`ggplot2` and `dplyr` are probably the most popular packages in `tidyverse`. When you start using `ggplot2` it becomes almost essential to use `dplyr` too because `ggplot2` is heavily based on dataframes or tibbles as input data. Before covering `dplyr` we will explain what tibbles are in the next section because `dplyr` returns tibbles as objects.

## Tibbles {#tibbles}

Tibbles are a modern take on dataframes (which we introduced in Section \@ref(dataframes)). Tibbles are in fact very similar to dataframes, but if you're used to dataframes, there are a few things that may surprise you when you start using tibbles. 

Tibbles can be constructed with the `tibble` function, which is similar to the `data.frame` function for constructing dataframes:

```r
library(tibble)
dat = tibble(x=rnorm(20), y=rep(c("A", "B"), each=10))
```

They can be easily converted to dataframes with the `as.data.frame` function if needed:

```r
dat_frame = as.data.frame(dat)
```

and dataframes can also be easily converted to tibbles with the `as_tibble` function:

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
##  1  0.0983
##  2 -0.280 
##  3  1.02  
##  4  0.0584
##  5 -0.417 
##  6 -1.74  
##  7  0.365 
##  8 -0.317 
##  9 -1.37  
## 10 -0.501
```

but it returns a 1-column tibble instead. Furthermore trying `as.numeric(dat[dat$y=="A", "x"])` to convert it to a vector doesn't work. The solution is to use the `pull` function from `dplyr`:


```r
library(dplyr)
pull(dat[dat$y=="A", ], x)
```

```
##  [1]  0.09831035 -0.27951673  1.02105526  0.05843470 -0.41656644 -1.73980278
##  [7]  0.36467128 -0.31725294 -1.36515908 -0.50087454
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
## t = -0.52826, df = 14.151, p-value = 0.6055
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -1.3785623  0.8332464
## sample estimates:
##   mean of x   mean of y 
## -0.30767009 -0.03501214
```

## `dplyr`

`dplyr` is the Swiss army knife of dataframe manipulation. It can be used to take subsets of the rows of a dataframe on the basis of the values of one or more variables (`filter` function). It can be used to take subsets of the column of a dataframe (`select` function). It can be used to create new columns that are functions of one or more rows of a dataframe (`mutate` function). It can be used to create dataframes that summarize variables of another dataframe on the basis of factor groupings (`summarize` combined with `group_by` functions). All these operations could be done also without `dplyr` through base R functions. However, `dplyr` really shines in making these operations seamless and straightforward, with a concise sintax.

A key element that allows you to seamlessly manipulate dataframes with `dplyr` with a concise sintax is the pipe operator `%>%`. This operator comes from the `magrittr` package that is automaticaly loaded when you lod `dplyr`. Let's suppose that we want to take the subset of the `iris` dataframe consisting of observations for the species setosa, and we're only interested in the columns for petal length and width. We can do this without using the pipe operator as follows:

```r
iris_set = filter(iris, Species=="setosa")
iris_set = select(iris_set, Petal.Length, Petal.Width)
```

when used without the pipe operator, the first argument that we pass to the `filter` and `select` functions is the dataframe on which they need to operate. With the pipe we don't need to pass this argument to the functions, we simply "pipe" the dataframe through as follows:

```r
iris_set = iris %>% filter(Species=="setosa") %>% select(Petal.Length, Petal.Width)
```

this way we've "chained" two operations and accomplised our aim with a single, less verbose, line of code! You can think of the `%>%` operator as taking the output of the function on its left and passing it forward to the function on its right for further processing.

### `group_by` and `summarize`

The `group_by` function generates a "grouped" tibble:

```r
grIris = iris %>% group_by(Species);
str(grIris)
```

```
## tibble [150 × 5] (S3: grouped_df/tbl_df/tbl/data.frame)
##  $ Sepal.Length: num [1:150] 5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
##  $ Sepal.Width : num [1:150] 3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
##  $ Petal.Length: num [1:150] 1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
##  $ Petal.Width : num [1:150] 0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
##  $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...
##  - attr(*, "groups")= tibble [3 × 2] (S3: tbl_df/tbl/data.frame)
##   ..$ Species: Factor w/ 3 levels "setosa","versicolor",..: 1 2 3
##   ..$ .rows  : list<int> [1:3] 
##   .. ..$ : int [1:50] 1 2 3 4 5 6 7 8 9 10 ...
##   .. ..$ : int [1:50] 51 52 53 54 55 56 57 58 59 60 ...
##   .. ..$ : int [1:50] 101 102 103 104 105 106 107 108 109 110 ...
##   .. ..@ ptype: int(0) 
##   ..- attr(*, ".drop")= logi TRUE
```

this is useful because subsequent functions in the pipeline operate on the groups defined with `group_by`. For example, the following lines will compute the mean petal length for each species:

```r
iris %>% group_by(Species) %>% summarize(meanPetLen=mean(Petal.Length))
```

```
## # A tibble: 3 x 2
##   Species    meanPetLen
##   <fct>           <dbl>
## 1 setosa           1.46
## 2 versicolor       4.26
## 3 virginica        5.55
```

note that we can group by more than one variable, and we can compute more than one statistics in the `summarize` call, as shown in the following example which uses the `mpg` dataset provided by `ggplot2`:

```r
library(ggplot2)
mpg_grpd = mpg %>% group_by(manufacturer, trans) %>% summarize(meanHWY=mean(hwy),
                                                               sdHWY=sd(hwy),
                                                               meanCTY=mean(cty),
                                                               sdCTY=sd(cty))
head(mpg_grpd)
```

```
## # A tibble: 6 x 6
## # Groups:   manufacturer [2]
##   manufacturer trans      meanHWY sdHWY meanCTY sdCTY
##   <chr>        <chr>        <dbl> <dbl>   <dbl> <dbl>
## 1 audi         auto(av)      28.5  2.12    19.5  2.12
## 2 audi         auto(l5)      25.8  1.92    16    1.22
## 3 audi         auto(s6)      25    1.63    17.2  1.26
## 4 audi         manual(m5)    26.5  1.73    18.5  1.73
## 5 audi         manual(m6)    28    3       18.3  2.89
## 6 chevrolet    auto(l4)      20.6  5.43    14.7  3.36
```

### Getting counts of cases


```r
y = rnorm(100, mean=100, sd=20)
fct = rep(c("A", "B", "C", "D"), each=25)
dat = data.frame(fct=fct, y=y)
dat %>% group_by(fct) %>% filter(y>100) %>% summarize(n())
```

```
## # A tibble: 4 x 2
##   fct   `n()`
##   <chr> <int>
## 1 A        13
## 2 B        16
## 3 C         8
## 4 D        15
```

```r
## shorthand version
dat %>% filter(y>100) %>% count(fct)
```

```
##   fct  n
## 1   A 13
## 2   B 16
## 3   C  8
## 4   D 15
```

```r
## alternative version
dat %>% group_by(fct) %>% filter(y>100) %>% tally()
```

```
## # A tibble: 4 x 2
##   fct       n
##   <chr> <int>
## 1 A        13
## 2 B        16
## 3 C         8
## 4 D        15
```

## Tests statistics with `dplyr` and `broom`

The [`broom`](https://cran.r-project.org/web/packages/broom/) package converts the output of some R models to tibbles. As we will see below, used in conjunction with `dplyr` this provides a very efficient way of fitting models or performing statistical tests on grouped data. Let's first see an example of what `broom` does. We'll fit a simple linear regression model using the `iris` dataset. We'll first fit the model on the data of the setosa species only:

```r
data(iris)
fit_single = lm(Petal.Length~Sepal.Length, data=iris %>% filter(Species=="setosa"))
```

if we run `str(fit_single)` we can see that the output of `lm` is a list. We can get a nice printout of the results with `summary(fit_single)`, but while easy to read, such output is again stored in a list (you can check with `str(summary(fit_single))`) which is not easy to store or process further (e.g. combining the output with that of other model fits and storing in a CSV file). `broom` aims to solve this issue. The `tidy` function turns the coefficients and test statistics for each term of the model into a tibble:

```r
library(broom)
fit_single_tidy = tidy(fit_single)
```

the `glance` function turns parameters and statistics *global* to the model (as opposed to being specific to a given term) into a tibble:

```r
fit_single_glance = glance(fit_single)
```

finally the `augment` function computes statistics such as the fitted values and residuals that can be added back to the input dataframe, thus *augmenting* it.

```r
iris_aug = iris %>% group_by(Species) %>% summarize(lmfit=augment(lm(Petal.Length~Sepal.Length)))
```

Combined with `dplyr` the `broom` function makes fitting statistical models on grouped data efficient. Below we'll use again the `iris` dataset, but we'll use the whole dataset, fitting a separate model for each iris species in one go:

```r
fit_frame_tidy = iris %>% group_by(Species) %>% summarize(lmfit=tidy(lm(Petal.Length~Sepal.Length)))
fit_frame_glance = iris %>% group_by(Species) %>% summarize(lmfit=glance(lm(Petal.Length~Sepal.Length)))
```
