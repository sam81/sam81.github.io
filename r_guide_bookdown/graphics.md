# Graphics



There are four main graphics libraries that can be used in R. The first is the base graphics system that comes builtin with every R installation and will be described in this chapter. The other three main graphics libraries (`ggplot2`, `lattice`, and `plotly`), can be installed as additional packages. Each of these libraries provide a complete, independent system to generate graphics in R. Choosing one library over the other is mostly a matter of personal preference because pretty much any graphic that can be built with one library can also be built with the others. Some graphics are easier to build with one library than another and vice-versa. In recent years `ggplot2` has gained lots of popularity and `lattice` is less popular than it was some years ago. Despite the increasing popularity of `ggplot2` the base R graphics that are described in this chapter are still widely used. `plotly` is a recent entry and is somehow a special case because it is primarily designed to generate interactive graphics that can be displayed on html pages, while the other graphics libraries have very limited interactive functionality and are primarily designed to generate static high-quality graphics. `plotly` is special also because through the `ggplotly` function it can convert a `ggplot2` graph into an interactive `plotly` graphic.

Because the base R graphics library is still widely used and (in my opinion) is somewhat simpler to use for beginners, I would recommend learning it first. The `ggplot2` library is described in chapter \@ref(ggplot2), the `lattice` graphics library is described in chapter \@ref(lattice), and the `plotly` library is described in chapter \@ref(plotly).

## Overview of R base graphics functions

  Function
  --------
  `plot`
  `barplot`
  `boxplot`
  `histogram`
  `matplot`
  `stripchart`
  `interaction.plot`

## The `plot` Function {#plot}

The `plot` function is most commonly used to draw a scatterplot of two variables, however if given a R object with a plot method as an argument  it will produce different types of graphics depending on the object it is plotting.
Let's see an example of a scatterplot with some simulated data:

```r
  a = rnorm(5, 1.6, n=50)
  b = rnorm(15, 4.3, n=50)
```
this creates two vectors of length 50 with values normally distributed, now we can plot the values of vector `b`, against the values of vector `b` with:

```r
plot(x=a, y=b) ## or for short plot(a, b)
```

