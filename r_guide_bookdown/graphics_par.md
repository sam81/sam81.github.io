
## Setting Graphics Parameters {#par}



Graphics parameters allow you to tweak many elements of a plot, such as the font for the labels, the symbols or line types to use and so on, see `?par` to get a full list and description of these parameters. Graphics parameters can be set and accessed with the function `par`, called without arguments, as `par`, it will give you a full list of the current defaults, if you want to query only one or a few parameters use:

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

The following sections will give a more in depth explanation of some graphics parameters, before that however we'll have a closer look at how to use `par`.

#### Saving and Restoring Graphics Parameters

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
s <- seq(0, 10, .1) 
plot(s, sin(s))           #we plot something
```

```r
par(oldpar)              # and then we restore the old parameters
```

notice that calling `par`, opens a graphics device if there is not one already open, the changes you do using `par` apply only to this graphics device, any other new graphic device that you open will have the default graphics parameters. 

#### List of Graphics Parameters by Category

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


Parameter   Function
---------   ------------------------------------------
`family`    font family (e.g. "sans", "serif", "mono")
`font`      font type for text (1=plain, 2=bold, 3=italic etc...)
`font.axis` font type for axis annotation (1=plain, 2=bold, 3=italic etc...)
`font.lab`  font type for x and y labels (1=plain, 2=bold, 3=italic etc...)
`font.main` font type for main titles (1=plain, 2=bold, 3=italic etc...)
`font.sub`  font type for sub titles (1=plain, 2=bold, 3=italic etc...)
`ps`        point size of text

Table: (\#tab:parfonts) Parameters for fonts.



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
<img src="graphics_par_files/figure-html/lty-1.png" alt="The six default line types in R" width="576" />
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
<img src="graphics_par_files/figure-html/pch-1.png" alt="Plotting symbols from 0 to 25" width="576" />
<p class="caption">(\#fig:pch)Plotting symbols from 0 to 25</p>
</div>

### Fonts {#fonts}

The interface for setting font parameters for plots in R is somehow complex and I have a limited knowledge of how it works. I nonetheless hope that the notes below can be of some use to other people.

One of the reasons why setting font parameters can be confusing is that font parameters can be affected by several graphics parameters. For example the graphics parameter `ps` sets the point size of the font, but this size can be scaled by the `cex` parameter. Also font parameters can be set not only using the `par` interface, but directly when opening a graphics device, and these two ways of setting font parameters can have subtle differences. For example, the font point size can be set by using the `pointsize` argument when opening a `pdf` device:



this changes the size of both the font and the plotting symbols. The graphics parameter `ps`, however, changes only the size of the font:



the documentation for the graphics parameter `ps` also says that "unlike the pointsize argument of most devices, this does not change the relationship between `mar` and `mai` (nor `oma` and `omi`).". I'm not sure what this means, but just be aware setting `ps` with a call to `par` or setting `pointsize` when opening a graphics device are not equivalent. Also, note that if you set the font size when you open a graphics device, the setting may be overridden by subsequent calls to `par`:



`cex` also overrides the graphics device settings:



The `par` setting `family` can be used to choose a serif, sans serif, or mono font:

<div class="figure">
<img src="graphics_par_files/figure-html/unnamed-chunk-11-1.png" alt="Changing font family" width="576" />
<p class="caption">(\#fig:unnamed-chunk-11)Changing font family</p>
</div>

however, specifying the exact font used is more complicated. One way to do this is by using the `extrafont` package https://cran.r-project.org/web/packages/extrafont/README.html


```r
library(ggplot2)
n=100
dat=data.frame(x=rnorm(n), y=rnorm(n))
p = ggplot(dat, aes(x=x, y=y)) + geom_point()
p = p + xlab("X-Label") + ylab("Y-Label")
p = p + theme(text=element_text(size=12, family="Ubuntu"))
## NOT RUN
##ggsave("cairo_pdf_graphic.pdf", p, width=3.4, height=3.4, device=cairo_pdf)
```

System fonts in pdf files can be used with the `cairo_pdf` device; this has the additional advantage of directly embedding the fonts in the pdf. For R base graphics you can invoke `cairo_pdf()` instead of `pdf()`. For `ggplot2`, when you invoke the `ggsave` function you can pass the argument `device=cairo_pdf` to use the `cairo_pdf` device.


Changing the font style is easy using the `font` parameter, a value of 1 corresponds to a normal (or plain) style, 2 to bold, 3 to italics, 4 to bold italics, and 5 will map the font to a symbol:


<div class="figure">
<img src="graphics_par_files/figure-html/unnamed-chunk-13-1.png" alt="Changing font style" width="576" />
<p class="caption">(\#fig:unnamed-chunk-13)Changing font style</p>
</div>

but note that setting `font` only changes the font style of plotted text. If you want to change the font style of other textual elements such as the axis labels or the plot title you have to set other graphics parameters:

<div class="figure">
<img src="graphics_par_files/figure-html/unnamed-chunk-14-1.png" alt="Changing font style for other textual elements" width="576" />
<p class="caption">(\#fig:unnamed-chunk-14)Changing font style for other textual elements</p>
</div>
	
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
<img src="graphics_par_files/figure-html/legend1-1.png" alt="Plot with &quot;manually&quot; added legend" width="576" />
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
if you want to use LaTeX mathematical symbols, you have to use the `expression` function:


```r
text(x=3, y=1.5, expression(alpha))
```


```r
dpText = expression(italic("d'"))
uVtext = expression(paste('Amplitude (', mu, 'V)'))
deltaEText = expression(paste('Change', Delta, 'E'))
reLevText = expression(paste('Level ', italic('re.'), ' 1 ', mu, V^{2}, ' (dB)'))
```
           


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

## Creating Layouts for Multiple Graphs {glayout}


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
<img src="graphics_par_files/figure-html/mfrow23-1.png" alt="A 2x3 Layout with `mfrow`" width="576" />
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
<img src="graphics_par_files/figure-html/mfcol23-1.png" alt="A 2x3 Layout with `mfcol`" width="576" />
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
<img src="graphics_par_files/figure-html/figureregion-1.png" alt="The figure region includes the plotting region and the margins" width="576" />
<p class="caption">(\#fig:figureregion)The figure region includes the plotting region and the margins</p>
</div>

<div class="figure">
<img src="graphics_par_files/figure-html/figureregionsubplot-1.png" alt="Device with outer margins and multiple figure regions" width="576" />
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
<img src="graphics/colours_table.png" alt="Some built-in colours in R." width="289" />
<p class="caption">(\#fig:color1)Some built-in colours in R.</p>
</div>

<div class="figure">
<img src="graphics/grays_table.png" alt="Different shades of gray." width="276" />
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
<img src="graphics_par_files/figure-html/coltransp-1.png" alt="Colour opacity" width="576" />
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
