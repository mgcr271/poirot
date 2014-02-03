---
title: Leaflet Heat Maps
author: Ramnath Vaidyanathan
class: dark
framework: thinny
highlighter: prettify
hitheme: twitter-bootstrap
mode: selfcontained
url: {lib: ../../libraries}
image: crime_map.png
date: "2014-02-03"
---


```r
library(knitr)
options(rcharts.cdn = TRUE, RCHART_WIDTH = 800, RCHART_HEIGHT = 400)
opts_chunk$set(tidy = F, results = "asis", comment = NA, fig.path = "fig/")
```




```r
library(rCharts)
L2 <- Leaflet$new()
L2$setView(c(29.7632836,  -95.3632715), 10)
L2$tileLayer(provider = "MapQuestOpen.OSM")
L2
```

<iframe src='
fig/unnamed-chunk-1.html
' scrolling='no' seamless
class='rChart leaflet '
id=iframe-
chart182a6b6bf663
></iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>



## Get Data


```r
data(crime, package = 'ggmap')
library(dplyr)
dat = crime %.% 
  group_by(lat, lon) %.% 
  summarise(count = n())
```

```
Error: This function should not be called directly
```

```r
dat2 = toJSONArray2(na.omit(dat), json = F, names = F)
```

```
Error: cannot open file
'/Users/ramnathv/Desktop/Tests/Jekyll/slidify-thinny/posts/leaflet-heat-maps/.cache/unnamed-chunk-2_c4657d43667f0845ff2766e3d7ebe9a1.rdb':
No such file or directory
```

```r
cat(rjson::toJSON(dat2[1:2]))
```