<div class="figure">
<img src="graphics_files/figure-html/scatterplot-1.png" alt="A scatterplot." width="576" />
<p class="caption">(\#fig:scatterplot)A scatterplot.</p>
</div>

the resulting scatterplot appears in Figure \@ref(fig:scatterplot).

If we were to plot the change of a variable over time it could be a good idea to connect the values at different time points in the plot with lines, this is easily achieved setting the option `type`. Below is an example, the result is shown in Figure \@ref(fig:timewlines).

```r
ti=1:50
b=rnorm(15,4.3,n=50)
plot(ti, b, type='l')
```

<div class="figure">
<img src="graphics_files/figure-html/timewlines-1.png" alt="Values connected by lines." width="576" />
<p class="caption">(\#fig:timewlines)Values connected by lines.</p>
</div>

Some other possible values for the option `type` are `p` for points (the default), `b` for *both* points and lines, and `o` for overplotted points and lines (very similar to `b`).

##Drawing Functions

The `plot` function can be used for drawing mathematical functions, for example:

```r
vec = seq(from=0, to=4*pi, length=120)
plot(vec,sin(vec), type="l")
```

<img src="graphics_files/figure-html/unnamed-chunk-2-1.png" width="576" />

in this case is necessary to use `l` as the type of plot, otherwise the plot would resemble a messy scatterplot. It is also possible to plot markers on the points in which the function is actually evaluated. Using the option `b` for the plot type, both a continuous line and markers are plotted.
There are lots of options to define the appearance of a plot, next comes an overloaded example used just to introduce some of these options. A more detailed description is given in Section \@ref(par).

### The `matplot` function

Plotting the sine and cosine functions together with the `matplot` function, the result is in Figure \@ref(fig:sincos)


```r
a= seq(from=0, to= 2*pi, length=20)
s = sin(a)
c = cos(a)
aa = cbind(a,a) #matrix of hor coord
cs = cbind(c,s) #matrix of vert coord
matplot(x=aa, y=cs, type="l", lwd=1.8, ylab="sine and cosine functions")
```

<div class="figure">
<img src="graphics_files/figure-html/sincos-1.png" alt="Sine and cosine functions with `matplot`." width="576" />
<p class="caption">(\#fig:sincos)Sine and cosine functions with `matplot`.</p>
</div>

## Barplots

Barplots sometimes come in handy when you want to summarise your data. Suppose we have administered a test to three different groups of people, which we will designate as `a`, `b` and `c`. We want to summarise and compare the performance of each group with a nice graph, a barplot will make it. The data are stored in the file `test.txt`, in the format shown in Table \@ref(tab:test).

**a** **b** **c**
----- ----- -----
4     6     5
5     8     3
3     7     8
6     5     4
5     9     9
7     7     8
5     6     5
8     5     7
5     8     6
4     10    4

Table: (\#tab:test) Data for the test example.


We can read in the file directly as a dataframe:

```r
test = read.table("datasets/test.txt", header=TRUE)
```

We want to use the `tapply` function to get quickly summary tables
with the means and standard deviations for the three groups. However
the format of the data frame at this point is not suitable for the
`tapply` function, because it has 3 observations for each row, and the
`tapply` function can be used only with the format "one row per
observation", in which we have the values of the observations in one
column and a set of "labels" identifying the group to which a given
observation belongs to in another column. Fortunately, we can easily
change the format of our dataframe with the `stack` command. What it
does is just to create a single "values" vector from the three columns we had previously, and to add automatically another "index"  vector with the labels we need:


```r
test2 = stack(test)
```

we can have a look at the first elements of the new dataframe typing:


```r
head(test2, n=10)
```

```
##    values ind
## 1       4   a
## 2       5   a
## 3       3   a
## 4       6   a
## 5       5   a
## 6       7   a
## 7       5   a
## 8       8   a
## 9       5   a
## 10      4   a
```

please notice that the `stack` function has automatically named the two vectors `values` and `ind`, we need to know these names to use the `tapply` function.

Now we'll create the two summary tables using the `tapply` function, one with the means and one with the standard deviations for the three groups:


```r
test_means = tapply(X=test2$values, IND=test2$ind, FUN=mean)
test_sd = tapply(X=test2$values, IND=test2$ind, FUN=sd)
```

Now we can draw a simple barplot displaying the means for each group:


```r
barplot(test_means, col=c("darkred","salmon2","plum4"))
```

<div class="figure">
<img src="graphics_files/figure-html/barplotsimple-1.png" alt="Simple barplot." width="576" />
<p class="caption">(\#fig:barplotsimple)Simple barplot.</p>
</div>

we've added colours to the graph using the `col` option. You can look at the resulting graph in Figure \@ref(fig:barplotsimple).

You can control the width of the bars specifying the `width` option, and setting the range for the x axis with the `xlim` option, specifying only the width does nothing, you must also set the `xlim`. You set `xlim` with a vector of the form `xlim = c(from, to)`, in which `from` is the origin and `to` is the end of the axis. In the example below we will set `xlim` to go from 0 to 3 and the bars to have a width of 0.5:


```r
barplot(test_means,col=c("darkred","salmon2","plum4"),
        xlim = C(0,3), width=0.5)
```

this sets the width of all bars at 0.5, we could specify the width for each single bar instead, by giving to `width` a vector with the width values for each bar.

You can also control the spacing between the bars with the `space` option. The default is set to 0.2.

### Barplots with Error Bars

Now let's say we want to get the same barplot but with error bars showing the standard deviation for each group. We could achieve this result adding lines to the current graph, but there is a better option, we can use the `barplot2` command, which comes with the `gplots` library and provides an easy way of adding error bars to a barplot. So once we have the `gplots} package installed we first load it:


```r
library(gplots)
```

and then we can draw our barplot with error bars. To get them, we need to set the option `plot.ci=TRUE` and then specify the upper and lower bounds of the error bars with the `ci.u` and `ci.l` commands. So we first create the values for `ci.u` and `ci.l`, so that the error bars represent one standard deviation around the mean. We'll use the values from the two tables we had created before, with means and standard deviations for the three groups:


```r
upper = test_means + (test_sd)
lower = test_means - (test_sd)
```

Now we can draw our barplot, you can see the result in Figure \@ref(fig:barplotbars):


```r
barplot2(test_means,col=c("darkred","salmon2","plum4"),
         plot.ci=TRUE,ci.u=upper,ci.l=lower)
```

<div class="figure">
<img src="graphics_files/figure-html/barplotbars-1.png" alt="Barplot with error bars." width="576" />
<p class="caption">(\#fig:barplotbars)Barplot with error bars.</p>
</div>



## Boxplots

Boxplots can be used to visualise the central tendency and the dispersion of the data for a given sample, and to directly compare these same characteristics for different samples. Let's look at one of them, the data are organised in a dataframe in the file `boxplot1.txt`. They are the scores for two different groups (group a and group b) in a test. With the boxplot we want to see how the scores are distributed in the two groups. The dataframe contains a column `score`, with the score for each subject and another column `group` that defines the group a given observation comes from. So we first read in the dataframe, and then ask for the boxplots with the distribution of scores, as a function of the group they belong to.


```r
dats = read.table("datasets/boxplot1.txt", header=TRUE)
boxplot(dats$score~dats$group, names=c("group a", "group b"))
```

<div class="figure">
<img src="graphics_files/figure-html/boxplot1-1.png" alt="Boxplots comparing the distribution for two groups." width="576" />
<p class="caption">(\#fig:boxplot1)Boxplots comparing the distribution for two groups.</p>
</div>

The results are in Figure \@ref(fig:boxplot1), the thick black lines in the middle of the boxes represent the median, if this is about the middle of the box, the distribution of the data should be normal. The two lines that delimit the box are called "hinges", and they are approximately the first and the third quartiles. The horizontal lines that form the `Ts' above and below the box are called ``whiskers'', and inside them are contained all the observations that fall within a distance of 1.5 times the size of the box, upwards or downwards. Points that fall outside this distance are outliers and they are represented as a circle. In our case there are two outliers in group a. Apart from checking if the distribution is normal, you can also check if the variances are approximately equal, by comparing the size of the boxes (the distance between the hinges). If one boxplot is clearly bigger than the other one (for example two times bigger), then the variances for the two groups are likely not to be equal.



## Histograms

Histograms can be used to visualise the distribution of a sample, the function `hist`, can be used to plots a histogram of frequencies (counts) of the sample, or of its density function (setting the option `freq=F`). Let's first create a sample with a normal distribution, and then plot its histogram, the result is in Figure \@ref(fig:hist).


```r
my_distr = rnorm(100, 5, 1.7)
hist(my_distr)
```

<div class="figure">
<img src="graphics_files/figure-html/hist-1.png" alt="Frequency distribution of a random sample." width="576" />
<p class="caption">(\#fig:hist)Frequency distribution of a random sample.</p>
</div>

## Stripcharts

If the groups  contain a small number of observations, it might be better to use  a stripchart to visualise their distributions. In a stripchart each point represents a single observation. By default they are drawn on a line, so if two observations have the same score, they overlap. To avoid this problem you can give a certain amount of jitter to the plot, so that observations with the same score are scattered a little and can be easily distinguished. Here's the code for producing a stripchart, the data are in the file `stripchart1.txt` and they are arranged in the same way as the data in `boxplot1.txt`, just the sample sizes are smaller, with 6 observations per group.


```r
stripchart(dats$score~dats$group, method="jitter",
           jitter=0.1, pch=1, vertical=TRUE)
```

<div class="figure">
<img src="graphics_files/figure-html/stripchart1-1.png" alt="Stripchart example." width="576" />
<p class="caption">(\#fig:stripchart1)Stripchart example.</p>
</div>


## Interaction Plots

Interaction plots can be used to visualise the means for the levels of a factor, at the levels of another factor, for example in a two-way ANOVA design, and in this way they allow to spot possible interactions between the two factors involved. The following graph comes from a two-way ANOVA design, in which the variable of interest was the proportion of errors in a task, as a function of the spatial congruency and stimulus onset asincrony (SOA), of a distracting stimulus presented during the execution of the task.


```r
dats = read.table('datasets/direrr_all.txt', header=T)
levels(dats$congr)[1] = 'c - congruent'
levels(dats$congr)[2] = 'a - inc. goal-directed'
levels(dats$congr)[3] = 'b - inc. no-goal-directed'

interaction.plot(dats$SOA,dats$congr,dats$errors,
                 ylab="Proportion of errors", xlab="SOA",
                 trace.label="Congruency",
                 lty=c("solid", "dashed", "dotdash"))
```

<div class="figure">
<img src="graphics_files/figure-html/int-1.png" alt="Interaction plot with different line types." width="576" />
<p class="caption">(\#fig:int)Interaction plot with different line types.</p>
</div>

well the essential ingredients to get the plot are just the first
three arguments (the others are optional), the default order in which
you have to give them is a bit awkward, because the variable of
interest (in our case `dats$errors`) is the third argument, the
first one is the factor that goes on the x axis, and the second one is
the *trace factor*, whose levels will be represented as lines of
a different type, or of different colours. The y axis yields the
measure for our variable of interest. You can see the plot in
Figure \@ref(fig:int). Another good way to represent the levels for the
trace factor, is through the use of symbols, in the following graph
(see the result in Figure \@ref(fig:intb)) both line type `lty` and
symbols `pch` are used to differentiate between the levels of the
trace factor, in order to get this you need to set the option `type` to `b` that means use both line type and points (symbols), setting this option to `l` will give just different line types while setting it to `p`, will give just different symbols. In these examples I specified the line types and the symbols to use, but this is not necessary, if you don't, R will cycle through its different line types and/or symbols as necessary to represent all the levels of the trace factor. See Section \@ref(lty) for a description of the different line types and Section \@ref(secpch) for a description of the different symbols available in R.


```r
interaction.plot(dats$SOA,dats$congr,dats$errors,
                 ylab="Proportion of errors", xlab="SOA",
                 trace.label="Congruency",
                 lty=c("solid", "dashed", "dotdash"),
                 type="b",pch=c(0,15,17))
```

<div class="figure">
<img src="graphics_files/figure-html/intb-1.png" alt="Interaction plot with different line types and different symbols." width="576" />
<p class="caption">(\#fig:intb)Interaction plot with different line types and different symbols.</p>
</div>


