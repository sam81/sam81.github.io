# Preparing graphics for publications {#graphicspublications}



Academic journals usually have a number of stylistic requirements for figures, including for example font family and size, absence of decorative elements such as borders around the figure, and labeling of figures with letters in multi-panel figures. Some people first produce a figure in R (or other software), and then post-process it "manually" using graphics editing software to meet the journal requirements. While this is a viable option, I find it very inefficient and prefer to obtain publication-quality figures directly in R. 

## Figure size and fonts

This section will cover some common requirements regarding fonts and print size for publication-quality figures, and applies for figures generated with any of the R graphics systems (R base, `ggplot`, and `lattice`). The next sections will present specific examples with each graphics system.

Statistical plots for print journals should be saved in a [vector graphics](https://en.wikipedia.org/wiki/Vector_graphics) format such as `pdf` or `eps`, while images should be saved in a [raster graphics](https://en.wikipedia.org/wiki/Raster_graphics) format such as `tiff`.

Academic journals typically require using a [sans-serif](https://en.wikipedia.org/wiki/Sans-serif) font with a 12 point size for figure annotations. Use of fonts for R graphics was covered in Section \@ref(fonts), but we'll present some examples in the next sections.

Most journals require you to submit figure files with embedded fonts. For `pdf` files, the easiest way to do this is to use the `cairo_pdf` function instead of the `pdf` function for base R and `lattice` graphics, and use the `device=cairo_pdf` in `ggsave` for `ggplot2` graphics; the `pdf` files saved in this way will have their fonts embedded (see Section \@ref(fonts)). Alternatively, you can embed fonts using an external program. On Linux I use the following bash script to embed fonts through the `gs` program:

```bash
#! /bin/bash

#usage embed_pdf_fonts input output
gs -q -dNOPAUSE -dBATCH -dEPSCrop \
   -sDEVICE=pdfwrite -dPDFSETTINGS=/prepress \
   -dPrinted=false -sOutputFile=$2 $1 
```

Many academic journals use a two column layout, and have guidelines indicating the maximal physical size of a figure. A figure typically spans either one or two columns, although sometimes also figures spanning one and a half, or only half a column are allowed. The exact requirements re maximum figure width by figure type (1-col, 2-col, etc...) vary from journal to journal, but they are usually close to those indicated in Table \@ref(tab:figsize).

Fig. type    Fig. width
------------ ------------------ 
1/2 column   1.535 in (39 mm)
1 column     3.307 in (84 mm)
1.5 columns  5.078 in (129 mm)
2 columns    6.85 in  (174 mm)

Table: (\#tab:figsize) Typical figure width in academic journals for different figure types. Width in given in inches (in), and millimeters (mm).

The default print size of the figures used by the `cairo_pdf` and `ggsave` functions (i.e. if you don't specify `width` and `height`) is much larger than the typical 1-column figure size required by academic journals. It becomes more difficult to fit certain elements (e.g. axis labels) in a smaller space, so it's best to start saving figures in the smaller size typically required by journals early in the paper write-up process and make sure they look right in that size.

### Baser R and `lattice` plots

To obtain `pdf` figures of a given print width and height we can pass the `width` and `height` arguments to the `cairo_pdf` function, specifying the desired values in inches:

```r
cairo_pdf("Fig1.pdf", width=3.307, height=3.307)
```

The default font used by `cairo_pdf` is sans-serif at size 12, so you don't really need to change that, should you need to you can pass the `family` and `pointsize` arguments to the function:

```r
cairo_pdf("Fig1.pdf", width=3.307, height=3.307,
          family="Helvetica", pointsize=12)
```

### `ggplot2`

The pointsize and font depend on the theme you're using. Many themes use a sans-serif font, so you probably don't need to change that. However, you may need to change the point size (e.g. the default `ggplot2` theme uses an 11 point size). You can change that with `theme(text=element_text(size=12))`. The following code shows an example of changing both the font family and the point size:

```r
n=100
dat=data.frame(x=rnorm(n), y=rnorm(n))
p = ggplot(dat, aes(x=x, y=y)) + geom_point()
p = p + xlab("X-Label") + ylab("Y-Label")
p = p + theme(text=element_text(size=12, family="Helvetica"))
```

Alternatively, you can set the font family and pointsize directly when calling a specific theme, as in the following example:

```r
p = p + theme_classic(base_family="Helvetica", base_size=12)
```

To obtain figures of a certain physical width and height we can pass the `width` and `height` arguments to the `ggsave` function:


```r
ggsave("Fig1.pdf", plot1, width=3.307, height=3.307, device=cairo_pdf)
```

by default `width` and `height` units are in inches, though this can be changed by passing a `units` argument if desired (see `?ggsave`).

## Themes

Many journals require a rather plain style for figures, with a white background, no grid lines, and no "borders" or "boxes" around the figure beside the x and y axes.

### Base R

By default plots have a white background and no grid. The "borders" around the figures can be removed through the `bty` argument. Setting `bty` to `n` removes the borders from all sides, while setting it to `L` leaves the borders around the bottom and left axes:

```r
plot(rnorm(100), bty="L")
```

### `ggplot2`

The `theme_classic` theme generally follows the style commonly requested by academic journals, so in many cases all you'll need to do is set this theme, increasing the font point size from 11 to 12:

```r
n=100; dat=data.frame(x=rnorm(n), y=rnorm(n))
p = ggplot(dat, aes(x=x, y=y)) + geom_point()
p = p + xlab("X-Label") + ylab("Y-Label")
p = p + theme_classic(base_size=12)
```

Alternatively, you can use the `theme_cowplot` from the [`cowplot`](https://cran.r-project.org/web/packages/cowplot/index.html) package, which is very similar to `theme_classic`.

### `lattice`

## Multi-panel figures

### `ggplot2`

The `plot_grid` function from the `cowplot` package can be used to arrange separate `ggplot2` plots in a grid layout and label them, as shown in Figure \@ref(fig:plotgrid1), which was obtained with the following code:

```r
library(cowplot); library(ggplot2)
data(iris)
p1 = ggplot(iris, aes(Sepal.Width, Sepal.Length)) + geom_point()
p2 = ggplot(iris, aes(Petal.Width, Petal.Length)) + geom_point()
plot_grid(p1, p2, labels = c('A', 'B'), label_size = 12)
```

<div class="figure">
<img src="12_figures_for_publication_files/figure-html/plotgrid1-1.png" alt="Multi-panel figure arranged and labeled with the `plot_grid` function." width="489.6" />
<p class="caption">(\#fig:plotgrid1)Multi-panel figure arranged and labeled with the `plot_grid` function.</p>
</div>

### `lattice`

The `plot_grid` function from the `cowplot` package can be used to arrange separate `lattice` plots in a grid layout and label them, as shown in Figure \@ref(fig:lattplotgrid1), which was obtained with the following code:


```r
library(lattice)
p1 = xyplot(Sepal.Length~Sepal.Width, data=iris)
p2 = xyplot(Petal.Length~Petal.Width, data=iris)
plot_grid(p1, p2, labels = c('A', 'B'), label_size = 12)
```

<div class="figure">
<img src="12_figures_for_publication_files/figure-html/lattplotgrid1-1.png" alt="Multi-panel figure arranged and labeled with the `plot_grid` function." width="489.6" />
<p class="caption">(\#fig:lattplotgrid1)Multi-panel figure arranged and labeled with the `plot_grid` function.</p>
</div>

For multi-panel plots obtained via panel conditioning, we can use the `page` function to add labels. First we write some boilerplate code for preparing the plot.

```r
library(grid)
ir2 = iris[iris$Species!="virginica",]
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
```

Now we draw the graph (shown in Figure \@ref(fig:lattfiglab1)) with `xyplot`, and add labels via the `page` function which uses the whole display area as the default viewport. We use `grid` functions inside the `page` function to annotate the plot. 

```r
p = xyplot(Sepal.Length~Sepal.Width | Species, data=ir2, axis=axis.L,
           scales=list(x=list(alternating=c(1,1)),
                       y=list(limits=c(min(ir2$Sepal.Length),
                                       max(ir2$Sepal.Length)),
                              relation="free")),
           strip=FALSE,
           par.settings=list(axis.line=list(col="transparent")),
           page = function(n) {
               grid.text(label = "A",
                         x = unit(0.05, "npc"), y = unit(0.96, "npc"),
                         just = c("left", "center"), gp=gpar(fontface=2))
               grid.text(label = "B",
                         x = unit(0.525, "npc"), y = unit(0.96, "npc"),
                         just = c("left", "center"), gp=gpar(fontface=2))
           }
           )
print(p)
```

<div class="figure">
<img src="12_figures_for_publication_files/figure-html/lattfiglab1-1.png" alt="`lattice` panels with labels." width="489.6" />
<p class="caption">(\#fig:lattfiglab1)`lattice` panels with labels.</p>
</div>
