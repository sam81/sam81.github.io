# Lattice graphics {#lattice}



The `lattice` package provides an alternative to the base R graphics system; it is an implementation of the ideas developed and implemented mainly by Rick Becker and Bill Cleveland in the Trellis graphics system for the S language [@Adler2010; @Crawley2013]. Trellis displays were developed as a framework to easily display of the relationship between a dependent variable and multiple factors. 

The best introduction to `lattice` graphics is the book by @Sarkar2008, the author of `lattice`. The `lattice` package documentation, available [here](https://cran.r-project.org/web/packages/lattice/lattice.pdf), is a good reference. The articles of @Sarkar2002, @Sarkar2003, and the book chapter of @Murrell2001 are good introductory resources.

Because `lattice` is mostly compatible with the Trellis graphics system in [S-Plus](https://en.wikipedia.org/wiki/S-PLUS), articles/documents written for Trellis [e.g. @BeckerEtAl1996; @BeckerEtAl1996b; @ClevelandAndFuentes1997; @BeckerAndCleveland2002], also provide a good introduction to `lattice` and to the concept of trellis displays.

## Overview of lattice graphics

Table \@ref(tab:latticefunctions) lists some of the most common high-level `lattice` plotting functions. We'll look at some of these in the following sections, but first we'll describe the model formulae system used by `lattice` and introduce the concept of multi-panel conditioning in the next section.

  Function       Chart types
  ---------      -----------------
  `xyplot`       scatterplot, time-series, interaction plot
  `barchart`     barplot
  `dotplot`      dotplot
  `stripplot`    stripplot
  `bwplot`       box-and-whisker 

Table: (\#tab:latticefunctions) Some high-level plotting functions in `lattice`.

## Introduction to model formulae and multi-panel conditioning {#lattmpancond}

To illustrate how conditioning on one or more factors works in `lattice` we'll use the `npk` dataset, which contains data from a factorial experiment on the effects of N, P, K (nitrogen, phosphate, potassium) on the growth of peas:

```r
data(npk)
npk$P = factor(npk$P, levels=c("0", "1"), labels=c("P=0", "P=1"))
npk$K = factor(npk$K, levels=c("0", "1"), labels=c("K=0", "K=1"))
```

in the dataset `N`, `P`, and `K` are indicator variables for the application of the respective element (0=no, 1=yes). We renamed the levels of the P and K factors to make legends and strip labels easier to read. We'll first plot the mean yield as a function of N, averaging across the levels of the other factors. In order to do this we'll generate a summary data set:

```r
library(dplyr)
npk_summ_N = npk %>% group_by(N) %>% summarize(mean_yield=mean(yield),
                                               sd_yield=sd(yield))
```

Two basic elements of `lattice` plots consist of a dataframe, and a model formula specifying how you want a given variable to be displayed along the dimensions of one or more factors. In our case, we want `mean_yield ~ N` (you could read the `~`, "as a function of"), the barchart can be produced with the code below, and is displayed in Figure \@ref(fig:lattnpkp1):

```r
library(lattice)
##trellis.device() #optional opens an X11() window on Linux
pl1 = barchart(mean_yield ~ N, data=npk_summ_N,
               xlab="N", ylab="Mean yield")
print(pl1)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattnpkp1-1.png" alt="Yield as a function of nitrogen application." width="326.4" />
<p class="caption">(\#fig:lattnpkp1)Yield as a function of nitrogen application.</p>
</div>

Now suppose we want to visualise the relation between `yield` and N depending on P administration. In lattice there are two ways of doing it, one is by the use of a "grouping" factor. The second way is by multi-panel factor conditioning. The advantage of multi-panel conditioning, as we will see soon, is that it can be extended to an unlimited number of factors. Let's start with the first solution, in which we use a grouping factor. We first generate a summary dataset as a function of both N and P with:

```r
npk_summ_NP = npk %>% group_by(N,P) %>% summarize(mean_yield=mean(yield),
                                                  sd_yield=sd(yield))
```

and then generate the barplot (shown in Figure \@ref(fig:lattnpkp2)):

```r
pl2 = barchart(mean_yield ~ N, groups=P, 
               data=npk_summ_NP, auto.key=TRUE,
               xlab="N", ylab="Mean yield")
print(pl2)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattnpkp2-1.png" alt="Barplot with bars side by side showing yield as a function of nitrogen and potassium application." width="326.4" />
<p class="caption">(\#fig:lattnpkp2)Barplot with bars side by side showing yield as a function of nitrogen and potassium application.</p>
</div>

the grouping factor is given by the `groups` argument, note also that we have set `auto.key` to `TRUE` in order to automatically add a legend.

The second way of building the graph is by multi-panel conditioning. We achieve this by setting P as a conditioning factor with the `|` operator. The model formula becomes `mean_yield ~ N | P`, which you could read as "mean yield as a function of N, given P`. The plot shown in Figure \@ref(fig:lattnpkp3):

```r
pl3 = barchart(mean_yield ~ N | P, data=npk_summ_NP,
               xlab="N", ylab="Mean yield")
print(pl3)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattnpkp3-1.png" alt="Barplot showing yield as a function of nitrogen and potassium application using multi-panel conditioning." width="326.4" />
<p class="caption">(\#fig:lattnpkp3)Barplot showing yield as a function of nitrogen and potassium application using multi-panel conditioning.</p>
</div>

We can visualize the effect of all three factors (N, P, K) by showing mean values of K along different panels. We'll first generate a summary dataset as a function of all three factors:

```r
npk_summ_NPK = npk %>% group_by(N,P,K) %>% summarize(mean_yield=mean(yield),
                                                     sd_yield=sd(yield))
```

and then generate the plot (shown in Figure \@ref(fig:lattnpkp4)):

```r
pl4 = barchart(mean_yield ~ N | P+K, data=npk_summ_NPK,
               xlab="N", ylab="Mean yield")
print(pl4)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattnpkp4-1.png" alt="Barplot showing yield as a function of nitrogen and potassium application. Panels plot mean yield as a function of the application of phosphate (0=no, 1=yes)." width="489.6" />
<p class="caption">(\#fig:lattnpkp4)Barplot showing yield as a function of nitrogen and potassium application. Panels plot mean yield as a function of the application of phosphate (0=no, 1=yes).</p>
</div>

Also in this case we could have used a grouping variable rather than using two conditioning factors (Figure \@ref(fig:lattnpkp5)):

```r
pl5 = barchart(mean_yield ~ N | P, groups=K, data=npk_summ_NPK,
               xlab="N", ylab="Mean yield", auto.key=T)
print(pl5)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattnpkp5-1.png" alt="Barplot showing yield as a function of nitrogen, phosphate, and potassium application. Panels plot mean yield as a function of the application of potassium (0=no, 1=yes)." width="326.4" />
<p class="caption">(\#fig:lattnpkp5)Barplot showing yield as a function of nitrogen, phosphate, and potassium application. Panels plot mean yield as a function of the application of potassium (0=no, 1=yes).</p>
</div>

what is the best display is up to the user to decide, and depends from case to case.

## Common charts {#lattcommon}

### `xyplot`

The `xyplot` function can be used to generate scatterplots and time-series plots.

We'll plot a scatterplot using the `iris` dataset (plot shown in Figure \@ref(fig:lattiris2)):

```r
p = xyplot(Sepal.Length~Sepal.Width, groups=Species, data=iris, auto.key=T)
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattiris2-1.png" alt="Scatterplot of sepal length by sepal width for the iris measurements in the `iris` dataset." width="326.4" />
<p class="caption">(\#fig:lattiris2)Scatterplot of sepal length by sepal width for the iris measurements in the `iris` dataset.</p>
</div>

We can also obtain points connected by lines by passing the `type="b"` argument to `xyplot`. To illustrate this we'll plot a time series using the `Orange` built-in R datasets, which contains data on the growth of five orange trees (plot shown in Figure \@ref(fig:lattorange1)):

```r
xyplot(circumference~age, group=Tree, data=Orange,
       type="b", as.table=T,
       xlab="Age (days)", ylab="Circumference (mm)",
       auto.key=list(title="Tree", space="right"))
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattorange1-1.png" alt="Growth of orange trees: trunk circumference by age (in days) for five trees." width="326.4" />
<p class="caption">(\#fig:lattorange1)Growth of orange trees: trunk circumference by age (in days) for five trees.</p>
</div>

### `barchart` {#lattbarchart}

We have already seen a few examples of barcharts built with `lattice` in Section \@ref(lattmpancond). We'll look at a few more examples in this section.

The following dataset contains both positive and negative values:

```r
dat = data.frame(y=c(-0.5, 0.7, 1, -0.8),
                 fct1=c("A", "A", "B", "B"),
                 fct2 = c("1", "2", "1", "2")
                 )
```

in order to plot a barchart with bars going either up or down from a baseline value of zero we have to pass the argument `origin=0`. We'll also change the fill color of the bars by changing the `col` attribute in the `superpose.polygon` list of settings (more info on setting graphical parameters in `lattice` are given in Section \@ref(trellispar)). The code for the barchart is shown below, the resulting plot is shown in Figure \@ref(fig:lattbarorig1).

```r
trellis.par.set(superpose.polygon = 
                    list(col = c("darkslateblue", "indianred"))
                )
p = barchart(y~fct1, origin=0, groups=fct2,
             data=dat)
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattbarorig1-1.png" alt="Barchart with origin set to zero." width="326.4" />
<p class="caption">(\#fig:lattbarorig1)Barchart with origin set to zero.</p>
</div>

### Barcharts with error bars

There is no built-in support to add error bars to barcharts in `lattice`. This may be partly due to the fact that the `lattice` author thinks that barcharts with error bars are a bad data visualization choice (see discussion on R mailing list [here](https://stat.ethz.ch/pipermail/r-help/2006-May/105570.html)). 

Writing a custom panel function (see Section \@ref(trellispanel)) for adding error bars to bar charts is complicated by the fact `panel.barchart` performs several calculations for placing the bars and the custom function needs to take them into account to work correctly. The code associated with this guide contains a file (`barchart_errbar.R`) with a modified barchart panel function that can draw error bars in some conditions. We'll see how to use it shortly, but keep in mind that it may or may not work correctly, and that you should double check that it is doing the right thing! Also, it doesn't work for horizontal barcharts.

For the following examples we'll use the `npk` dataset, which we have introduced in Section \@ref(lattmpancond). Again, we first generate a summary dataset with mean yield as a function of N with the code below: 

```r
data(npk)
npk$P = factor(npk$P, levels=c("0", "1"), labels=c("P=0", "P=1"))
npk$K = factor(npk$K, levels=c("0", "1"), labels=c("K=0", "K=1"))
npk_summ_N = npk %>% group_by(N) %>% summarize(mean_yield=mean(yield),
                                               sd_yield=sd(yield))
npk_summ_N$ci.l = npk_summ_N$mean_yield - npk_summ_N$sd_yield
npk_summ_N$ci.u = npk_summ_N$mean_yield + npk_summ_N$sd_yield
```

The last two lines of code add a `ci.l` and a `ci.u` column, which represent, respectively, the lower and upper limits of the error bars; we'll use these two columns to add error bars to the plot. The code to draw the barchart with error bars is shown below:

```r
source("code/barchart_errbar.R")
p1 = barchart(mean_yield~N, data=npk_summ_N,
              ci.l=npk_summ_N$ci.l, ci.u=npk_summ_N$ci.u,
              plot.ci=TRUE,
              auto.key=list(space="top", points=F, rectangles=T),
              prepanel=prepanel.barchart_errbar,
              panel=function(x,y, ci.l, ci.u, subscripts,...){
                  ci.l = ci.l[subscripts]
                  ci.u = ci.u[subscripts]
                  panel.barchart_errbar(x, y, ci.l=ci.l, ci.u=ci.u,
                                  subscripts=subscripts,...)
              }
              )
print(p1)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattbrcherr1-1.png" alt="Barchart with error bar with no multipanel conditioning and no groups." width="326.4" />
<p class="caption">(\#fig:lattbrcherr1)Barchart with error bar with no multipanel conditioning and no groups.</p>
</div>

we first `source` the `barchart_errbar.R` file containing the custom panel and prepanel functions needed to draw the barchart with error bars, and then call the `barchart` function to draw the plot. Compared to a normal barchart we need to pass it a few additional arguments: the `ci.l` and `ci.u` vectors representing the lower and upper error bars limits; `plot.ci`, a boolean indicating whether to draw the error bars or not; `prepanel`, which is used to pass the `prepanel.barchart_errbar` function that sets the `y` axis limits so as to accommodate the error bars; a custom panel function, which in turn calls the `panel.barchart_errbar` function. Both `prepanel.barchart_errbar`, `panel.barchart_errbar` are functions defined in the `barchart_errbar.R` file. The resulting plot is shown in Figure \@ref(fig:lattbrcherr1)


Next we're going to look at the case of a single panel with a `groups` argument. For the next example we generate a new summary dataset as a function of N and P:

```r
npk_summ_NP = npk %>% group_by(N,P) %>% summarize(mean_yield=mean(yield),
                                                  sd_yield=sd(yield))
npk_summ_NP$ci.l = npk_summ_NP$mean_yield - npk_summ_NP$sd_yield
npk_summ_NP$ci.u = npk_summ_NP$mean_yield + npk_summ_NP$sd_yield
```

then we draw the barchart (shown in Figure \@ref(fig:lattbrcherr2)) with the code below:

```r
p2 = barchart(mean_yield~N, groups=P, data=npk_summ_NP,
              ci.l=npk_summ_NP$ci.l, ci.u=npk_summ_NP$ci.u,
              plot.ci=T, auto.key=list(space="top", points=F, rectangles=T),
              panel=function(x, y, ci.l, ci.u, subscripts,...){
                  ci.l = ci.l[subscripts]
                  ci.u = ci.u[subscripts]
                  panel.barchart_errbar(x,y, ci.l=ci.l, ci.u=ci.u,
                                  subscripts=subscripts,...)
              }, prepanel=prepanel.barchart_errbar
              )
print(p2)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattbrcherr2-1.png" alt="Barchart with error bar with groups but no multipanel conditioning." width="326.4" />
<p class="caption">(\#fig:lattbrcherr2)Barchart with error bar with groups but no multipanel conditioning.</p>
</div>

Finally, we look at the case of multiple panels with grouped bars. For this example we generate a summary dataset as a function of N, P, and K:

```r
npk_summ_NPK = npk %>% group_by(N,P,K) %>% summarize(mean_yield=mean(yield),
                                                     sd_yield=sd(yield))
npk_summ_NPK$ci.l = npk_summ_NPK$mean_yield - npk_summ_NPK$sd_yield
npk_summ_NPK$ci.u = npk_summ_NPK$mean_yield + npk_summ_NPK$sd_yield
```

then we draw the barchart (shown in Figure \@ref(fig:lattbrcherr3)) with the code below:

```r
p3 = barchart(mean_yield~N | P, groups=K, data=npk_summ_NPK,
             ci.l=npk_summ_NPK$ci.l, ci.u=npk_summ_NPK$ci.u,
             plot.ci=T, auto.key=list(space="top", points=F, rectangles=T),
             panel=function(x, y, ci.l, ci.u, subscripts,...){
                      ci.l = ci.l[subscripts]
                      ci.u = ci.u[subscripts]
                      panel.barchart_errbar(x,y, ci.l=ci.l, ci.u=ci.u,
                                      subscripts=subscripts,...)
             }, prepanel=prepanel.barchart_errbar)
print(p3)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattbrcherr3-1.png" alt="Barchart with error bar with groups and multipanel conditioning." width="489.6" />
<p class="caption">(\#fig:lattbrcherr3)Barchart with error bar with groups and multipanel conditioning.</p>
</div>

### Histograms and density plots {#latthist}

We'll plot an histogram of sepal length from the `iris` dataset (Figure \@ref(fig:latthist1)):

```r
data(iris)
p = histogram(~Sepal.Length, data=iris)
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/latthist1-1.png" alt="Histogram of sepal length in the `iris` dataset." width="326.4" />
<p class="caption">(\#fig:latthist1)Histogram of sepal length in the `iris` dataset.</p>
</div>

the model formula for the `histogram` function is somehow unusual because the "y" variable is computed by `lattice`, and we pass the variable for which we want to compute it after the tilde (`~`). As usual it is also possible to use multi-panel conditioning, as shown by the graph in Figure \@ref(fig:latthist2), which plots histograms for sepal length separately for each iris species, and was obtained with the code below:

```r
p = histogram(~Sepal.Length | Species, data=iris)
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/latthist2-1.png" alt="Histogram of sepal length in the `iris` dataset as a function of iris species." width="489.6" />
<p class="caption">(\#fig:latthist2)Histogram of sepal length in the `iris` dataset as a function of iris species.</p>
</div>

Density plots can be obtained with a similar formula (Figure \@ref(fig:lattdens1)):

```r
p = densityplot(~Sepal.Length, data=iris)
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattdens1-1.png" alt="Density plot of sepal length in the `iris` dataset." width="326.4" />
<p class="caption">(\#fig:lattdens1)Density plot of sepal length in the `iris` dataset.</p>
</div>

by default `densityplot` plots the individual observation as points. This can be turned off by setting `plot.points=FALSE`:

```r
p = densityplot(~Sepal.Length, plot.points=FALSE,
                data=iris)
```

### Interaction plots

For the next example we'll use the `ToothGrowth` built-in R dataset, which contains measurements of the length of odontoblasts (cells responsible for tooth growth) in 60 guinea pigs. Each guinea pig had received one of three doses of vitamin C (0.5, 1, and 2 mg/day) by one of two delivery methods, orange juice (OJ) or ascorbic acid (VC). 


```r
data(ToothGrowth)
head(ToothGrowth)
```

```
##    len supp dose
## 1  4.2   VC  0.5
## 2 11.5   VC  0.5
## 3  7.3   VC  0.5
## 4  5.8   VC  0.5
## 5  6.4   VC  0.5
## 6 10.0   VC  0.5
```

we'll first generate a summary dataset as a function of delivery method (`supp` variable), and dose:

```r
ToothGrowthSumm = ToothGrowth %>% group_by(supp, dose) %>%
                                  summarize(meanLen=mean(len))
ToothGrowthSumm$dose = factor(ToothGrowthSumm$dose)
```

now we can generate the plot (shown in Figure \@ref(fig:latttoothgrowth1)) with the following code:

```r
p = xyplot(meanLen~supp, groups=dose, data=ToothGrowthSumm,
           panel="panel.superpose", panel.groups="panel.linejoin",
           auto.key=list(points=F, lines=T, title="Dose", space="right"),
           xlab="Delivery method", ylab="Mean growth")
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/latttoothgrowth1-1.png" alt="Tooth growth by vitamin C dose and delivery method in guinea pigs." width="408" />
<p class="caption">(\#fig:latttoothgrowth1)Tooth growth by vitamin C dose and delivery method in guinea pigs.</p>
</div>

Obtaining an interaction plot with both lines and points requires writing a custon panel function:

```r
panelint = function(x, y, ...){
    panel.points(x, y, ...)
    panel.linejoin(x, y, ...)
}
```

the plot, shown in Figure \@ref(fig:latttoothgrowth2), can then be obtained by using this custom function:

```r
p = xyplot(meanLen~supp, groups=dose, data=ToothGrowthSumm,
           panel="panel.superpose", panel.groups="panelint",
           auto.key=list(points=T, lines=T, title="Dose", space="right"),
           xlab="Delivery method", ylab="Mean growth")
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/latttoothgrowth2-1.png" alt="Tooth growth by vitamin C dose and delivery method in guinea pigs." width="408" />
<p class="caption">(\#fig:latttoothgrowth2)Tooth growth by vitamin C dose and delivery method in guinea pigs.</p>
</div>

## Customizing lattice graphics {#customizingtrellis}

### Themes {#trellispar}

The function `show.settings` gives a graphical overview of the current theme. The settings for the current theme are stored in a list, which you can get with:

```r
curr_theme = trellis.par.get()
```

the list contains many items, which are themselved lists of more basic attributes, we'll take a peak at the names of the itmes:

```r
names(curr_theme)
```

we can modify this list, for example setting filled points instead of hollow circles as `plot.symbol` and `superpose.symbol`

```r
curr_theme$plot.symbol$pch = 16
curr_theme$superpose.symbol$pch = rep(16, 7)
```

once this is done we have to call the `trellis.par.set` function to use the modified list as the current theme:

```r
trellis.par.set(curr_theme)
```


### Textual elements

#### Strip labels

The labels of the panels strips are the names of the levels of the conditioning factor variable. To change their labels you can pass the `factor.levels` argument to the `strip.custom` function:


```r
x = rnorm(20)
y = rnorm(20)
treat = factor(rep(c("A", "B"), each=10))
xyplot(y~x|treat, strip=strip.custom(factor.levels=c("Group A", "Group B")))
```

<div class="figure">
<img src="09_lattice_files/figure-html/strip1-1.png" alt="Custom strip labels." width="326.4" />
<p class="caption">(\#fig:strip1)Custom strip labels.</p>
</div>

Alternatively, you change the names for the factor levels:

```r
levels(treat) = c("Group A", "Group B")
```

if you have more than one conditioning factor you can customize the strip labels for each by passing a custom strip function. `which.given` specifies the conditioning
variable that the strip corresponds to:


```r
cnd = factor(rep(c("I", "II"), 10))

customstrip = function(which.given, ..., factor.levels){
    levs = if (which.given==1){
        c("Group A", "Group B")
    } else if  (which.given==2){
        c("Cnd. I", "Cnd. II")
    }
    strip.default(which.given, ..., factor.levels = levs)
}
xyplot(y~x | treat + cnd, strip=customstrip, as.table=TRUE)
```

<div class="figure">
<img src="09_lattice_files/figure-html/strip2-1.png" alt="Custom strip labels with two conditioning factors." width="489.6" />
<p class="caption">(\#fig:strip2)Custom strip labels with two conditioning factors.</p>
</div>

### Strips

#### Changing the strip background color


```r
data(iris)
ir2 = iris[iris$Species!="virginica",]
xyplot(Sepal.Length~Sepal.Width | Species, data=ir2, as.table=T,
       par.settings=list(strip.background=list(col="gray90")))
```

<div class="figure">
<img src="09_lattice_files/figure-html/stripcolor1-1.png" alt="Changing strip background color." width="456.96" />
<p class="caption">(\#fig:stripcolor1)Changing strip background color.</p>
</div>

or to set a color for several plots:

```r
trellis.par.set(strip.background=list(col="gray90"))
```

#### Remove "box" and strip borders

The code below plots a figure without a bounding frame and without a strip, the result is shown in Figure \@ref(fig:rmstripbox).

```r
axis.L =  function(side, ..., line.col){
    if (side %in% c("bottom", "left")) {
        col = trellis.par.get("axis.text")$col
        axis.default(side, ..., line.col = col)
        if (side == "bottom")
            grid::grid.lines(y = 0)
        if (side == "left")
            grid::grid.lines(x = 0)
    }
}
xyplot(Sepal.Length~Sepal.Width | Species, data=ir2, axis=axis.L,
       scales=list(x=list(alternating=c(1,1)),
                   y=list(limits=c(min(ir2$Sepal.Length),
                                   max(ir2$Sepal.Length)),
                          relation="free")),
       par.settings=list(strip.border=list(col="transparent"),
                         strip.background=list(col="transparent"),
                         axis.line=list(col="transparent")))
```

<div class="figure">
<img src="09_lattice_files/figure-html/rmstripbox-1.png" alt="Remove strip and box around the panels." width="522.24" />
<p class="caption">(\#fig:rmstripbox)Remove strip and box around the panels.</p>
</div>

The following code is a variant without tick labels on the inner y panel, the result is shown in Figure \@ref(fig:rmstripbox2).

```r
axis.L2 =  function(side, ..., line.col){
    if (side %in% c("bottom", "left")) {
        col = trellis.par.get("axis.text")$col
        if (side == "bottom"){
            grid::grid.lines(y = 0)
            axis.default(side, ..., line.col = col, labels="yes")
        }
        if (side == "left"){
            grid::grid.lines(x = 0)
            drawTickLabels = ifelse(current.column() == 1, "yes", "no")
            axis.default(side, ..., line.col = col, labels=drawTickLabels)
        }
    }
}


p = xyplot(Sepal.Length~Sepal.Width | Species, data=ir2, axis=axis.L2, as.table=T,
           scales=list(x=list(alternating=FALSE),
                       y=list(limits=c(min(ir2$Sepal.Length),
                                       max(ir2$Sepal.Length)),
                              relation="free",
                              alternating=4)),
           par.settings=list(strip.border=list(col="transparent"),
                             strip.background=list(col="transparent"),
                             axis.line=list(col="transparent")))
print(p)
```

<div class="figure">
<img src="09_lattice_files/figure-html/rmstripbox2-1.png" alt="Remove strip and box around the panels. Do not draw tick labels on the inner y panel." width="522.24" />
<p class="caption">(\#fig:rmstripbox2)Remove strip and box around the panels. Do not draw tick labels on the inner y panel.</p>
</div>

### Log axis with pretty tickmarks

The `latticeExtra` package provides a function to get a log axis with pretty tickmarks in lattice, as shown in the code below. The result is shown in Figure \@ref(fig:lattextrlogaxpretty).

```r
library(latticeExtra)
x = as.factor(c("cnd1", "cnd2"))
y = c(0.4, 80)
dat = data.frame(x=x, y=y)
xyplot(y~x, data=dat,
       scales=list(y=list(log=10)),
       yscale.components = yscale.components.log10ticks)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattextrlogaxpretty-1.png" alt="Log axis with pretty tickmarks using latticeExtra." width="326.4" />
<p class="caption">(\#fig:lattextrlogaxpretty)Log axis with pretty tickmarks using latticeExtra.</p>
</div>

#### Doing it manually

I had written this before discovering the `yscale.components.log10ticks` function in `latticeExtra`. I'm leaving it here in case it's useful for other similar axis customizations. 

The procedure to get a log axis with pretty tickmarks manually in lattice is a bit involved. We'll only cover the log base 10 case here. The first step is to define a function that returns the tick locations:


```r
log10Ticks = function(lim, onlyMajor=FALSE){
    minPow = floor(log10(lim[1]))
    maxPow = ceiling(log10(lim[2]))
    powSeq = seq(minPow, maxPow)
    majTicks = 10^powSeq
    minTicks = numeric()
    for (i in 1:length(majTicks)){
        bb = (1:10)/10;
        minTicks = c(minTicks, (bb*10^powSeq[i]))
    }
    if (onlyMajor==TRUE){
        axSeq = majTicks
    } else {
        axSeq = minTicks
    }
    axSeq = axSeq[lim[1] <= axSeq & axSeq <= lim[2]]
    return(axSeq)
}
```

by default the function returns both the major (e.g. 1, 10, 100, etc...) and the minor (e.g. 2, 3, 4,...20, 30, 40, etc...) tick locations, but returns only the major tick locations if `onlyMajor=TRUE`. This function will be used by the `yscale.components.log10` function below. This function will be passed as the `yscale.components` argument in the `xyplot` call that generates the graph. The `yscale.components.log10` function will return a list specifying all parameters of the y axis. To simplify this process the function calls the `yscale.components.default` function to retrieve the default parameters, and then simply modifies some of these parameters to draw the pretty log axis:

```r
#the function is automatically passed the limits of the panel as an argument
yscale.components.log10 = function(lim, ...){
    #retrieve default parameters
    ans = yscale.components.default(lim = lim, ...)
    #compute major and minor tick locations
    tick.at = log10Ticks(10^lim, onlyMajor=FALSE)
    #compute major tick locations only
    tick.at.major = log10Ticks(10^lim, onlyMajor=TRUE)
    #which are the major ticks?
    major = tick.at %in% tick.at.major
    #where the ticks should be position
    ans$left$ticks$at = log10(tick.at)
    #set tick length, depending on whether minor or major
    ans$left$ticks$tck = ifelse(major, 1.5, 0.75)
    #labels location
    ans$left$labels$at = log10(tick.at)
    #set tick labels
    ans$left$labels$labels = as.character(tick.at)
    #set minor tick labels as empty
    ans$left$labels$labels[!major] = "" 
    ans$left$labels$check.overlap = FALSE
    return(ans)
}
```

once the `yscale.components.log10` function is ready, we can use it in the call to `xyplot`, note that we also need to set the y scale to log10 in the `scales` argument, as shown in the code below. The result is shown in Figure \@ref(fig:latticelogaxispretty).

```r
x = as.factor(c("cnd1", "cnd2"))
y = c(0.4, 80)
dat = data.frame(x=x, y=y)
xyplot(y~x, data=dat,
       scales=list(y=list(log=10)),
       yscale.components = yscale.components.log10)
```

<div class="figure">
<img src="09_lattice_files/figure-html/latticelogaxispretty-1.png" alt="Log axis with pretty tickmarks." width="326.4" />
<p class="caption">(\#fig:latticelogaxispretty)Log axis with pretty tickmarks.</p>
</div>

### Adding regression lines/curves to a `xyplot`

For this example we'll use the `mpg` dataset from the `ggplot2` package:

```r
mpg = ggplot2::mpg
```

A loess can be easily added by passing a `type` argument to `xyplot`:

```r
xyplot(hwy~displ|drv, data=mpg, type=c("p", "smooth"), as.table=T)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattloess1-1.png" alt="`xyplot` with a loess curve." width="489.6" />
<p class="caption">(\#fig:lattloess1)`xyplot` with a loess curve.</p>
</div>

in this case we passed a vector with elements `p`, for the points, and `smooth` for the loess curve. The resulting plot is shown in Figure \@ref(fig:lattloess1).

A linear fit line can be added by passing `r` as an element in the `type` argument vector, as shown in the next example using the `iris` dataset (plot shown in Figure \@ref(fig:lattlinfit1)):

```r
data(iris)
xyplot(Sepal.Length~Petal.Length|Species, data=iris,
       type=c("p", "r"), as.table=T)
```

<div class="figure">
<img src="09_lattice_files/figure-html/lattlinfit1-1.png" alt="`xyplot` with a linear fit line." width="489.6" />
<p class="caption">(\#fig:lattlinfit1)`xyplot` with a linear fit line.</p>
</div>

The `panel.smoother` function in the [`latticeExtra`](https://cran.r-project.org/web/packages/latticeExtra/index.html) package can additionally plot confidence bands.

## Writing panel functions {#trellispanel}

Panel functions allow to customize and extend `lattice` plots, and can be used to generate new types of plots. The underlying system is sophisticated, but also complex, and overall writing panel function is not easy. Your mileage may vary, some simple customizations and extensions may be relatively easy, but it may be tricky to customize complex plots that include both conditioning and grouping variables. I am still far from mastering the system for writing panel functions, and these notes simply reflect what I've learnt so far.

### Combining panel functions

Rather than writing a new panel function from scratch, often you just want to add some elements to a plot, for example a regression line, or error bars. The easiest way to do this is writing a panel function that combines two standard panel functions. There are a number of predefined panel functions (see `?panel.functions`) that can be used to add lines, grids etc..., to a scatterplot, barchart or other high-level lattice plotting function.

We'll start with a very simple example: adding a horizontal line at a fixed height in each panel. The dataset below contains a variable `y` with both positive and negative values:

```r
dat = data.frame(y=c(-0.5, 0.7, 1, -0.8, -0.75, 0.25, 0.32, -0.1),
                 fct1=rep(rep(c("A", "B"), each=2), 2),
                 fct2 = rep(c("L1", "L2"), 4),
                 fct3 = rep(c("F1", "F2"), each=4)
                 )
```

we can visualize it with a dotplot (Figure \@ref(fig:dotplotnohline)):

```r
trellis.par.set(superpose.symbol = 
                list(col = c("darkslateblue", 
                "indianred"), pch=19))
p1 = dotplot(y ~ fct1 | fct2, groups=fct3, 
            data=dat, auto.key=TRUE, 
            aspect=1, as.table=TRUE)
p1
```

<div class="figure">
<img src="09_lattice_files/figure-html/dotplotnohline-1.png" alt="Dotplot without a horizontal line at `y=0`." width="489.6" />
<p class="caption">(\#fig:dotplotnohline)Dotplot without a horizontal line at `y=0`.</p>
</div>

however, because the data represent positive or negative displacements from zero, it would be useful to add a horizontal line passing at zero. In order to do this, we'll write a panel function that combines the `panel.dotplot` function with the `panel.abline` function. The latter function will be used to add the horizontal line:

```r
panel.hRefDotplot = function(x, y, ref=NULL, ...){
    panel.dotplot(x, y, ...)
    panel.abline(h=ref, ...)
}
```

our new `panel.hRefDotplot` panel function accepts three arguments, `x` and `y`, which are "standard" arguments passed by the higher-level plotting functions (like `dotplot`, or `xyplot`) to panel functions to specify the data to draw. The third argument represents the position at which to draw the horizontal line of reference for the data; we want it to be zero in this case, but passing the argument as a variable rather than hard-coding the value into the panel function will allow us to recycle this panel function in case we want the horizontal reference line drawn at some other points in the future. Besides these arguments, our panel function accepts also an undefined number of other arguments, which are designated by the `...` notation. These are usually graphics parameters that can be specified in the high level plotting function. Our `panel.hRefDotplot` function is quite simple: we first call  `panel.dotplot` to draw the standard dotplot, and then call `panel.abline` giving it the value of `ref` to draw the horizontal line in each panel. The actual plot (Figure \@ref(fig:dotplothline)) is done by calling the high level `dotplot` function and specifying `panel.hRefDotplot` as the panel function to use:

```r
p2 = dotplot(y ~ fct1 | fct2, groups=fct3, 
            data=dat, auto.key=TRUE, 
            aspect=1, as.table=TRUE,
            panel=panel.hRefDotplot, ref=0)
p2
```

<div class="figure">
<img src="09_lattice_files/figure-html/dotplothline-1.png" alt="Dotplot with a horizontal reference line." width="489.6" />
<p class="caption">(\#fig:dotplothline)Dotplot with a horizontal reference line.</p>
</div>

Note the last line of the call: first, we're telling `dotplot` to use our `hRefDotplot` function to do the plotting by specifying the `panel` argument, second we're specifying another argument, `ref` in the call, this is not a standard argument, but it will be automatically passed to our panel function to decide at which height to draw the horizontal line. 

### Adding error bars to an `xyplot` (single panel, no groups)

We now move on to a slightly more complex example. Adding error bars to an `xyplot` consisting of a single panel, and with no `groups` variable. For this example we'll use the `iris` dataset, and in particular we'll plot the mean sepal width  $\pm 1$ sd as a function of species. The following lines of code prepare the dataset to plot:

```r
data(iris)
iris_summ = iris %>% group_by(Species) %>%
    summarize(mean_sep_w = mean(Sepal.Width), sd_sep_w=sd(Sepal.Width))
iris_summ$ylow = iris_summ$mean_sep_w - iris_summ$sd_sep_w
iris_summ$yup = iris_summ$mean_sep_w + iris_summ$sd_sep_w
```

The last two lines of code add a `ylow` and a `yup` variable, which represent the lower and upper y coordinates of the error bars. Because error bars take space, we have to increase the limits of the y axis. We could do this "manually" by adding a `ylim` argument to `xyplot` a more elegant solution is to pass a prepanel function that computes the limits taking into account the space taken by the error bars:

```r
prepanel.err = function(x, y, ylow, yup, ...){
     list(ylim = range(y, yup, ylow, finite = TRUE)) 
}
```

Next, we write the panel function that calls `xyplot`, and adds the error bars by calling `panel.arrows`:

```r
panel.err0 = function(x, y, ylow, yup,
             col.line=trellis.par.get()[["superpose.line"]][["col"]][1],
                      ...){
             panel.xyplot(x, y, ...)
             panel.arrows(x0 =x, y0 =ylow, x1=x, y1=yup,
                          angle=90, end="both",
                          col=col.line,
                          length=0.1, ...)
}
```

we can now draw the plot, which is shown in Figure \@ref(fig:latterrngnp), with the following code:

```r
p = xyplot(mean_sep_w~Species, data=iris_summ,
           ylow=iris_summ$ylow, yup=iris_summ$yup,
           panel=panel.err0, prepanel=prepanel.err)
p
```

<div class="figure">
<img src="09_lattice_files/figure-html/latterrngnp-1.png" alt="`xyplot` with error bars for a single panel with no groupings." width="326.4" />
<p class="caption">(\#fig:latterrngnp)`xyplot` with error bars for a single panel with no groupings.</p>
</div>

the only differences from a plain `xyplot` call is that we're passing our panel and prepanel functions as additional arguments.

### Adding error bars to an `xyplot` with multiple panels (no groups)

We move on to the case of adding error bars to a `xyplot` with multiple panels, but no `groups` variable. We'll use the `npk` dataset, and in particular we'll plot the mean yield $\pm 1$ sd as a function of N and P administration. The following lines of code prepare the dataset to plot:

```r
data(npk)
npk_summ_NP = npk %>% group_by(N,P) %>% summarize(mean_yield=mean(yield),
                                                  sd_yield=sd(yield))
npk_summ_NP$ylow = npk_summ_NP$mean_yield - npk_summ_NP$sd_yield
npk_summ_NP$yup = npk_summ_NP$mean_yield + npk_summ_NP$sd_yield
```

The panel function that we're going to use is given by the following lines of code:

```r
panel.err1 = function(x, y, subscripts, ylow, yup,
             col.line=trellis.par.get()[["superpose.line"]][["col"]][1],
              ...){
    panel.xyplot(x, y, ...)
    panel.arrows(x0 =x, y0 =ylow[subscripts], x1=x, y1=yup[subscripts],
                 angle=90, end="both",
                 col=col.line,
                 length=0.1, ...)
}
```

Note how besides the `ylow` and `yup` variables the function accepts an additional `subscripts` variable. This variable holds the indexes of the rows of the dataframe that are used to plot a given panel. The `x` and `y` variables have already been "filtered" to contain only the values needed for a given panel; the `ylow` and `yup` variables need to be "filtered" using the `subscripts`. The call to `xyplot` is given below, and the resulting plot is shown in Figure \@ref(fig:latterrp):

```r
p = xyplot(mean_yield~N | P, data=npk_summ_NP,
           ylow=npk_summ_NP$ylow, yup=npk_summ_NP$yup,
           panel=panel.err1, prepanel=prepanel.err)
p
```

<div class="figure">
<img src="09_lattice_files/figure-html/latterrp-1.png" alt="`xyplot` with error bars for a multiple panels with no groups." width="408" />
<p class="caption">(\#fig:latterrp)`xyplot` with error bars for a multiple panels with no groups.</p>
</div>

we're reusing the prepanel function from the last example (`prepanel.err`) because for this plot the computations for the y limits are the same.

### Adding error bars to an `xyplot` with groups (single panel)

We'll now look at the case of adding error bars to an `xyplot` in a single panel but with a `groups` variable. We'll use the same dataset, and the same prepanel function that we employed in the last example. The panel function is given below:

```r
panel.errg = function(x, y, subscripts, ylow, yup, col.line="black", ...){
    panel.xyplot(x, y, ...)
    panel.arrows(x0=x, y0=ylow[subscripts], x1=x, y1=yup[subscripts],
                 end="both", angle=90, col=col.line, length=0.1)
}
```

in this case `subscripts` reflects the indexes of the rows of the dataframe containing the values for a given *group*. The call to `xyplot` is shown below, and the resulting plot is shown in Figure \@ref(fig:latterrg):

```r
p = xyplot(mean_yield~N, groups=P, data=npk_summ_NP,
           ylow=npk_summ_NP$ylow, yup=npk_summ_NP$yup,
           panel=panel.superpose, panel.groups=panel.errg,
           prepanel=prepanel.err)
p
```

<div class="figure">
<img src="09_lattice_files/figure-html/latterrg-1.png" alt="`xyplot` with error bars with groups." width="326.4" />
<p class="caption">(\#fig:latterrg)`xyplot` with error bars with groups.</p>
</div>

when groups are involved, `lattice` calls `panel.superpose` to handle plotting the data of different groups with distinguishing attributes (e.g. colors, line type, etc...). `panel.superpose` then calls a `panel.groups` function to handle the actual plotting for each group. In this case we're passing our custom `panel.groups` function to add error bars.

We've achieved our aim of adding error bars, but there's an issue: the bars are overlapping and the plot looks ugly. To fix this issue we write another panel function which will slightly offset the position of the points and the bars on the x axis depending on which group is being plotted. To be able to do this, we let the function accept the `group.number` argument, a number denoting which group is being plotted:

```r
panel.errg2 = function(x, y, subscripts, group.number,
                       ylow, yup, col.line="black", ...){
    offs = ifelse(group.number==1, -0.1, 0.1)
    x = x + offs
    panel.xyplot(x, y, ...)
    panel.arrows(x0=x, y0=ylow[subscripts], x1=x, y1=yup[subscripts],
                 end="both", angle=90, col=col.line, length=0.1)
}
```


The call to `xyplot` is shown in the code below, and the resulting plot, which now looks better, is shown in Figure \@ref(fig:latterrg2):

```r
p = xyplot(mean_yield~N, groups=P, data=npk_summ_NP,
           ylow=npk_summ_NP$ylow, yup=npk_summ_NP$yup,
           panel=panel.superpose, panel.groups=panel.errg2,
           prepanel=prepanel.err)
p
```

<div class="figure">
<img src="09_lattice_files/figure-html/latterrg2-1.png" alt="`xyplot` with error bars with groups." width="326.4" />
<p class="caption">(\#fig:latterrg2)`xyplot` with error bars with groups.</p>
</div>

### Adding error bars to an `xyplot` with groups and multiple panels

We'll now move on to the case of adding error bars to an `xyplot` with multiple panels and a grouping variable. For this example we'll again use the `npk` dataset, but this time we'll plot the mean yield $\pm 1$ sd as a function of N, P, and K administration. The following lines of code prepare the dataset to plot:

```r
npk_summ_NPK = npk %>% group_by(N,P,K) %>% summarize(mean_yield=mean(yield),
                                                     sd_yield=sd(yield))
npk_summ_NPK$ylow = npk_summ_NPK$mean_yield - npk_summ_NPK$sd_yield
npk_summ_NPK$yup = npk_summ_NPK$mean_yield + npk_summ_NPK$sd_yield
```

We won't need to write a new prepanel or a new panel function, because those from the last example will be suitable also for the current plot. The call to `xyplot` is shown in the code below, and the plot is shown in Figure \@ref(fig:latterrgp):

```r
p = xyplot(mean_yield~N | K, groups=P, data=npk_summ_NPK,
           ylow=npk_summ_NPK$ylow, yup=npk_summ_NPK$yup,
           panel=panel.superpose, panel.groups=panel.errg2,
           prepanel=prepanel.err)
p
```

<div class="figure">
<img src="09_lattice_files/figure-html/latterrgp-1.png" alt="`xyplot` with error bars with groups and multiple panels" width="489.6" />
<p class="caption">(\#fig:latterrgp)`xyplot` with error bars with groups and multiple panels</p>
</div>

## Further resources {#lattres}

### Books, articles, and tutorials {#lattmat}

[Creating publication quality graphs in R](https://moc.online.uni-marburg.de/gitbooks/publicationQualityGraphics/_book/index.html) by Tim Appelhans, provides useful notes on both `lattice` and `ggplot2`, including ways to manipulate and arrange plots using `grid`. The git repository of the tutorial is available [here](https://github.com/tim-salabim/publication-quality-graphics).

### Related packages {#lattrelpkg}

[`latticeExtra`](https://cran.r-project.org/web/packages/latticeExtra/index.html) provides several additional high-level plotting functions and many utility functions.
[`wzRfun`](https://github.com/walmes/wzRfun) contains miscellaneous utilities, including panel functions to add error bars and error bands to `lattice` plots.
