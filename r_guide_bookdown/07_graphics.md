# Graphics



There are four main graphics libraries that can be used in R. The first is the base graphics system that comes builtin with every R installation and will be described in this chapter. The other three main graphics libraries (`ggplot2`, `lattice`, and `plotly`), can be installed as additional packages. Each of these libraries provide a complete, independent system to generate graphics in R. Choosing one library over the other is mostly a matter of personal preference because pretty much any graphic that can be built with one library can also be built with the others. Some graphics are easier to build with one library than another and vice-versa. In recent years `ggplot2` has gained lots of popularity and `lattice` is less popular than it was some years ago. Despite the increasing popularity of `ggplot2` the base R graphics that are described in this chapter are still widely used. `plotly` is a recent entry and is somehow a special case because it is primarily designed to generate interactive graphics that can be displayed on html pages, while the other graphics libraries have very limited interactive functionality and are primarily designed to generate static high-quality graphics. `plotly` is special also because through the `ggplotly` function it can convert a `ggplot2` graph into an interactive `plotly` graphic.

Because the base R graphics library is still widely used and (in my opinion) is somewhat simpler to use for beginners, I would recommend learning it first. The `ggplot2` library is described in Chapter \@ref(ggplot2), the `lattice` graphics library is described in Chapter \@ref(lattice), and the `plotly` library is described in Chapter \@ref(plotly).

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
<img src="07_graphics_files/figure-html/scatterplot-1.png" alt="A scatterplot." width="326.4" />
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
<img src="07_graphics_files/figure-html/timewlines-1.png" alt="Values connected by lines." width="326.4" />
<p class="caption">(\#fig:timewlines)Values connected by lines.</p>
</div>

Some other possible values for the option `type` are `p` for points (the default), `b` for *both* points and lines, and `o` for overplotted points and lines (very similar to `b`).

## Drawing Functions

The `plot` function can be used for drawing mathematical functions, for example:

```r
vec = seq(from=0, to=4*pi, length=120)
plot(vec,sin(vec), type="l")
```

<img src="07_graphics_files/figure-html/unnamed-chunk-2-1.png" width="326.4" />

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
<img src="07_graphics_files/figure-html/sincos-1.png" alt="Sine and cosine functions with `matplot`." width="326.4" />
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
<img src="07_graphics_files/figure-html/barplotsimple-1.png" alt="Simple barplot." width="326.4" />
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
<img src="07_graphics_files/figure-html/barplotbars-1.png" alt="Barplot with error bars." width="326.4" />
<p class="caption">(\#fig:barplotbars)Barplot with error bars.</p>
</div>

## Boxplots

Boxplots can be used to visualise the central tendency and the dispersion of the data for a given sample, and to directly compare these same characteristics for different samples. Let's look at one of them, the data are organised in a dataframe in the file `boxplot1.txt`. They are the scores for two different groups (group a and group b) in a test. With the boxplot we want to see how the scores are distributed in the two groups. The dataframe contains a column `score`, with the score for each subject and another column `group` that defines the group a given observation comes from. So we first read in the dataframe, and then ask for the boxplots with the distribution of scores, as a function of the group they belong to.


```r
dats = read.table("datasets/boxplot1.txt", header=TRUE)
boxplot(dats$score~dats$group, names=c("group a", "group b"))
```

<div class="figure">
<img src="07_graphics_files/figure-html/boxplot1-1.png" alt="Boxplots comparing the distribution for two groups." width="326.4" />
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
<img src="07_graphics_files/figure-html/hist-1.png" alt="Frequency distribution of a random sample." width="326.4" />
<p class="caption">(\#fig:hist)Frequency distribution of a random sample.</p>
</div>

## Stripcharts

If the groups  contain a small number of observations, it might be better to use  a stripchart to visualise their distributions. In a stripchart each point represents a single observation. By default they are drawn on a line, so if two observations have the same score, they overlap. To avoid this problem you can give a certain amount of jitter to the plot, so that observations with the same score are scattered a little and can be easily distinguished. Here's the code for producing a stripchart, the data are in the file `stripchart1.txt` and they are arranged in the same way as the data in `boxplot1.txt`, just the sample sizes are smaller, with 6 observations per group.


```r
stripchart(dats$score~dats$group, method="jitter",
           jitter=0.1, pch=1, vertical=TRUE)
```

<div class="figure">
<img src="07_graphics_files/figure-html/stripchart1-1.png" alt="Stripchart example." width="326.4" />
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
<img src="07_graphics_files/figure-html/int-1.png" alt="Interaction plot with different line types." width="326.4" />
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
<img src="07_graphics_files/figure-html/intb-1.png" alt="Interaction plot with different line types and different symbols." width="326.4" />
<p class="caption">(\#fig:intb)Interaction plot with different line types and different symbols.</p>
</div>

## Setting Graphics Parameters {#par}

Graphics parameters allow you to tweak many elements of a plot, such as the font for the labels, the symbols or line types to use and so on, see `?par` to get a full list and description of these parameters. Graphics parameters can be set and accessed with the function `par`, called without arguments, as `par()`, it will give you a full list of the current defaults, if you want to query only one or a few parameters use:

```r
par("lwd")          ## see current line width
```

```
## [1] 1
```

```r
par(c("lwd","pch")) ## see lwd and plotting symbols
```

```
## $lwd
## [1] 1
## 
## $pch
## [1] 1
```

to change the value of a parameter you can use:


```r
par(lwd=1.4)       ## change line width
par(pch="*",       ## change plotting symbol
    bg ="gray80")  ## use a light gray background
```

Moreover most plotting functions like `plot`, `barplot` and so on, allow you to set some of the parameters for the current plot, as an argument to the function itself, for example in `plot` you can choose the type of plot (points vs lines) and the plotting symbol as options with `type` and `pch`:


```r
d = rnorm(15,4,2)
e = rnorm(15,9,3)
plot(d~e, type="p", pch=3)
```

The following sections will give a more in depth explanation of some graphics parameters. Before that, however, we'll have a closer look at how to use `par`.

### Saving and Restoring Graphics Parameters

Often you'll want to change the graphics parameters only for a few plots, and then reset them back to the defaults. The function `par` when used to change the value of some graphics parameters returns a list with the *old* values of the graphics parameters that have changed:


```r
par("lwd", "col") #these are the default parameters
```

```
## $lwd
## [1] 1
## 
## $col
## [1] "black"
```

```r
oldpar = par(lwd=2, col="red") #while changing the parameters
                                #we store the old values in a list
oldpar
```

```
## $lwd
## [1] 1
## 
## $col
## [1] "black"
```

```r
s = seq(0, 10, .1) 
plot(s, sin(s))           #we plot something
```

```r
par(oldpar)              # and then we restore the old parameters
```

note that calling `par`, opens a graphics device if one is not already open, the changes you do using `par` apply only to this graphics device, any other new graphic device that you open will have the default graphics parameters. 

### Line type with the `lty` parameter {#lty}

There are six line types that you can call in R just with names or numbers (actually there are seven, the first one is "blank" or 0 which just draws nothing). These are listed in Table \@ref(tab:lty) and shown in Figure \@ref(fig:lty). There is a different, more complicated way for setting many more line types, please, consult the manual for further information on that.

No.  Name
---  ---------
1    solid
2    dashed
3    dotted
4    dotdash
5    longdash
6    twodash

Table: (\#tab:lty) The six default line types in R.

<div class="figure">
<img src="07_graphics_files/figure-html/lty-1.png" alt="The six default line types in R" width="326.4" />
<p class="caption">(\#fig:lty)The six default line types in R</p>
</div>

### Symbols with the `pch` parameter {#secpch}

`pch` is a graphical parameter for changing the way points are plotted in certain graphical functions. This parameter can be set in two ways, the first one is to give a symbol to be plotted as a character, for example 

```r
pch="+"
pch="T"
pch="*"
pch="3" 
```

in this case you enclose the character you want to use between quotes, you can't use more than a single character. The other way to set the `pch` is to use a number between 0 and 25, which will select one of 26 special symbols available for plotting (see Figure \@ref(fig:pch)), like circles, triangles, and so on:


```r
pch=1
pch=3
pch=5
```

<div class="figure">
<img src="07_graphics_files/figure-html/pch-1.png" alt="Plotting symbols from 0 to 25" width="326.4" />
<p class="caption">(\#fig:pch)Plotting symbols from 0 to 25</p>
</div>

### Fonts {#parfonts}

The `par` interface can be used to set a number of font parameters. One thing to keep in mind is that certain font properties can also be set at the device (X11, pdf, png, etc...) level. Properties set with `par` after opening the device will override properties set at the device level. Additionally, each device deals with fonts differently, and setting a given font family with `par` for a given deice (e.g. X11) will not necessarily work for other devices (e.g. pdf). The font parameters that can be set through the `par` interface are listed in Table \@ref(tab:parfonts)

Parameter   Function
---------   ------------------------------------------
`family`    font family (e.g. "sans", "serif", "mono")
`font`       font type for text (1=plain, 2=bold, 3=italic, 4=bold italic)
`font.axis` font type for axis annotation (1=plain, 2=bold, 3=italic, 4=bolditalic)
`font.lab`  font type for x and y labels (1=plain, 2=bold, 3=italic, 4=bolditalic)
`font.main` font type for main titles (1=plain, 2=bold, 3=italic, 4=bolditalic)
`font.sub`  font type for sub titles (1=plain, 2=bold, 3=italic, 4=bolditalic)
`ps`          point size of text

Table: (\#tab:parfonts) Parameters for fonts.

The various parameters starting with "font" (`font`, `font.axis`, `font.lab`, etc...) do not change the *family*, they change the *style* (e.g. plain, bold, italic). These parameters are set via a number: a value of 1 corresponds to a normal (or plain) style, 2 to bold, 3 to italics, 4 to bold italics, and 5 will map the font to a symbol:

<div class="figure">
<img src="07_graphics_files/figure-html/unnamed-chunk-16-1.png" alt="Changing font style" width="326.4" />
<p class="caption">(\#fig:unnamed-chunk-16)Changing font style</p>
</div>

Note that setting `font` only changes the font style of plotted text. If you want to change the font style of other textual elements such as the axis labels or the plot title you have to set the corresponding graphics parameters:

<div class="figure">
<img src="07_graphics_files/figure-html/unnamed-chunk-17-1.png" alt="Changing font style for other textual elements" width="326.4" />
<p class="caption">(\#fig:unnamed-chunk-17)Changing font style for other textual elements</p>
</div>

The `ps` parameters sets the *base* point size of the font; the final pointsize is scaled by the `cex` parameter (i.e. text size = ps*cex), so if `cex` is different than 1, the final pointsize will not be equal to `ps`. Note that `ps` changes only the size of the text font:



while setting the `pointsize` when opening certain graphics devices, such as the `pdf` device, changes the size of both the font and the plotting symbols:


The documentation for the graphics parameter `ps` also says that "unlike the pointsize argument of most devices, this does not change the relationship between `mar` and `mai` (nor `oma` and `omi`).". I'm not sure what this means, but just be aware setting `ps` with a call to `par` or setting `pointsize` when opening a graphics device are not equivalent. Also, keep in mind that if you set the font size when you open a graphics device, the setting may be overridden by subsequent calls to `par`:



`cex` also overrides the graphics device settings:



The `par` setting `family` can be used to choose a `serif`, `sans`, or `mono` font:

<div class="figure">
<img src="07_graphics_files/figure-html/unnamed-chunk-22-1.png" alt="Changing font family" width="326.4" />
<p class="caption">(\#fig:unnamed-chunk-22)Changing font family</p>
</div>

Note that `serif`, `sans`, or `mono` are generic font families. The actual font family (e.g. Helvetica, Arial, Times New Roman, etc...) that gets used depends on a mapping, which is different between graphics devices, between these generic names and an actual system font. It is possible to directly specify a system font when calling `par`:

```r
plot.new(); plot.window(xlim=c(1,10), ylim=c(1, 10))
par(family="Palatino")
plot(1:10)
```

<img src="07_graphics_files/figure-html/unnamed-chunk-23-1.png" width="326.4" />

however, this may or may not work depending on 1) whether the font is actually installed on your system 2) the graphics device (not all graphics devices have access to all installed system fonts). More information on how to use system fonts is available in Chapter \@ref(fonts)

	
### Colors {#parcolors}

Parameter   Function
---------   --------------------------
`col`       plotting colour
`col.axis`  colour for axis annotation 
`col.lab`   colour for x and y labels 
`col.main`  colour for main title 
`col.sub`   colour for sub-titles 
`bg`        background colour
`fg`        foreground colour

Table: (\#tab:parcolours) Parameters for colours.
	
## Adding Elements to a Plot

### Adding a Legend {#legend}

Some graphics functions by default add a legend to the graph (e.g. `interaction.plot`), or allow to add (or remove) a legend by setting an option inside the function (e.g. `barplot`). However the default settings for the legend, such as positioning, text or symbols, might not be suitable to your graph, in which case you need to turn off the default legend (if there is one), and add a customised legend with the `legend` function. Below there is an example of an interaction plot with the legend added through the `legend` function, the resulting graph is in Figure \@ref(fig:legend1)


```r
dats = read.table('datasets/direrr_all.txt', header=T)
levels(dats$congr)[1] = 'c - congruent'
levels(dats$congr)[2] = 'a - inc. goal-directed'
levels(dats$congr)[3] = 'b - inc. no-goal-directed'
interaction.plot(dats$SOA,dats$congr,dats$errors,
   ylab='Mean proportion of directional errors', xlab='SOA',
   type='b',pch=c(0,15,17),legend=FALSE) ## eliminate default legend
legend('topright',legend=c('a - congruent','b - inc. goal-directed',
   'c - inc. no-goal-directed'),lty=c(3,2,1), pch=c(0,15,17),
   bty='n', title='Congruency')
```

<div class="figure">
<img src="07_graphics_files/figure-html/legend1-1.png" alt="Plot with &quot;manually&quot; added legend" width="326.4" />
<p class="caption">(\#fig:legend1)Plot with "manually" added legend</p>
</div>

here we first generated the plot, and afterwards we added the legend. The first argument, `topright` indicates the position where we want the legend to appear, other possible values are `bottomright`, `bottomleft`, `right`, `bottom`, `center` and so on. It is also possible to specify the position of the legend by giving the coordinates of its top-left corner, for the above example we might have written:


```r
legend(x=2.3, y=0.2, legend=c('a - congruent','b - inc. goal-directed',
   ....example not continued
```

The text of the legend is given as a character vector, each element of the vector represents one item of the legend. 

Since we're using both different line types, and different symbols to differentiate the lines in the interaction plot, for the legend we need to specify both in the legend, with the `lty`, and `pch` arguments, the line types and symbols of course, should be the same as those used for the plot.

In the above example we suppressed the drawing of a box around the legend setting the `bty` argument to `n`, if you want a box set it to `o` (this is the default anyway, so you don't really need to specify it if you want the box). If you choose to enclose the legend into a box, you can set its background colour through the `bg` argument.

There are of course many other options, to get a full listing, please look up the manual.


### Adding Text

You can insert text in a graph with the `text` function, you have just to specify the x and y coordinates of the point on which to center the text:

```r
plot(x, y)
text(x=3,y=1.5, "mean for control group")
```
if you want to use mathematical symbols you can use the `expression` function:


```r
text(x=3, y=1.5, expression(alpha))
```

more details on how to use mathematical symbols are given in Section \@ref(plotmath) .
           
### Adding a Grid {#addgrid}

A grid can be easily added to an existing plot with the function `grid`

```r
s <- seq(from=0, to=2*pi, length=100)
plot(s, sin(s))
plot(s, sin(s), type='l')
grid() ##add the grid
```

The default colour for the grid is lightgray, you can choose another colour setting the `col` option:


```r
grid(col="red")
```

### Setting the Axes {#axes}


If you don't like the way the axes are set for a given plot, you can draw the plot without them first, and then add customised axes with the `axis+ function. There are several ways to get rid of the default axes on a plot:


```r
plot(1:10, axes=FALSE) #do not draw any axes or box around
                       # the plot
plot(1:10, xaxt="n")       #don't draw the x axis alone
plot(1:10, yaxt="n")       #don't draw the y axis alone
```

Once you've removed of one or more axis you can draw them calling the `axis` function:


```r
axis(1, at=seq(1, 10, 3), labels=as.character(seq(1, 10, 3)))
```

the first argument specifies the side on which the axis should be drawn, 1 means the bottom axis, 2 the left axis, 3 the top axis, and 4 the right axis. See `?axis` for other arguments to the function.

## Creating Layouts for Multiple Graphs {#glayout}


### mfrow and mfcol {mfrowmfcol}

The parameters `mfrow` and `mfcol` allow you to divide the graphics device you're using (e.g. `X11` or `pdf`) into multiple boxes, each box will contain a new figure. These parameters are set giving a vector of the form:


```r
par(mfrow=c(n_rows,n_columns))
```

where `n_rows` is the number of rows and `n_columns` is the number of columns you want for your layout. For example, the following will create a layout with 2 rows and 3 columns as you can see in Figure \@ref(fig:mfrow23):


```r
par(mfrow=c(2,3))
symb <- as.character(1:6)
for(i in 1:6){
    plot(1, 1, pch=symb[i], xlab='', ylab='')
} 
```

<div class="figure">
<img src="07_graphics_files/figure-html/mfrow23-1.png" alt="A 2x3 Layout with `mfrow`" width="326.4" />
<p class="caption">(\#fig:mfrow23)A 2x3 Layout with `mfrow`</p>
</div>


`mfcol` works exactly the same way as `mfrow`, but the figures are drawn in sequence by column rather than by row, for example the following code yields the Figure \@ref(fig:mfcol23)

```r
par(mfcol=c(2,3))
symb<-as.character(1:6)
for(i in 1:6){
  plot(1,1,pch=symb[i],xlab='',ylab='')
} 
```

<div class="figure">
<img src="07_graphics_files/figure-html/mfcol23-1.png" alt="A 2x3 Layout with `mfcol`" width="326.4" />
<p class="caption">(\#fig:mfcol23)A 2x3 Layout with `mfcol`</p>
</div>

of course you can get other layouts, try:


```r
par(mfrow=c(2,2)) ## 4 boxes
par(mfrow=c(1,3)) ## one row, 3 cols
```

Rather than having the figures drawn sequentially, following the order determined by `mfrow` or `mfcol`, it is possible to specify directly the position for the next figure using the `mfg` parameter. The next example will draw the plot in the slot defined by the crossing between the second row and the first column of a 2x2 layout:


```r
par(mfrow=c(2,2)) ##create 2x2 layout
par(mfg=c(2,1))   ##set next fig at 2dn row, 1st col
plot(1:100, sin(1:100))
```

### layout {#layout}

A more flexible way to divide a graphics device into multiple plotting regions is given by the `layout` function. With the `layout` function, the graphics device is divided into a matrix of *n_rows x n_cols* sub-regions, and each figure is assigned to one or more of these sub-regions. Let's look at an example:


```r
m <- matrix(c(1,2,3,4), nrow=2, byrow=TRUE)
m
```

```
##      [,1] [,2]
## [1,]    1    2
## [2,]    3    4
```

```r
layout(m)
```

the matrix `m` we've created, completely defines our layout for the graphics window. The window is divided into 2 x 2=4 sub-regions. Moreover, the first figure is assigned the sub-region on the top-left of the  window, the second figure the sub-region at the top-right, the third the region at the bottom-left, and the fourth the region at the bottom-right. This example is in itself not very different from what we would get with `mfrow`, however two key differences make `layout` more powerful than `mfrow`. First, it is possible to assign more than a single sub-region to a figure, for example:


```r
m <- matrix(c(1,1,2,3), nrow=2, byrow=TRUE)
m
```

```
##      [,1] [,2]
## [1,]    1    1
## [2,]    2    3
```

```r
layout(m)
```

divides the graphics window into 4 sub-regions as above, but the first figure is assigned the two sub-regions on top, while the bottom-left sub-region is assigned to the second figure, and the bottom-right to the third.

The second key difference with `mfrow` is that with `layout` it is possible to define the width of the columns, and the height of the rows composing the array of sub-regions in the graphics window. Width and height are given as vectors of *relative* widths and heights. For example:


```r
m <- matrix(c(1,1,2,3), nrow=2, byrow=TRUE)
m
```

```
##      [,1] [,2]
## [1,]    1    1
## [2,]    2    3
```

```r
layout(m, width=c(1/4,3/4), height=c(2/3,1/3))
```

will make the first column 1/4 of the total width, and the second column the other 3/4, the same reasoning applies to the row heights.
The command `layout.show(n)`, where `n` is the number of $n^{th}$ figure that is going to be plotted, will show the outline of its layout in the graphics window.


## Graphics Device Regions and Coordinates

In traditional R graphics the graphics device (e.g. the `X11` window where your plot and annotations appear) is divided into different regions (see Figure \@ref(fig:figureregion)):

- the *plotting region* is the area in which the drawing of points or lines representing your data occur. The plotting region is contained into the *figure region*
- the *figure region* is composed of the plotting regions plus the margins where the plot can be annotated with labels for the axes, a title etc...
- the *outer margins* surround the figure regions. The outer margins are usually set to zero (i.e. there are no outer margins), they become useful however for annotating multiple plots that appear on the same page (e.g.~plots generated with `mfrow+). When multiple plots are arranged on the same page (or device), each is assigned  a *figure region* with its own margins, so the outer margins can be used for annotating the overall page (see Figure \@ref(fig:figureregionsubplot)).

<div class="figure">
<img src="07_graphics_files/figure-html/figureregion-1.png" alt="The figure region includes the plotting region and the margins" width="326.4" />
<p class="caption">(\#fig:figureregion)The figure region includes the plotting region and the margins</p>
</div>

<div class="figure">
<img src="07_graphics_files/figure-html/figureregionsubplot-1.png" alt="Device with outer margins and multiple figure regions" width="326.4" />
<p class="caption">(\#fig:figureregionsubplot)Device with outer margins and multiple figure regions</p>
</div>
  
The width and height of the device is usually specified when the device is opened, for example:

```r
X11(width=8, height=8)
```

opens a `X11` device measuring 8x8 inches. The size of an open device can be queried with `par("din")`, this is a read only graphics parameter, which I guess means that once a certain device is opened its size cannot be changed (an `X11` window however can be re-sized with the mouse, and `par("din")` correctly reports the new size). 
Different units of measure can be used to specify the size of the areas inside a device, some of these units will be shortly introduced here, others will be explained when they are first used:

- *inches*: an inch is 2.54 centimetres (notice that the actual physical measure of what you see on your monitor may depend on your monitor's settings, e.g. dpi, screen resolution, etc...)
- *lines of text*: this measure depends on the value of `cex` and `pointsize`
- *Normalised Device Coordinates (NDC)*: the device region is 1x1 NDC whatever the actual physical measure. The lower left corner has coordinates (x=0, y=0) and the upper right corner (x=1, y=1). Using NDC thus the size of regions inside the device can be specified in relative terms to the device size.


In the next paragraphs the graphics parameters for controlling the different regions inside the device will be explained. Always keep in mind the layout of a graphics device in R (see Figure \@ref(fig:figureregion) and  Figure \@ref(fig:figureregionsubplot)).

#### Figure Region

The figure region can be set either in inches or in normalised device coordinates. 

- `fig`: NDC coordinates of the device in the form `c(x1, x2, y1, y2)`, where (x1, y1) are the coordinates of the lower left corner, and (x2, y2) are the coordinates of the upper right corner). Example:


```r
par(fig=c(0.1, 0.5, 0.1, 0.6)) 
plot(1:10)
par(fig=c(0.5, 1, 0.1, 0.6)) # the next call to plot will erase the
                             # current plot
plot(1:10)
par(fig=c(0, 0.5, 0.1, 0.6), new=TRUE) # the next call to plot will not 
                                       # erase the current plot
plot(1:10)
```
as shown in the example to change the figure region without starting a new plot add `new=TRUE`, this may be used for creating complex arrangements for multiple plots within the same device. 

- `fin`: the figure region dimension (width, height) in inches. Example:


```r
par(fin=c(5,5))
```

#### Plotting Region

-  `plt`: a vector of the form `c(x1, x2, y1, y2)` giving the coordinates of the plot region as fractions of the current figure region
- `pin`: the current plot dimensions, `(width,height)`, in inches

#### Margins

- `mar`: the width of the margins for the four sides of the plot, specified in terms of *lines of text*. The margins are specified in the form `c(bottom, left, top, right)`. The default is `c(5,4,4,2) + 0.1`. Example:

```r
par("mar") # get current margins size
par(mar=c(6, 2, 3, 0) + 0.1) #set new margins size
```
- `mai`: the same as `mar`, but the unit of measure is inches rather than lines of text

#### Outer Margins

- `oma`: a vector of the form `c(bottom, left, top, right)` giving the size of the outer margins in lines of text
- A vector of the form `c(bottom, left, top, right)` giving the size of the outer margins in inches
- A vector of the form `c(x1, x2, y1, y2)` giving the outer margin region in normalised device coordinates (NDC)

  
##Plotting from Scratch {#plotfromscratch}


The high level plotting functions such as `plot`, `histogram`, `barplot` and so on, provide a good and quick way to produce graphs. Plotting from scratch, using the low-level plotting commands is generally not necessary, unless you want to create some new, customised plotting functions. Learning to plot "from scratch", however is a very good way to learn how graphics parameters work, which is often necessary to customise plots created with the high-level plotting functions.

We'll start with a very simple example of a scatterplot:


```r
plot.new()
plot.window(xlim=c(0,10), ylim=c(0,10))
```

the `plot.new()` command creates a frame for plotting, and opens a graphics device if there is not one already opened. `plot.window` defines the limits for the x and y axes, points outside these limits will not appear in the plot. After these two commands we're ready to do the actual drawing


```r
points(x=c(1,2,3,4,5,9), y=c(2,5,3,4,5,3))
axis(side=1)
axis(side=2)
```

`points` will draw points at the coordinates given in the `x` an `y` arguments. To complete this very minimal plot you need at least some axes. The `axis` function adds the axis, the `side` argument specifies where the axis should be drawn, 1 means at the "bottom", 2 at the "left" side, and so on in a clockwise fashion.


## Colours for Graphics

The command `colours` gives a list of built-in colours available for graphics in R. You can see some of these colours in Figure \@ref(fig:color1). There are 101 built-in shades of gray, from `gray0`, that is almost black, to `gray100` that is almost white, you can see some of them in Figure \@ref(fig:gray). A complete table of built-in R colours is given in Appendix \@ref(fullcoltable).

<div class="figure">
<img src="graphics/colours_table.png" alt="Some built-in colours in R."  />
<p class="caption">(\#fig:color1)Some built-in colours in R.</p>
</div>

<div class="figure">
<img src="graphics/grays_table.png" alt="Different shades of gray."  />
<p class="caption">(\#fig:gray)Different shades of gray.</p>
</div>


You can also specify colours in rgb values. By default R accepts values in the range 0-1, but you can change the range with the `max` option to set the range as 0-255. Please note that changing the range doesn't change the colours you can use, it just changes the values you use to specify them, so for example the following graphs will have the same colours:


```r
vec = c(3,6)
barplot (vec, col= c(rgb(0.176, 0.262, 0.49),
         rgb(0.568, 0.254 0.654)))
barplot (vec, col= c(rgb(45,67,125, max=255),
         rgb(145,65,167, max=255)))
```

the first uses the default range, and the second uses the range 0-255, but I simply derived the values for the first graph, dividing those for the second by 255.

The function `col2rgb` can be used to get the rgb values of a built-in colour from its name. The rgb values are given in this case in the range 0-255. Here's an example:


```r
col2rgb("lightslateblue")
```

```
##       [,1]
## red    132
## green  112
## blue   255
```

### Colour Opacity

The `adjustcolor` function can be used to set the opacity of a color using the `alpha.f` argument:


```r
mycol = adjustcolor("skyblue", alpha.f=0.5) 
x = rnorm(1000)
y = rnorm(1000)
plot(x, y, pch=21, bg=mycol, cex=2)
```

<div class="figure">
<img src="07_graphics_files/figure-html/coltransp-1.png" alt="Colour opacity" width="326.4" />
<p class="caption">(\#fig:coltransp)Colour opacity</p>
</div>

the `adjustcolor` function accepts a vector of colors as an argument, so you can change the transparency of several colours at


## Managing Graphic Devices

### Opening Another Graphics Window

When you issue the command for a graph R opens a window to show it, if you afterwards issue another command for a graph, if the previous window is still open, R doesn't open another one, but rather replaces the old graph with the new one. If you wish to show the new graph in a separate window, you have to open the graphic device yourself, this is accomplished with the command `X11` under Unix and with the command `windows` under the Windows OS. The device window can also be closed from the command line with:


```r
dev.off()
```

if you have many device windows open and you want to close them all at once use:


```r
graphics.off()
```
For further functions to manage multiple device windows see `?dev.set`.

### Exporting Graphics


With R it's also possible to export your graphics in different file formats, such as JPEG or postscript files. To do this, you need to open first the graphics device you want to use, then insert the command for the graphic, and finally turn off the graphic device. Here's an example of how to produce a graphic in JPEG format: 


```r
jpeg(file="plot.jpeg")
plot(x,y)
dev.off()
```

Other devices you can use, with their corresponding file format are `pdf`, `postscript`, `png` and `bitmap`.


## Mathematical expressions and variables {#plotmath}

It is possible to use mathematical symbols in plot labels and text. The base system for providing math symbols in plot annotations has been described by @MurrellAndIhaka2000, which is a recommended reading. An overview of the system with a comprehensive list of all the symbols can be obtained with `?plotmath`. I don't find the system particularly intuitive, and can't claim to fully understand its inner workings, but I'll nevertheless attempt to explain in rough terms how it works.

The basic idea is that instead of providing a string to `text`, `xlab` or other functions through which you want to plot some text, you provide an `expression`, in the sense of a mathematical expression. Two examples are given below:

```r
plot.new(); plot.window(xlim=c(4, 6), ylim=c(0, 5))
text(5, 1, labels=expression(alpha/(x+2)))
text(5, 2.5, labels=expression(frac(alpha, x+2)))
box()
```

<img src="07_graphics_files/figure-html/unnamed-chunk-48-1.png" width="326.4" />

note how `alpha` is turned into the corresponding Greek letter. Often you'll want to combine strings with mathematical expressions in labels. This can be achieved using the `paste` function, as shown below:

```r
plot.new(); plot.window(xlim=c(4, 6), ylim=c(0, 5))
text(5, 1, labels=expression(paste("Amplitude (", mu, "V)")))
box()
```

<img src="07_graphics_files/figure-html/unnamed-chunk-49-1.png" width="326.4" />

Strings and mathematical expressions can also be combined using the multiplication (`*`) operator (e.g. `expression("Amplitude (" * mu * "V)")`), but this seems somewhat improper and runs into limitations. For example `expression(alpha == 3 * beta == 2)` results in an error, probably because it is not a valid mathematical expression, while `expression(paste(alpha == 3, beta == 2))` works without errors.

Spaces between symbols can be obtained by using one or more tilde (`~`) operators, as shown below:

```r
plot.new(); plot.window(xlim=c(4, 6), ylim=c(0, 5))
text(5, 1, labels=expression(paste("No spaces: ", alpha * beta)))
text(5, 2.5, labels=expression(paste("Spaces: ", alpha ~ beta)))
box()
```

<img src="07_graphics_files/figure-html/unnamed-chunk-50-1.png" width="326.4" />

Sometimes you may want to print the value of a variable inside the expression of a plot label. In this case you can use the `substitute` function:

```r
x = rnorm(100); y=rnorm(100)
corrOut = cor.test(x,y)
corrEst = corrOut$estimate
corrPVal = corrOut$p.value
plot(x,y)
title(main=substitute(rho == v1, list(v1=round(corrEst,2))))
```

<img src="07_graphics_files/figure-html/unnamed-chunk-51-1.png" width="326.4" />

the second argument to the function is a list of all the variable values that need to be substituted. In the example below two values are substituted:

```r
plot(x,y)
title(main=substitute(paste(rho == v1, "; ", italic(p) == v2),
                      list(v1=round(corrEst,2), v2=round(corrPVal, 2))))
```

<img src="07_graphics_files/figure-html/unnamed-chunk-52-1.png" width="326.4" />

A few more example of mathematical expressions in labels are given below:

```r
uVText = expression(paste("Amplitude (", mu, "V)"))
dF0Text = expression(paste( Delta, 'F0 (%)'))
dpText = expression(paste(italic("d' ")))
subText1 = expression(paste(sigma, scriptscriptstyle(c), " (Cents)"))
subText2 = expression(paste(delta, scriptscriptstyle(k)  %+-% 162.5,
                            ' Cents', sep=''))
sqText = expression(paste('F0 Acceleration (Hz/', s^2, ')'))
piText = expression(paste('Start Phase 1.5', pi,))
betaText = expression(beta[0])
log10Text = expression(log[10](nu))
uVsqText = expression(paste('Level ', italic('re'), ' 1 ',
                            mu, V^{2}, ' (dB)'))
bdText = expression(paste( bold("Enhancement ("),
                          italic("d'"), bold("units)")))
zScoreText = expression(paste(italic(z), " Score"))
densText = expression(paste("Density (", kg/m^3, ")"))
beta2Text = expression(mu[beta[0]])
noiseText = expression(paste("Noise [", log[10], "(Energy)]"))
PTAText = expression(paste("PTA"["0.5-2"], " (dB)"))
RSqText = expression("R"^"2")
atopText = expression(atop("PTA"["1-2"],"(dB SPL)"))
latencyText = expression(paste("Latency (", italic(z),
                               " Score)"))

par(mfrow=c(2,2))

plot.new(); plot.window(xlim=c(0, 10), ylim=c(0, 10))
text(5, 1.00, labels=uVText)
text(5, 2.25, dF0Text)
text(5, 3.50, dpText)
text(5, 4.75, subText1)
text(5, 6.00, subText2)
text(5, 7.25, sqText)
text(5, 8.5, piText)
text(5, 9.75, betaText)
box()

plot.new(); plot.window(xlim=c(0, 10), ylim=c(0, 10))
text(5, 1.00, labels=log10Text)
text(5, 2.25, uVsqText)
text(5, 3.50, bdText)
text(5, 4.75, zScoreText)
text(5, 6.00, densText)
text(5, 7.25, beta2Text)
text(5, 8.5, noiseText)
text(5, 9.75, PTAText)
box()

plot.new(); plot.window(xlim=c(0, 10), ylim=c(0, 10))
text(5, 1.00, labels=latencyText)
text(5, 2.25, RSqText)
text(5, 3.50, "")
text(5, 4.75, "")
text(5, 6.00, "")
text(5, 7.25, "")
text(5, 8.5, "")
text(5, 9.75, "")
box()
title(xlab=atopText)
```

<img src="07_graphics_files/figure-html/unnamed-chunk-53-1.png" width="652.8" />


