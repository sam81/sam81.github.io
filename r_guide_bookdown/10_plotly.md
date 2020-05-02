# Plotly {#plotly}



\BeginKnitrBlock{rmdwarning}<div class="rmdwarning">Figures in this section may not appear in the pdf version of the book. Please use the html version if that is the case: https://sam81.github.io/r_guide_bookdown/preface.html</div>\EndKnitrBlock{rmdwarning}

The ["plotly for R"](https://cpsievert.github.io/plotly_book/) book [@Sievert2017] is the best introduction for
learning plotly.



```r
library(plotly)
x = rnorm(10); y=rnorm(10)
plot_ly(x=x, y=y, type="scatter", mode="markers")
```

<div class="figure">
<!--html_preserve--><div id="htmlwidget-bac47fbdddd72d28f25a" style="width:326.4px;height:384px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-bac47fbdddd72d28f25a">{"x":{"visdat":{"22752296ab7e":["function () ","plotlyVisDat"]},"cur_data":"22752296ab7e","attrs":{"22752296ab7e":{"x":[-0.971535881466228,-1.49543005827358,0.241305630982573,-1.11521683868156,1.78310302723466,-0.723796870716765,-1.37222477112664,0.708008734959035,1.76563386221737,1.66159655506408],"y":[-1.60871086003632,0.0621130495253194,-0.349582047598745,0.0788747462399355,-1.06708942986096,-1.30898126849961,1.94731071417712,0.162904535016411,-0.118552801215717,0.0730559427083261],"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.971535881466228,-1.49543005827358,0.241305630982573,-1.11521683868156,1.78310302723466,-0.723796870716765,-1.37222477112664,0.708008734959035,1.76563386221737,1.66159655506408],"y":[-1.60871086003632,0.0621130495253194,-0.349582047598745,0.0788747462399355,-1.06708942986096,-1.30898126849961,1.94731071417712,0.162904535016411,-0.118552801215717,0.0730559427083261],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
<p class="caption">(\#fig:plotly1)Plotly example</p>
</div>


```r
plot_ly(x=x, y=y, type="scatter", mode="markers",
        marker=list(color="black" , size=15 , opacity=0.8))
```

<!--html_preserve--><div id="htmlwidget-3073a07e571f693b2b15" style="width:326.4px;height:384px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-3073a07e571f693b2b15">{"x":{"visdat":{"2275719211e7":["function () ","plotlyVisDat"]},"cur_data":"2275719211e7","attrs":{"2275719211e7":{"x":[-0.971535881466228,-1.49543005827358,0.241305630982573,-1.11521683868156,1.78310302723466,-0.723796870716765,-1.37222477112664,0.708008734959035,1.76563386221737,1.66159655506408],"y":[-1.60871086003632,0.0621130495253194,-0.349582047598745,0.0788747462399355,-1.06708942986096,-1.30898126849961,1.94731071417712,0.162904535016411,-0.118552801215717,0.0730559427083261],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.971535881466228,-1.49543005827358,0.241305630982573,-1.11521683868156,1.78310302723466,-0.723796870716765,-1.37222477112664,0.708008734959035,1.76563386221737,1.66159655506408],"y":[-1.60871086003632,0.0621130495253194,-0.349582047598745,0.0788747462399355,-1.06708942986096,-1.30898126849961,1.94731071417712,0.162904535016411,-0.118552801215717,0.0730559427083261],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8,"line":{"color":"rgba(31,119,180,1)"}},"type":"scatter","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Using plotly with knitr

\BeginKnitrBlock{rmdwarning}<div class="rmdwarning">What is described in this section doesn't seem to work anymore. I'll update the section once I've found out a solution.</div>\EndKnitrBlock{rmdwarning}

Plotly figures are rendered directly in the html output with knitr:

````markdown
```{r chunk-label, fig.cap = 'A figure caption.'}
plot_ly(economics, x = ~date, y = ~unemploy / pop)
```
````

please note that if the plot is assigned to a variable you need to call that variable in the code chunk for the plot to be rendered:


````markdown
```{r chunk-label, fig.cap = 'A figure caption.'}
p = plot_ly(economics, x = ~date, y = ~unemploy / pop)
p #call the variable storing the plot to render it
```
````

Plotly figures can also appear in pdf files generate by knitr if the `webshot` package is installed. Besides this package, you will also need to have PhantomJS (http://phantomjs.org/) installed on your system. You can install both `webshot` and PhantomJS from within R with the following commands:


```r
install.packages("webshot")
webshot::install_phantomjs()
```


