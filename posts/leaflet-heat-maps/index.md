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
crime_dat = crime %.% 
  group_by(lat, lon) %.% 
  summarise(count = n())
```

```
Error: This function should not be called directly
```

```r
crime_dat = toJSONArray2(na.omit(crime_dat), json = F, names = F)
```

```
Error: object 'crime_dat' not found
```

```r
cat(rjson::toJSON(crime_dat[1:2]))
```

```
Error: object 'crime_dat' not found
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
", rjson::toJSON(crime_dat)
))
```

```
Error: object 'crime_dat' not found
```

```r

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




