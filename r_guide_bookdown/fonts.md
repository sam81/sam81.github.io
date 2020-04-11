# Fonts for graphics {#fonts}

In Section \@ref(parfonts) we mentioned that it is possible to specify a system font (such as Palatino, or Arial) for the R base graphics via the `par(family=...)` setting, but this may or may not work depending on the specific graphics device (and of course also on whether that font is installed in your system or not). 

I'll focus on the issue of generating pdfs with custom fonts because I mostly use pdf for saving R graphics. The `pdf` device *does not* automaticaly embed the fonts in the file, and works only with a limited number of fonts (see `?pdfFonts` for details). An easy way to get around this issues is to use the `cairo_pdf` device instead of the `pdf` device: the `cairo_pdf` device can access all the true type fonts (TTF) and open type fonts (OTF) in your system and also embeds them in the pdf. The device can be used with both base graphics:


```r
cairo_pdf("base_graphics_ubuntu_font.pdf", family="Ubuntu")
plot(1:10)
dev.off()
```

or with ggplot2 (by setting the `device` argument in `ggsave` to `cairo_pdf`:


```r
library(ggplot2)
n=100
dat=data.frame(x=rnorm(n), y=rnorm(n))
p = ggplot(dat, aes(x=x, y=y)) + geom_point()
p = p + xlab("X-Label") + ylab("Y-Label")
p = p + theme(text=element_text(size=12, family="Ubuntu"))
ggsave("ggplot2_cairo_pdf.pdf", p, width=3.4, height=3.4,
       device=cairo_pdf)
```

An alternative solution for using system fonts in pdfs is by using the `extrafont` package https://cran.r-project.org/web/packages/extrafont/README.html . Another alternative is the `showtext` package https://cran.rstudio.com/web/packages/showtext/index.html . Compared to `extrafont` the `showtext` package has the advantage of being able to use open type fonts; however `showtext` has the disadvantage of not rendering the fonts as "drawings" (you can't copy and paste text). Overall, my favorite solution is to use `cairo_pdf` because it handles both TTF and OTF fonts.



