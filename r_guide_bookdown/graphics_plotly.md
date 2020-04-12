# Plotly {#plotly}

The ["plotly for R"](https://cpsievert.github.io/plotly_book/) book [@Sievert2017] is the best introduction for
learning plotly.



```r
library(plotly)
x = rnorm(10); y=rnorm(10)
plot_ly(x=x, y=y, type="scatter", mode="markers")
```

<div class="figure">
<!--html_preserve--><div id="htmlwidget-b63ea9612a4542c0262d" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-b63ea9612a4542c0262d">{"x":{"visdat":{"210613a2973a":["function () ","plotlyVisDat"]},"cur_data":"210613a2973a","attrs":{"210613a2973a":{"x":[-0.209278387058214,0.539368608279536,0.781801254313261,0.00124609527783263,2.15825411397926,0.334612201443295,1.72367641012015,-0.88040949696454,-0.164541074674266,-0.771977207246512],"y":[-1.89946878477394,-0.94335364804952,0.204653100339705,0.493571541996393,-0.580336784788311,0.0342479317505636,1.34357738097855,0.251507935415238,0.625509263433732,1.31950867269273],"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.209278387058214,0.539368608279536,0.781801254313261,0.00124609527783263,2.15825411397926,0.334612201443295,1.72367641012015,-0.88040949696454,-0.164541074674266,-0.771977207246512],"y":[-1.89946878477394,-0.94335364804952,0.204653100339705,0.493571541996393,-0.580336784788311,0.0342479317505636,1.34357738097855,0.251507935415238,0.625509263433732,1.31950867269273],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
<p class="caption">(\#fig:plotly1)Plotly example</p>
</div>


```r
plot_ly(x=x, y=y, type="scatter", mode="markers",
        marker=list(color="black" , size=15 , opacity=0.8))
```

<!--html_preserve--><div id="htmlwidget-06fa6735e6ac1a59c8e4" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-06fa6735e6ac1a59c8e4">{"x":{"visdat":{"2106452cc1c0":["function () ","plotlyVisDat"]},"cur_data":"2106452cc1c0","attrs":{"2106452cc1c0":{"x":[-0.209278387058214,0.539368608279536,0.781801254313261,0.00124609527783263,2.15825411397926,0.334612201443295,1.72367641012015,-0.88040949696454,-0.164541074674266,-0.771977207246512],"y":[-1.89946878477394,-0.94335364804952,0.204653100339705,0.493571541996393,-0.580336784788311,0.0342479317505636,1.34357738097855,0.251507935415238,0.625509263433732,1.31950867269273],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.209278387058214,0.539368608279536,0.781801254313261,0.00124609527783263,2.15825411397926,0.334612201443295,1.72367641012015,-0.88040949696454,-0.164541074674266,-0.771977207246512],"y":[-1.89946878477394,-0.94335364804952,0.204653100339705,0.493571541996393,-0.580336784788311,0.0342479317505636,1.34357738097855,0.251507935415238,0.625509263433732,1.31950867269273],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8,"line":{"color":"rgba(31,119,180,1)"}},"type":"scatter","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## Using plotly with knitr

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


