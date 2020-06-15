# Plotly {#plotly}



\BeginKnitrBlock{rmdwarning}<div class="rmdwarning">Figures in this section may not appear in the pdf version of the book. Please use the html version if that is the case: https://sam81.github.io/r_guide_bookdown/preface.html</div>\EndKnitrBlock{rmdwarning}

The ["plotly for R"](https://cpsievert.github.io/plotly_book/) book [@Sievert2020] is the best introduction for
learning plotly.



```r
library(plotly)
x = rnorm(10); y=rnorm(10)
plot_ly(x=x, y=y, type="scatter", mode="markers")
```

<div class="figure">
<!--html_preserve--><div id="htmlwidget-f0a4227ad7001b6496bf" style="width:326.4px;height:384px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-f0a4227ad7001b6496bf">{"x":{"visdat":{"63603e7d3194":["function () ","plotlyVisDat"]},"cur_data":"63603e7d3194","attrs":{"63603e7d3194":{"x":[-0.0301533278610176,-0.329453306593231,1.14453224329946,-0.218112382640143,0.444878315829196,0.00529001327844141,0.611862258305473,0.405162443499927,-0.329061594438906,0.0407986066934483],"y":[0.137643582341123,0.622372534020908,1.46216053390177,-0.257213203887429,-1.69898015716459,-0.479202138464529,-0.539776287479324,0.385736082565637,0.892500328113368,1.32871138142997],"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.0301533278610176,-0.329453306593231,1.14453224329946,-0.218112382640143,0.444878315829196,0.00529001327844141,0.611862258305473,0.405162443499927,-0.329061594438906,0.0407986066934483],"y":[0.137643582341123,0.622372534020908,1.46216053390177,-0.257213203887429,-1.69898015716459,-0.479202138464529,-0.539776287479324,0.385736082565637,0.892500328113368,1.32871138142997],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
<p class="caption">(\#fig:plotly1)Plotly example</p>
</div>


```r
plot_ly(x=x, y=y, type="scatter", mode="markers",
        marker=list(color="black" , size=15 , opacity=0.8))
```

<!--html_preserve--><div id="htmlwidget-1228e638701aad7fb5c9" style="width:326.4px;height:384px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-1228e638701aad7fb5c9">{"x":{"visdat":{"63606c883ef0":["function () ","plotlyVisDat"]},"cur_data":"63606c883ef0","attrs":{"63606c883ef0":{"x":[-0.0301533278610176,-0.329453306593231,1.14453224329946,-0.218112382640143,0.444878315829196,0.00529001327844141,0.611862258305473,0.405162443499927,-0.329061594438906,0.0407986066934483],"y":[0.137643582341123,0.622372534020908,1.46216053390177,-0.257213203887429,-1.69898015716459,-0.479202138464529,-0.539776287479324,0.385736082565637,0.892500328113368,1.32871138142997],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.0301533278610176,-0.329453306593231,1.14453224329946,-0.218112382640143,0.444878315829196,0.00529001327844141,0.611862258305473,0.405162443499927,-0.329061594438906,0.0407986066934483],"y":[0.137643582341123,0.622372534020908,1.46216053390177,-0.257213203887429,-1.69898015716459,-0.479202138464529,-0.539776287479324,0.385736082565637,0.892500328113368,1.32871138142997],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8,"line":{"color":"rgba(31,119,180,1)"}},"type":"scatter","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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


