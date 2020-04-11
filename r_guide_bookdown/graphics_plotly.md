# Plotly {#plotly}

The ["plotly for R"](https://cpsievert.github.io/plotly_book/) book [@Sievert2017] is the best introduction for
learning plotly.



```r
library(plotly)
x = rnorm(10); y=rnorm(10)
plot_ly(x=x, y=y, type="scatter", mode="markers")
```

<div class="figure">
<!--html_preserve--><div id="htmlwidget-5a3713bb060b73443a4d" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-5a3713bb060b73443a4d">{"x":{"visdat":{"60a735f52577":["function () ","plotlyVisDat"]},"cur_data":"60a735f52577","attrs":{"60a735f52577":{"x":[0.671497612974677,1.02789830810962,0.0120737800834501,1.57339891846654,1.33987037391164,1.24379472723262,0.737334237289542,1.07889279826381,-0.168443885656228,0.106178308008765],"y":[0.231159139251109,3.66552600711294,-1.50726338496514,1.15941349153929,-1.21170687578271,-1.02780876118418,-1.3588595772327,-1.0254190528291,0.398589701237934,0.662948875434488],"mode":"markers","alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[0.671497612974677,1.02789830810962,0.0120737800834501,1.57339891846654,1.33987037391164,1.24379472723262,0.737334237289542,1.07889279826381,-0.168443885656228,0.106178308008765],"y":[0.231159139251109,3.66552600711294,-1.50726338496514,1.15941349153929,-1.21170687578271,-1.02780876118418,-1.3588595772327,-1.0254190528291,0.398589701237934,0.662948875434488],"mode":"markers","type":"scatter","marker":{"color":"rgba(31,119,180,1)","line":{"color":"rgba(31,119,180,1)"}},"error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
<p class="caption">(\#fig:plotly1)Plotly example</p>
</div>


```r
plot_ly(x=x, y=y, type="scatter", mode="markers",
        marker=list(color="black" , size=15 , opacity=0.8))
```

<!--html_preserve--><div id="htmlwidget-56c66bdc26be45547e79" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-56c66bdc26be45547e79">{"x":{"visdat":{"60a7655cd1c1":["function () ","plotlyVisDat"]},"cur_data":"60a7655cd1c1","attrs":{"60a7655cd1c1":{"x":[0.671497612974677,1.02789830810962,0.0120737800834501,1.57339891846654,1.33987037391164,1.24379472723262,0.737334237289542,1.07889279826381,-0.168443885656228,0.106178308008765],"y":[0.231159139251109,3.66552600711294,-1.50726338496514,1.15941349153929,-1.21170687578271,-1.02780876118418,-1.3588595772327,-1.0254190528291,0.398589701237934,0.662948875434488],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8},"alpha_stroke":1,"sizes":[10,100],"spans":[1,20],"type":"scatter"}},"layout":{"margin":{"b":40,"l":60,"t":25,"r":10},"xaxis":{"domain":[0,1],"automargin":true,"title":[]},"yaxis":{"domain":[0,1],"automargin":true,"title":[]},"hovermode":"closest","showlegend":false},"source":"A","config":{"showSendToCloud":false},"data":[{"x":[0.671497612974677,1.02789830810962,0.0120737800834501,1.57339891846654,1.33987037391164,1.24379472723262,0.737334237289542,1.07889279826381,-0.168443885656228,0.106178308008765],"y":[0.231159139251109,3.66552600711294,-1.50726338496514,1.15941349153929,-1.21170687578271,-1.02780876118418,-1.3588595772327,-1.0254190528291,0.398589701237934,0.662948875434488],"mode":"markers","marker":{"color":"black","size":15,"opacity":0.8,"line":{"color":"rgba(31,119,180,1)"}},"type":"scatter","error_y":{"color":"rgba(31,119,180,1)"},"error_x":{"color":"rgba(31,119,180,1)"},"line":{"color":"rgba(31,119,180,1)"},"xaxis":"x","yaxis":"y","frame":null}],"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

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


