# Plotly {#plotly}

The ["plotly for R"](https://cpsievert.github.io/plotly_book/) book [@Sievert2017] is the best introduction for
learning plotly.



```r
library(plotly)
x = rnorm(10); y=rnorm(10)
plot_ly(x=x, y=y, type="scatter", mode="markers")
```

<div class="figure">
<!--html_preserve--><div id="htmlwidget-35b8252fcf21894e2484" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-35b8252fcf21894e2484">{"x":{"visdat":{"6782a824f44":["function () ","plotlyVisDat"]},"cur_data":"6782a824f44","attrs":{"6782a824f44":{"x":[-1.00428286542442,1.05417795911374,-0.619561494125573,-0.476547721048776,-0.576409175139309,-0.62902736028738,0.751489751709448,0.020015897399275,0.873251166320728,0.564798662718603],"y":[-2.01420092709352,-0.743087656334874,-0.306147590870544,0.776191402304584,-0.967220310097813,0.476765566185596,1.21771654585145,1.09416102445336,-1.46504260528061,0.350142259479373],"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-1.00428286542442,1.05417795911374,-0.619561494125573,-0.476547721048776,-0.576409175139309,-0.62902736028738,0.751489751709448,0.020015897399275,0.873251166320728,0.564798662718603],"y":[-2.01420092709352,-0.743087656334874,-0.306147590870544,0.776191402304584,-0.967220310097813,0.476765566185596,1.21771654585145,1.09416102445336,-1.46504260528061,0.350142259479373],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
<p class="caption">(\#fig:plotly1)Plotly example</p>
</div>


```r
plot_ly(x=x, y=y, type="scatter", mode="markers",
        marker=list(color="black" , size=15 , opacity=0.8))
```

<!--html_preserve--><div id="htmlwidget-5caaf3efe18a2ac42005" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-5caaf3efe18a2ac42005">{"x":{"visdat":{"678241cee4f1":["function () ","plotlyVisDat"]},"cur_data":"678241cee4f1","attrs":{"678241cee4f1":{"x":[-1.00428286542442,1.05417795911374,-0.619561494125573,-0.476547721048776,-0.576409175139309,-0.62902736028738,0.751489751709448,0.020015897399275,0.873251166320728,0.564798662718603],"y":[-2.01420092709352,-0.743087656334874,-0.306147590870544,0.776191402304584,-0.967220310097813,0.476765566185596,1.21771654585145,1.09416102445336,-1.46504260528061,0.350142259479373],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-1.00428286542442,1.05417795911374,-0.619561494125573,-0.476547721048776,-0.576409175139309,-0.62902736028738,0.751489751709448,0.020015897399275,0.873251166320728,0.564798662718603],"y":[-2.01420092709352,-0.743087656334874,-0.306147590870544,0.776191402304584,-0.967220310097813,0.476765566185596,1.21771654585145,1.09416102445336,-1.46504260528061,0.350142259479373],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8,"line":{"color":"rgba(31,119,180,1)"}},"type":"scatter","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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