```
{"1960":{"AL":{"Year":1960,"State":"AL","Crime":186.6,"fillKey":"B"},"AK":{"Year":1960,"State":"AK","Crime":104.3,"fillKey":"A"},"AZ":{"Year":1960,"State":"AZ","Crime":207.7,"fillKey":"B"},"AR":{"Year":1960,"State":"AR","Crime":107.7,"fillKey":"A"},"CA":{"Year":1960,"State":"CA","Crime":239,"fillKey":"B"},"CO":{"Year":1960,"State":"CO","Crime":137.3,"fillKey":"A"},"CT":{"Year":1960,"State":"CT","Crime":36.6,"fillKey":"A"},"DE":{"Year":1960,"State":"DE","Crime":84,"fillKey":"A"},"FL":{"Year":1960,"State":"FL","Crime":223.4,"fillKey":"B"},"GA":{"Year":1960,"State":"GA","Crime":158.8,"fillKey":"A"},"HI":{"Year":1960,"State":"HI","Crime":21.8,"fillKey":"A"},"ID":{"Year":1960,"State":"ID","Crime":38.2,"fillKey":"A"},"IL":{"Year":1960,"State":"IL","Crime":365.1,"fillKey":"C"},"IN":{"Year":1960,"State":"IN","Crime":84.6,"fillKey":"A"},"IA":{"Year":1960,"State":"IA","Crime":23.8,"fillKey":"A"},"KS":{"Year":1960,"State":"KS","Crime":58.4,"fillKey":"A"},"KY":{"Year":1960,"State":"KY","Crime":97.3,"fillKey":"A"},"LA":{"Year":1960,"State":"LA","Crime":153.2,"fillKey":"A"},"ME":{"Year":1960,"State":"ME","Crime":29.8,"fillKey":"A"},"MD":{"Year":1960,"State":"MD","Crime":151.3,"fillKey":"A"},"MA":{"Year":1960,"State":"MA","Crime":48.8,"fillKey":"A"},"MI":{"Year":1960,"State":"MI","Crime":217.7,"fillKey":"B"},"MN":{"Year":1960,"State":"MN","Crime":42,"fillKey":"A"},"MS":{"Year":1960,"State":"MS","Crime":102.7,"fillKey":"A"},"MO":{"Year":1960,"State":"MO","Crime":172.9,"fillKey":"B"},"MT":{"Year":1960,"State":"MT","Crime":67.1,"fillKey":"A"},"NE":{"Year":1960,"State":"NE","Crime":41.8,"fillKey":"A"},"NV":{"Year":1960,"State":"NV","Crime":145.8,"fillKey":"A"},"NH":{"Year":1960,"State":"NH","Crime":13.3,"fillKey":"A"},"NJ":{"Year":1960,"State":"NJ","Crime":114.3,"fillKey":"A"},"NM":{"Year":1960,"State":"NM","Crime":143,"fillKey":"A"},"NC":{"Year":1960,"State":"NC","Crime":223.5,"fillKey":"B"},"ND":{"Year":1960,"State":"ND","Crime":14.2,"fillKey":"A"},"OH":{"Year":1960,"State":"OH","Crime":83.7,"fillKey":"A"},"OK":{"Year":1960,"State":"OK","Crime":97,"fillKey":"A"},"OR":{"Year":1960,"State":"OR","Crime":69.7,"fillKey":"A"},"PA":{"Year":1960,"State":"PA","Crime":99,"fillKey":"A"},"RI":{"Year":1960,"State":"RI","Crime":36.8,"fillKey":"A"},"SC":{"Year":1960,"State":"SC","Crime":143.7,"fillKey":"A"},"SD":{"Year":1960,"State":"SD","Crime":41.4,"fillKey":"A"},"TN":{"Year":1960,"State":"TN","Crime":91.1,"fillKey":"A"},"TX":{"Year":1960,"State":"TX","Crime":161,"fillKey":"B"},"UT":{"Year":1960,"State":"UT","Crime":54.3,"fillKey":"A"},"VA":{"Year":1960,"State":"VA","Crime":183.7,"fillKey":"B"},"WA":{"Year":1960,"State":"WA","Crime":56.6,"fillKey":"A"},"WV":{"Year":1960,"State":"WV","Crime":64.5,"fillKey":"A"},"WI":{"Year":1960,"State":"WI","Crime":31.9,"fillKey":"A"},"WY":{"Year":1960,"State":"WY","Crime":109.7,"fillKey":"A"}},"1961":{"AL":{"Year":1961,"State":"AL","Crime":168.5,"fillKey":"B"},"AK":{"Year":1961,"State":"AK","Crime":88.9,"fillKey":"A"},"AZ":{"Year":1961,"State":"AZ","Crime":164.5,"fillKey":"B"},"AR":{"Year":1961,"State":"AR","Crime":100.7,"fillKey":"A"},"CA":{"Year":1961,"State":"CA","Crime":232.7,"fillKey":"B"},"CO":{"Year":1961,"State":"CO","Crime":149.3,"fillKey":"A"},"CT":{"Year":1961,"State":"CT","Crime":33.6,"fillKey":"A"},"DE":{"Year":1961,"State":"DE","Crime":69.4,"fillKey":"A"},"FL":{"Year":1961,"State":"FL","Crime":217.8,"fillKey":"B"},"GA":{"Year":1961,"State":"GA","Crime":157.2,"fillKey":"A"},"HI":{"Year":1961,"State":"HI","Crime":24.5,"fillKey":"A"},"ID":{"Year":1961,"State":"ID","Crime":32.5,"fillKey":"A"},"IL":{"Year":1961,"State":"IL","Crime":362.2,"fillKey":"C"},"IN":{"Year":1961,"State":"IN","Crime":85.8,"fillKey":"A"},"IA":{"Year":1961,"State":"IA","Crime":23.1,"fillKey":"A"},"KS":{"Year":1961,"State":"KS","Crime":58.4,"fillKey":"A"},"KY":{"Year":1961,"State":"KY","Crime":94.5,"fillKey":"A"},"LA":{"Year":1961,"State":"LA","Crime":135.7,"fillKey":"A"},"ME":{"Year":1961,"State":"ME","Crime":33.7,"fillKey":"A"},"MD":{"Year":1961,"State":"MD","Crime":155.6,"fillKey":"A"},"MA":{"Year":1961,"State":"MA","Crime":53.1,"fillKey":"A"},"MI":{"Year":1961,"State":"MI","Crime":207.4,"fillKey":"B"},"MN":{"Year":1961,"State":"MN","Crime":43.4,"fillKey":"A"},"MS":{"Year":1961,"State":"MS","Crime":102.7,"fillKey":"A"},"MO":{"Year":1961,"State":"MO","Crime":166,"fillKey":"B"},"MT":{"Year":1961,"State":"MT","Crime":67,"fillKey":"A"},"NE":{"Year":1961,"State":"NE","Crime":41,"fillKey":"A"},"NV":{"Year":1961,"State":"NV","Crime":183.6,"fillKey":"B"},"NH":{"Year":1961,"State":"NH","Crime":14.3,"fillKey":"A"},"NJ":{"Year":1961,"State":"NJ","Crime":107,"fillKey":"A"},"NM":{"Year":1961,"State":"NM","Crime":130,"fillKey":"A"},"NC":{"Year":1961,"State":"NC","Crime":201,"fillKey":"B"},"ND":{"Year":1961,"State":"ND","Crime":25,"fillKey":"A"},"OH":{"Year":1961,"State":"OH","Crime":80.9,"fillKey":"A"},"OK":{"Year":1961,"State":"OK","Crime":103.3,"fillKey":"A"},"OR":{"Year":1961,"State":"OR","Crime":69,"fillKey":"A"},"PA":{"Year":1961,"State":"PA","Crime":97.9,"fillKey":"A"},"RI":{"Year":1961,"State":"RI","Crime":42.9,"fillKey":"A"},"SC":{"Year":1961,"State":"SC","Crime":137.8,"fillKey":"A"},"SD":{"Year":1961,"State":"SD","Crime":35.5,"fillKey":"A"},"TN":{"Year":1961,"State":"TN","Crime":108.2,"fillKey":"A"},"TX":{"Year":1961,"State":"TX","Crime":157.8,"fillKey":"A"},"UT":{"Year":1961,"State":"UT","Crime":49.2,"fillKey":"A"},"VT":{"Year":1961,"State":"VT","Crime":20,"fillKey":"A"},"VA":{"Year":1961,"State":"VA","Crime":186.1,"fillKey":"B"},"WA":{"Year":1961,"State":"WA","Crime":58.1,"fillKey":"A"},"WV":{"Year":1961,"State":"WV","Crime":63.1,"fillKey":"A"},"WI":{"Year":1961,"State":"WI","Crime":31.5,"fillKey":"A"},"WY":{"Year":1961,"State":"WY","Crime":85.2,"fillKey":"A"}}}
```


## Add HeatMap


```r
# Add leaflet-heat plugin. Thanks to Vladimir Agafonkin
L2$addAssets(jshead = c(
  "http://leaflet.github.io/Leaflet.heat/dist/leaflet-heat.js"
))

# Add javascript to modify underlying chart
L2$setTemplate(afterScript = sprintf("
<script>
  var addressPoints = %s
  var heat = L.heatLayer(addressPoints).addTo(map)           
</script>
", rjson::toJSON(na.omit(dat2))
))

L2
```

<iframe src='
fig/unnamed-chunk-3.html
' scrolling='no' seamless
class='rChart leaflet '
id=iframe-
chart182a6b6bf663
></iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>



<style>
  table.nofluid {width: auto; margin: 0 auto;}
  pre {margin-left: 0px;}
</style>




