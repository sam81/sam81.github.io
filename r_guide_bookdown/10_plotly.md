# Plotly {#plotly}



The ["plotly for R"](https://cpsievert.github.io/plotly_book/) book [@Sievert2017] is the best introduction for
learning plotly.



```r
library(plotly)
x = rnorm(10); y=rnorm(10)
plot_ly(x=x, y=y, type="scatter", mode="markers")
```

<div class="figure">
<!--html_preserve--><div id="htmlwidget-60b43988b2749679a5d3" style="width:326.4px;height:384px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-60b43988b2749679a5d3">{"x":{"visdat":{"59e54926a8a8":["function () ","plotlyVisDat"]},"cur_data":"59e54926a8a8","attrs":{"59e54926a8a8":{"x":[-0.0832243185962859,-1.2272013965494,-0.0124274550470548,-0.609831182306448,-1.42342036408001,-0.227496464182484,-0.578422983612338,-1.10238181756855,0.117700157559726,-1.40470862135467],"y":[0.0113208511332098,-0.458939648269686,1.0825412364537,0.571774117187679,-0.260206432232977,-0.438826717560869,1.41394541886509,0.688094180775632,0.940586861141531,2.06074428412423],"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.0832243185962859,-1.2272013965494,-0.0124274550470548,-0.609831182306448,-1.42342036408001,-0.227496464182484,-0.578422983612338,-1.10238181756855,0.117700157559726,-1.40470862135467],"y":[0.0113208511332098,-0.458939648269686,1.0825412364537,0.571774117187679,-0.260206432232977,-0.438826717560869,1.41394541886509,0.688094180775632,0.940586861141531,2.06074428412423],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
<p class="caption">(\#fig:plotly1)Plotly example</p>
</div>


```r
plot_ly(x=x, y=y, type="scatter", mode="markers",
        marker=list(color="black" , size=15 , opacity=0.8))
```

<!--html_preserve--><div id="htmlwidget-c3fff49c80ab97d2f86d" style="width:326.4px;height:384px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-c3fff49c80ab97d2f86d">{"x":{"visdat":{"59e517c9711d":["function () ","plotlyVisDat"]},"cur_data":"59e517c9711d","attrs":{"59e517c9711d":{"x":[-0.0832243185962859,-1.2272013965494,-0.0124274550470548,-0.609831182306448,-1.42342036408001,-0.227496464182484,-0.578422983612338,-1.10238181756855,0.117700157559726,-1.40470862135467],"y":[0.0113208511332098,-0.458939648269686,1.0825412364537,0.571774117187679,-0.260206432232977,-0.438826717560869,1.41394541886509,0.688094180775632,0.940586861141531,2.06074428412423],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[-0.0832243185962859,-1.2272013965494,-0.0124274550470548,-0.609831182306448,-1.42342036408001,-0.227496464182484,-0.578422983612338,-1.10238181756855,0.117700157559726,-1.40470862135467],"y":[0.0113208511332098,-0.458939648269686,1.0825412364537,0.571774117187679,-0.260206432232977,-0.438826717560869,1.41394541886509,0.688094180775632,0.940586861141531,2.06074428412423],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8,"line":{"color":"rgba(31,119,180,1)"}},"type":"scatter","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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


