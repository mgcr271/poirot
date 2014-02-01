---
title: Visualizing Bike Sharing Networks
author: Ramnath Vaidyanathan
class: clean
framework: thinny
highlighter: prettify
hitheme: twitter-bootstrap
mode: selfcontained
url: {lib: ../../libraries}
image: citibikes2.jpg
date: "2014-02-01"
---





## Step 1


```r
require(rCharts)
L1 <- Leaflet$new()
L1$tileLayer(provider = 'Stamen.TonerLite')
L1$setView(c(40.73029, -73.99076), 13)
L1$set(width = 1200)
L1
```

<iframe srcdoc='
&lt;!doctype HTML&gt;
&lt;meta charset = &#039;utf-8&#039;&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;link rel=&#039;stylesheet&#039; href=&#039;http://cdn.leafletjs.com/leaflet-0.5.1/leaflet.css&#039;&gt;
    
    &lt;script src=&#039;http://cdn.leafletjs.com/leaflet-0.5.1/leaflet.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    &lt;script src=&#039;https://rawgithub.com/leaflet-extras/leaflet-providers/gh-pages/leaflet-providers.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    &lt;script src=&#039;http://harrywood.co.uk/maps/examples/leaflet/leaflet-plugins/layer/vector/KML.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    
    &lt;style&gt;
    .rChart {
      display: block;
      margin-left: auto; 
      margin-right: auto;
      width: 1200px;
      height: 200px;
    }  
    &lt;/style&gt;
    
  &lt;/head&gt;
  &lt;body &gt;
    
    &lt;div id = &#039;chart136453dba1d00&#039; class = &#039;rChart leaflet&#039;&gt;&lt;/div&gt;    
    &lt;script&gt;
  var spec = {
 &quot;dom&quot;: &quot;chart136453dba1d00&quot;,
&quot;width&quot;:           1200,
&quot;height&quot;:            200,
&quot;urlTemplate&quot;: &quot;http://{s}.tile.osm.org/{z}/{x}/{y}.png&quot;,
&quot;layerOpts&quot;: {
 &quot;attribution&quot;: &quot;Map data&lt;a href=\&quot;http://openstreetmap.org\&quot;&gt;OpenStreetMap&lt;/a&gt;\n         contributors, Imagery&lt;a href=\&quot;http://mapbox.com\&quot;&gt;MapBox&lt;/a&gt;&quot; 
},
&quot;provider&quot;: &quot;Stamen.TonerLite&quot;,
&quot;center&quot;: [       40.73029,      -73.99076 ],
&quot;zoom&quot;:             13,
&quot;id&quot;: &quot;chart136453dba1d00&quot; 
}

  var map = L.map(spec.dom, spec.mapOpts)
  
    map.setView(spec.center, spec.zoom);

    if (spec.provider){
      L.tileLayer.provider(spec.provider).addTo(map)    
    } else {
		  L.tileLayer(spec.urlTemplate, spec.layerOpts).addTo(map)
    }
     
    
    
    
    
    
    if (spec.circle2){
      for (var c in spec.circle2){
        var circle = L.circle(c.center, c.radius, c.opts)
         .addTo(map);
      }
    }
    
    
    
    
    
   
   
   
&lt;/script&gt;
    
  &lt;/body&gt;
&lt;/html&gt;
' scrolling='no' seamless class='rChart 
leaflet
 '
id='iframe-chart136453dba1d00'>
</iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>


## Step 2


```r
network <- 'citibikenyc'
url = sprintf('http://api.citybik.es/%s.json', network)
bike = content(GET(url))
bike = lapply(bike, function(station){within(station, {
    latitude = as.numeric(lat)/10^6
    longitude = as.numeric(lng)/10^6
  })
})
```


## Step 3 | Inspect Data


```r
bike[[1]][c('name', 'latitude', 'longitude', 'bikes', 'free')]
```

```
$name
[1] "72 - W 52 St & 11 Ave"

$latitude
[1] 40.77

$longitude
[1] -73.99

$bikes
[1] 12

$free
[1] 27
```


## Step 4 | Add GeoJSON


```r
L1$geoJson(toGeoJSON(bike))
L1
```

<iframe srcdoc='
&lt;!doctype HTML&gt;
&lt;meta charset = &#039;utf-8&#039;&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;link rel=&#039;stylesheet&#039; href=&#039;http://cdn.leafletjs.com/leaflet-0.5.1/leaflet.css&#039;&gt;
    
    &lt;script src=&#039;http://cdn.leafletjs.com/leaflet-0.5.1/leaflet.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    &lt;script src=&#039;https://rawgithub.com/leaflet-extras/leaflet-providers/gh-pages/leaflet-providers.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    &lt;script src=&#039;http://harrywood.co.uk/maps/examples/leaflet/leaflet-plugins/layer/vector/KML.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    
    &lt;style&gt;
    .rChart {
      display: block;
      margin-left: auto; 
      margin-right: auto;
      width: 1200px;
      height: 200px;
    }  
    &lt;/style&gt;
    
  &lt;/head&gt;
  &lt;body &gt;
    
    &lt;div id = &#039;chart136453dba1d00&#039; class = &#039;rChart leaflet&#039;&gt;&lt;/div&gt;    
    &lt;script&gt;
  var spec = {
 &quot;dom&quot;: &quot;chart136453dba1d00&quot;,
&quot;width&quot;:           1200,
&quot;height&quot;:            200,
&quot;urlTemplate&quot;: &quot;http://{s}.tile.osm.org/{z}/{x}/{y}.png&quot;,
&quot;layerOpts&quot;: {
 &quot;attribution&quot;: &quot;Map data&lt;a href=\&quot;http://openstreetmap.org\&quot;&gt;OpenStreetMap&lt;/a&gt;\n         contributors, Imagery&lt;a href=\&quot;http://mapbox.com\&quot;&gt;MapBox&lt;/a&gt;&quot; 
},
&quot;provider&quot;: &quot;Stamen.TonerLite&quot;,
&quot;center&quot;: [       40.73029,      -73.99076 ],
&quot;zoom&quot;:             13,
&quot;id&quot;: &quot;chart136453dba1d00&quot;,
&quot;features&quot;: [
 {
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993928,      40.767272 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;72 - W 52 St &amp; 11 Ave&quot;,
&quot;idx&quot;:              0,
&quot;lat&quot;:       40767272,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.849Z&quot;,
&quot;lng&quot;:      -73993928,
&quot;id&quot;:              0,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006666,      40.719115 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;79 - Franklin St &amp; W Broadway&quot;,
&quot;idx&quot;:              1,
&quot;lat&quot;:       40719115,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.85Z&quot;,
&quot;lng&quot;:      -74006666,
&quot;id&quot;:              1,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.000165,      40.711174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;82 - St James Pl &amp; Pearl St&quot;,
&quot;idx&quot;:              2,
&quot;lat&quot;:       40711174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.851Z&quot;,
&quot;lng&quot;:      -74000165,
&quot;id&quot;:              2,
&quot;free&quot;:             18 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976323,      40.683826 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             33,
&quot;name&quot;: &quot;83 - Atlantic Ave &amp; Fort Greene Pl&quot;,
&quot;idx&quot;:              3,
&quot;lat&quot;:       40683826,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.851Z&quot;,
&quot;lng&quot;:      -73976323,
&quot;id&quot;:              3,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001497,      40.741776 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;116 - W 17 St &amp; 8 Ave&quot;,
&quot;idx&quot;:              4,
&quot;lat&quot;:       40741776,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.852Z&quot;,
&quot;lng&quot;:      -74001497,
&quot;id&quot;:              4,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978034,      40.696089 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;119 - Park Ave &amp; St Edwards St&quot;,
&quot;idx&quot;:              5,
&quot;lat&quot;:       40696089,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.853Z&quot;,
&quot;lng&quot;:      -73978034,
&quot;id&quot;:              5,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.959281,      40.686767 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;120 - Lexington Ave &amp; Classon Ave&quot;,
&quot;idx&quot;:              6,
&quot;lat&quot;:       40686767,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.853Z&quot;,
&quot;lng&quot;:      -73959281,
&quot;id&quot;:              6,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006744,      40.731724 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;127 - Barrow St &amp; Hudson St&quot;,
&quot;idx&quot;:              7,
&quot;lat&quot;:       40731724,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.854Z&quot;,
&quot;lng&quot;:      -74006744,
&quot;id&quot;:              7,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00297,      40.727102 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;128 - MacDougal St &amp; Prince St&quot;,
&quot;idx&quot;:              8,
&quot;lat&quot;:       40727102,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.855Z&quot;,
&quot;lng&quot;:      -74002970,
&quot;id&quot;:              8,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972924,      40.761628 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;137 - E 56 St &amp; Madison Ave&quot;,
&quot;idx&quot;:              9,
&quot;lat&quot;:       40761628,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.856Z&quot;,
&quot;lng&quot;:      -73972924,
&quot;id&quot;:              9,
&quot;free&quot;:             46 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993379,      40.692395 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;143 - Clinton St &amp; Joralemon St&quot;,
&quot;idx&quot;:             10,
&quot;lat&quot;:       40692395,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.856Z&quot;,
&quot;lng&quot;:      -73993379,
&quot;id&quot;:             10,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980689,      40.698398 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;144 - Nassau St &amp; Navy St&quot;,
&quot;idx&quot;:             11,
&quot;lat&quot;:       40698398,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.857Z&quot;,
&quot;lng&quot;:      -73980689,
&quot;id&quot;:             11,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009105,       40.71625 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;146 - Hudson St &amp; Reade St&quot;,
&quot;idx&quot;:             12,
&quot;lat&quot;:       40716250,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.858Z&quot;,
&quot;lng&quot;:      -74009105,
&quot;id&quot;:             12,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.011219,      40.715421 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;147 - Greenwich St &amp; Warren St&quot;,
&quot;idx&quot;:             13,
&quot;lat&quot;:       40715421,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.858Z&quot;,
&quot;lng&quot;:      -74011219,
&quot;id&quot;:             13,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980857,      40.720873 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;150 - E 2 St &amp; Avenue C&quot;,
&quot;idx&quot;:             14,
&quot;lat&quot;:       40720873,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.859Z&quot;,
&quot;lng&quot;:      -73980857,
&quot;id&quot;:             14,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997203,      40.721815 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;151 - Cleveland Pl &amp; Spring St&quot;,
&quot;idx&quot;:             15,
&quot;lat&quot;:       40721815,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.86Z&quot;,
&quot;lng&quot;:      -73997203,
&quot;id&quot;:             15,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009106,      40.714739 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;152 - Warren St &amp; Church St&quot;,
&quot;idx&quot;:             16,
&quot;lat&quot;:       40714739,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.86Z&quot;,
&quot;lng&quot;:      -74009106,
&quot;id&quot;:             16,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981632,      40.752062 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;153 - E 40 St &amp; 5 Ave&quot;,
&quot;idx&quot;:             17,
&quot;lat&quot;:       40752062,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.862Z&quot;,
&quot;lng&quot;:      -73981632,
&quot;id&quot;:             17,
&quot;free&quot;:             55 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996123,      40.690892 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;157 - Henry St &amp; Atlantic Ave&quot;,
&quot;idx&quot;:             18,
&quot;lat&quot;:       40690892,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.863Z&quot;,
&quot;lng&quot;:      -73996123,
&quot;id&quot;:             18,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978311,      40.748238 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;160 - E 37 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:             19,
&quot;lat&quot;:       40748238,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.864Z&quot;,
&quot;lng&quot;:      -73978311,
&quot;id&quot;:             19,
&quot;free&quot;:             18 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998102,       40.72917 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;161 - LaGuardia Pl &amp; W 3 St&quot;,
&quot;idx&quot;:             20,
&quot;lat&quot;:       40729170,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.865Z&quot;,
&quot;lng&quot;:      -73998102,
&quot;id&quot;:             20,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.970325,       40.75323 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;164 - E 47 St &amp; 2 Ave&quot;,
&quot;idx&quot;:             21,
&quot;lat&quot;:       40753230,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.865Z&quot;,
&quot;lng&quot;:      -73970325,
&quot;id&quot;:             21,
&quot;free&quot;:             43 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976048,        40.7489 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;167 - E 39 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             22,
&quot;lat&quot;:       40748900,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.866Z&quot;,
&quot;lng&quot;:      -73976048,
&quot;id&quot;:             22,
&quot;free&quot;:             42 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994564,      40.739713 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;168 - W 18 St &amp; 6 Ave&quot;,
&quot;idx&quot;:             23,
&quot;lat&quot;:       40739713,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.867Z&quot;,
&quot;lng&quot;:      -73994564,
&quot;id&quot;:             23,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984426,      40.760646 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;173 - Broadway &amp; W 49 St&quot;,
&quot;idx&quot;:             24,
&quot;lat&quot;:       40760646,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.867Z&quot;,
&quot;lng&quot;:      -73984426,
&quot;id&quot;:             24,
&quot;free&quot;:             41 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977386,      40.738176 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;174 - E 25 St &amp; 1 Ave&quot;,
&quot;idx&quot;:             25,
&quot;lat&quot;:       40738176,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.868Z&quot;,
&quot;lng&quot;:      -73977386,
&quot;id&quot;:             25,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010433,      40.709056 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;195 - Liberty St &amp; Broadway&quot;,
&quot;idx&quot;:             26,
&quot;lat&quot;:       40709056,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.869Z&quot;,
&quot;lng&quot;:      -74010433,
&quot;id&quot;:             26,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006817,      40.743349 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;212 - W 16 St &amp; The High Line&quot;,
&quot;idx&quot;:             27,
&quot;lat&quot;:       40743349,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.869Z&quot;,
&quot;lng&quot;:      -74006817,
&quot;id&quot;:             27,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99548,      40.700378 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;216 - Columbia Heights &amp; Cranberry St&quot;,
&quot;idx&quot;:             28,
&quot;lat&quot;:       40700378,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.87Z&quot;,
&quot;lng&quot;:      -73995480,
&quot;id&quot;:             28,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993836,      40.702771 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             37,
&quot;name&quot;: &quot;217 - Old Fulton St&quot;,
&quot;idx&quot;:             29,
&quot;lat&quot;:       40702771,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.871Z&quot;,
&quot;lng&quot;:      -73993836,
&quot;id&quot;:             29,
&quot;free&quot;:              2 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987071,      40.690284 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;218 - Gallatin Pl &amp; Livingston St&quot;,
&quot;idx&quot;:             30,
&quot;lat&quot;:       40690284,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.871Z&quot;,
&quot;lng&quot;:      -73987071,
&quot;id&quot;:             30,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999946,      40.737815 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;223 - W 13 St &amp; 7 Ave&quot;,
&quot;idx&quot;:             31,
&quot;lat&quot;:       40737815,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.872Z&quot;,
&quot;lng&quot;:      -73999946,
&quot;id&quot;:             31,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005524,      40.711463 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;224 - Spruce St &amp; Nassau St&quot;,
&quot;idx&quot;:             32,
&quot;lat&quot;:       40711463,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.873Z&quot;,
&quot;lng&quot;:      -74005524,
&quot;id&quot;:             32,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00803,      40.741951 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;225 - W 14 St &amp; The High Line&quot;,
&quot;idx&quot;:             33,
&quot;lat&quot;:       40741951,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.873Z&quot;,
&quot;lng&quot;:      -74008030,
&quot;id&quot;:             33,
&quot;free&quot;:             15 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971878,      40.754601 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;228 - E 48 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             34,
&quot;lat&quot;:       40754601,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.874Z&quot;,
&quot;lng&quot;:      -73971878,
&quot;id&quot;:             34,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99379,      40.727434 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;229 - Great Jones St&quot;,
&quot;idx&quot;:             35,
&quot;lat&quot;:       40727434,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.875Z&quot;,
&quot;lng&quot;:      -73993790,
&quot;id&quot;:             35,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990148,      40.695976 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;232 - Cadman Plaza E &amp; Tillary St&quot;,
&quot;idx&quot;:             36,
&quot;lat&quot;:       40695976,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.876Z&quot;,
&quot;lng&quot;:      -73990148,
&quot;id&quot;:             36,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989639,      40.692462 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;233 - Joralemon St &amp; Adams St&quot;,
&quot;idx&quot;:             37,
&quot;lat&quot;:       40692462,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.876Z&quot;,
&quot;lng&quot;:      -73989639,
&quot;id&quot;:             37,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987139,      40.728418 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;236 - St Marks Pl &amp; 2 Ave&quot;,
&quot;idx&quot;:             38,
&quot;lat&quot;:       40728418,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.877Z&quot;,
&quot;lng&quot;:      -73987139,
&quot;id&quot;:             38,
&quot;free&quot;:              9 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986723,      40.730473 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;237 - E 11 St &amp; 2 Ave&quot;,
&quot;idx&quot;:             39,
&quot;lat&quot;:       40730473,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.878Z&quot;,
&quot;lng&quot;:      -73986723,
&quot;id&quot;:             39,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008592,      40.736196 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;238 - Bank St &amp; Washington St&quot;,
&quot;idx&quot;:             40,
&quot;lat&quot;:       40736196,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.878Z&quot;,
&quot;lng&quot;:      -74008592,
&quot;id&quot;:             40,
&quot;free&quot;:             18 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981301,      40.691965 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;239 - Willoughby St &amp; Fleet St&quot;,
&quot;idx&quot;:             41,
&quot;lat&quot;:       40691965,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.879Z&quot;,
&quot;lng&quot;:      -73981301,
&quot;id&quot;:             41,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974931,       40.68981 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;241 - DeKalb Ave &amp; S Portland Ave&quot;,
&quot;idx&quot;:             42,
&quot;lat&quot;:       40689810,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.88Z&quot;,
&quot;lng&quot;:      -73974931,
&quot;id&quot;:             42,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973503,      40.697883 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;242 - Flushing Ave &amp; Carlton Ave&quot;,
&quot;idx&quot;:             43,
&quot;lat&quot;:       40697883,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.881Z&quot;,
&quot;lng&quot;:      -73973503,
&quot;id&quot;:             43,
&quot;free&quot;:              9 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979244,      40.688265 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;243 - Fulton St &amp; Rockwell Pl&quot;,
&quot;idx&quot;:             44,
&quot;lat&quot;:       40688265,
&quot;timestamp&quot;: &quot;2014-01-31T17:08:01.291Z&quot;,
&quot;lng&quot;:      -73979244,
&quot;id&quot;:             44,
&quot;free&quot;:             31 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965368,       40.69196 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;244 - Willoughby Ave &amp; Hall St&quot;,
&quot;idx&quot;:             45,
&quot;lat&quot;:       40691960,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.882Z&quot;,
&quot;lng&quot;:      -73965368,
&quot;id&quot;:             45,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977038,       40.69327 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;245 - Myrtle Ave &amp; St Edwards St&quot;,
&quot;idx&quot;:             46,
&quot;lat&quot;:       40693270,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.883Z&quot;,
&quot;lng&quot;:      -73977038,
&quot;id&quot;:             46,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00483,      40.735353 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;247 - Perry St &amp; Bleecker St&quot;,
&quot;idx&quot;:             47,
&quot;lat&quot;:       40735353,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.883Z&quot;,
&quot;lng&quot;:      -74004830,
&quot;id&quot;:             47,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007717,      40.721853 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;248 - Laight St &amp; Hudson St&quot;,
&quot;idx&quot;:             48,
&quot;lat&quot;:       40721853,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.884Z&quot;,
&quot;lng&quot;:      -74007717,
&quot;id&quot;:             48,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [        -74.009,      40.718709 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;249 - Harrison St &amp; Hudson St&quot;,
&quot;idx&quot;:             49,
&quot;lat&quot;:       40718709,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.885Z&quot;,
&quot;lng&quot;:      -74009000,
&quot;id&quot;:             49,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995652,       40.72456 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;250 - Lafayette St &amp; Jersey St&quot;,
&quot;idx&quot;:             50,
&quot;lat&quot;:       40724560,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.886Z&quot;,
&quot;lng&quot;:      -73995652,
&quot;id&quot;:             50,
&quot;free&quot;:             40 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9948,      40.723179 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;251 - Mott St &amp; Prince St&quot;,
&quot;idx&quot;:             51,
&quot;lat&quot;:       40723179,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.886Z&quot;,
&quot;lng&quot;:      -73994800,
&quot;id&quot;:             51,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998522,      40.732263 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;252 - MacDougal St &amp; Washington Sq&quot;,
&quot;idx&quot;:             52,
&quot;lat&quot;:       40732263,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.887Z&quot;,
&quot;lng&quot;:      -73998522,
&quot;id&quot;:             52,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994539,      40.735439 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;253 - W 13 St &amp; 5 Ave&quot;,
&quot;idx&quot;:             53,
&quot;lat&quot;:       40735439,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.888Z&quot;,
&quot;lng&quot;:      -73994539,
&quot;id&quot;:             53,
&quot;free&quot;:             47 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998004,      40.735324 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;254 - W 11 St &amp; 6 Ave&quot;,
&quot;idx&quot;:             54,
&quot;lat&quot;:       40735324,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.888Z&quot;,
&quot;lng&quot;:      -73998004,
&quot;id&quot;:             54,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002472,      40.719392 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;257 - Lispenard St &amp; Broadway&quot;,
&quot;idx&quot;:             55,
&quot;lat&quot;:       40719392,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.889Z&quot;,
&quot;lng&quot;:      -74002472,
&quot;id&quot;:             55,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.968854,      40.689407 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;258 - DeKalb Ave &amp; Vanderbilt Ave&quot;,
&quot;idx&quot;:             56,
&quot;lat&quot;:       40689407,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.89Z&quot;,
&quot;lng&quot;:      -73968854,
&quot;id&quot;:             56,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.012342,      40.701221 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;259 - South St &amp; Whitehall St&quot;,
&quot;idx&quot;:             57,
&quot;lat&quot;:       40701221,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.89Z&quot;,
&quot;lng&quot;:      -74012342,
&quot;id&quot;:             57,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.011677,      40.703651 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;260 - Broad St &amp; Bridge St&quot;,
&quot;idx&quot;:             58,
&quot;lat&quot;:       40703651,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.891Z&quot;,
&quot;lng&quot;:      -74011677,
&quot;id&quot;:             58,
&quot;free&quot;:             10 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983624,      40.694748 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;261 - Johnson St &amp; Gold St&quot;,
&quot;idx&quot;:             59,
&quot;lat&quot;:       40694748,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.892Z&quot;,
&quot;lng&quot;:      -73983624,
&quot;id&quot;:             59,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973729,      40.691782 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;262 - Washington Park&quot;,
&quot;idx&quot;:             60,
&quot;lat&quot;:       40691782,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.892Z&quot;,
&quot;lng&quot;:      -73973729,
&quot;id&quot;:             60,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996375,       40.71729 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;263 - Elizabeth St &amp; Hester St&quot;,
&quot;idx&quot;:             61,
&quot;lat&quot;:       40717290,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.893Z&quot;,
&quot;lng&quot;:      -73996375,
&quot;id&quot;:             61,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007318,      40.707064 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;264 - Maiden Ln &amp; Pearl St&quot;,
&quot;idx&quot;:             62,
&quot;lat&quot;:       40707064,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.894Z&quot;,
&quot;lng&quot;:      -74007318,
&quot;id&quot;:             62,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991475,      40.722293 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;265 - Stanton St &amp; Chrystie St&quot;,
&quot;idx&quot;:             63,
&quot;lat&quot;:       40722293,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.894Z&quot;,
&quot;lng&quot;:      -73991475,
&quot;id&quot;:             63,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.975748,      40.723683 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;266 - Avenue D &amp; E 8 St&quot;,
&quot;idx&quot;:             64,
&quot;lat&quot;:       40723683,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.895Z&quot;,
&quot;lng&quot;:      -73975748,
&quot;id&quot;:             64,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987654,      40.750977 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;267 - Broadway &amp; W 36 St&quot;,
&quot;idx&quot;:             65,
&quot;lat&quot;:       40750977,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.896Z&quot;,
&quot;lng&quot;:      -73987654,
&quot;id&quot;:             65,
&quot;free&quot;:             33 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999733,      40.719105 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;268 - Howard St &amp; Centre St&quot;,
&quot;idx&quot;:             66,
&quot;lat&quot;:       40719105,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.896Z&quot;,
&quot;lng&quot;:      -73999733,
&quot;id&quot;:             66,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971789,      40.693082 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;270 - Adelphi St &amp; Myrtle Ave&quot;,
&quot;idx&quot;:             67,
&quot;lat&quot;:       40693082,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.897Z&quot;,
&quot;lng&quot;:      -73971789,
&quot;id&quot;:             67,
&quot;free&quot;:              7 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978058,      40.685281 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;271 - Ashland Pl &amp; Hanson Pl&quot;,
&quot;idx&quot;:             68,
&quot;lat&quot;:       40685281,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.898Z&quot;,
&quot;lng&quot;:      -73978058,
&quot;id&quot;:             68,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976682,      40.686918 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;274 - Lafayette Ave &amp; Fort Greene Pl&quot;,
&quot;idx&quot;:             69,
&quot;lat&quot;:       40686918,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.898Z&quot;,
&quot;lng&quot;:      -73976682,
&quot;id&quot;:             69,
&quot;free&quot;:              1 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965633,        40.6865 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;275 - Washington Ave &amp; Greene Ave&quot;,
&quot;idx&quot;:             70,
&quot;lat&quot;:       40686500,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.899Z&quot;,
&quot;lng&quot;:      -73965633,
&quot;id&quot;:             70,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010455,      40.717487 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;276 - Duane St &amp; Greenwich St&quot;,
&quot;idx&quot;:             71,
&quot;lat&quot;:       40717487,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.9Z&quot;,
&quot;lng&quot;:      -74010455,
&quot;id&quot;:             71,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984764,      40.697665 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;278 - Concord St &amp; Bridge St&quot;,
&quot;idx&quot;:             72,
&quot;lat&quot;:       40697665,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.9Z&quot;,
&quot;lng&quot;:      -73984764,
&quot;id&quot;:             72,
&quot;free&quot;:             15 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982719,      40.699869 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;279 - Sands St &amp; Gold St&quot;,
&quot;idx&quot;:             73,
&quot;lat&quot;:       40699869,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.901Z&quot;,
&quot;lng&quot;:      -73982719,
&quot;id&quot;:             73,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995101,      40.733319 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;280 - E 10 St &amp; 5 Ave&quot;,
&quot;idx&quot;:             74,
&quot;lat&quot;:       40733319,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.902Z&quot;,
&quot;lng&quot;:      -73995101,
&quot;id&quot;:             74,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973714,      40.764397 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;281 - Grand Army Plaza &amp; Central Park S&quot;,
&quot;idx&quot;:             75,
&quot;lat&quot;:       40764397,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.903Z&quot;,
&quot;lng&quot;:      -73973714,
&quot;id&quot;:             75,
&quot;free&quot;:             58 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.968341,      40.708272 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;282 - Kent Ave &amp; S 11 St&quot;,
&quot;idx&quot;:             76,
&quot;lat&quot;:       40708272,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.904Z&quot;,
&quot;lng&quot;:      -73968341,
&quot;id&quot;:             76,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002637,      40.739016 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;284 - Greenwich Ave &amp; 8 Ave&quot;,
&quot;idx&quot;:             77,
&quot;lat&quot;:       40739016,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.905Z&quot;,
&quot;lng&quot;:      -74002637,
&quot;id&quot;:             77,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990741,      40.734545 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             34,
&quot;name&quot;: &quot;285 - Broadway &amp; E 14 St&quot;,
&quot;idx&quot;:             78,
&quot;lat&quot;:       40734545,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.905Z&quot;,
&quot;lng&quot;:      -73990741,
&quot;id&quot;:             78,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.95881,      40.684568 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;289 - Monroe St &amp; Classon Ave&quot;,
&quot;idx&quot;:             79,
&quot;lat&quot;:       40684568,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.906Z&quot;,
&quot;lng&quot;:      -73958810,
&quot;id&quot;:             79,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.964784,      40.760202 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;290 - 2 Ave &amp; E 58 St&quot;,
&quot;idx&quot;:             80,
&quot;lat&quot;:       40760202,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.907Z&quot;,
&quot;lng&quot;:      -73964784,
&quot;id&quot;:             80,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984844,      40.713126 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;291 - Madison St &amp; Montgomery St&quot;,
&quot;idx&quot;:             81,
&quot;lat&quot;:       40713126,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.907Z&quot;,
&quot;lng&quot;:      -73984844,
&quot;id&quot;:             81,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990764,      40.730286 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;293 - Lafayette St &amp; E 8 St&quot;,
&quot;idx&quot;:             82,
&quot;lat&quot;:       40730286,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.908Z&quot;,
&quot;lng&quot;:      -73990764,
&quot;id&quot;:             82,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995721,      40.730493 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;294 - Washington Square E&quot;,
&quot;idx&quot;:             83,
&quot;lat&quot;:       40730493,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.909Z&quot;,
&quot;lng&quot;:      -73995721,
&quot;id&quot;:             83,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992939,      40.714066 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;295 - Pike St &amp; E Broadway&quot;,
&quot;idx&quot;:             84,
&quot;lat&quot;:       40714066,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.909Z&quot;,
&quot;lng&quot;:      -73992939,
&quot;id&quot;:             84,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997046,       40.71413 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;296 - Division St &amp; Bowery&quot;,
&quot;idx&quot;:             85,
&quot;lat&quot;:       40714130,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.91Z&quot;,
&quot;lng&quot;:      -73997046,
&quot;id&quot;:             85,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986923,      40.734232 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;297 - E 15 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             86,
&quot;lat&quot;:       40734232,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.911Z&quot;,
&quot;lng&quot;:      -73986923,
&quot;id&quot;:             86,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979677,      40.686832 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;298 - 3 Ave &amp; Schermerhorn St&quot;,
&quot;idx&quot;:             87,
&quot;lat&quot;:       40686832,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.911Z&quot;,
&quot;lng&quot;:      -73979677,
&quot;id&quot;:             87,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990214,      40.728145 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;300 - Shevchenko Pl &amp; E 6 St&quot;,
&quot;idx&quot;:             88,
&quot;lat&quot;:       40728145,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.912Z&quot;,
&quot;lng&quot;:      -73990214,
&quot;id&quot;:             88,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983687,      40.722174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;301 - E 2 St &amp; Avenue B&quot;,
&quot;idx&quot;:             89,
&quot;lat&quot;:       40722174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.913Z&quot;,
&quot;lng&quot;:      -73983687,
&quot;id&quot;:             89,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977931,      40.720828 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;302 - Avenue D &amp; E 3 St&quot;,
&quot;idx&quot;:             90,
&quot;lat&quot;:       40720828,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.913Z&quot;,
&quot;lng&quot;:      -73977931,
&quot;id&quot;:             90,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999496,      40.723627 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;303 - Mercer St &amp; Spring St&quot;,
&quot;idx&quot;:             91,
&quot;lat&quot;:       40723627,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.914Z&quot;,
&quot;lng&quot;:      -73999496,
&quot;id&quot;:             91,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.013617,      40.704633 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;304 - Broadway &amp; Battery Pl&quot;,
&quot;idx&quot;:             92,
&quot;lat&quot;:       40704633,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.915Z&quot;,
&quot;lng&quot;:      -74013617,
&quot;id&quot;:             92,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967244,      40.760957 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;305 - E 58 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             93,
&quot;lat&quot;:       40760957,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.915Z&quot;,
&quot;lng&quot;:      -73967244,
&quot;id&quot;:             93,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -74.0053,      40.708235 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;306 - Cliff St &amp; Fulton St&quot;,
&quot;idx&quot;:             94,
&quot;lat&quot;:       40708235,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.916Z&quot;,
&quot;lng&quot;:      -74005300,
&quot;id&quot;:             94,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9899,      40.714274 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;307 - Canal St &amp; Rutgers St&quot;,
&quot;idx&quot;:             95,
&quot;lat&quot;:       40714274,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.917Z&quot;,
&quot;lng&quot;:      -73989900,
&quot;id&quot;:             95,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998511,      40.713079 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;308 - St James Pl &amp; Oliver St&quot;,
&quot;idx&quot;:             96,
&quot;lat&quot;:       40713079,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.917Z&quot;,
&quot;lng&quot;:      -73998511,
&quot;id&quot;:             96,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.013012,      40.714978 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;309 - Murray St &amp; West St&quot;,
&quot;idx&quot;:             97,
&quot;lat&quot;:       40714978,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.918Z&quot;,
&quot;lng&quot;:      -74013012,
&quot;id&quot;:             97,
&quot;free&quot;:             40 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989128,      40.689269 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;310 - State St &amp; Smith St&quot;,
&quot;idx&quot;:             98,
&quot;lat&quot;:       40689269,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.919Z&quot;,
&quot;lng&quot;:      -73989128,
&quot;id&quot;:             98,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98802,      40.717227 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;311 - Norfolk St &amp; Broome St&quot;,
&quot;idx&quot;:             99,
&quot;lat&quot;:       40717227,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.919Z&quot;,
&quot;lng&quot;:      -73988020,
&quot;id&quot;:             99,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989111,      40.722055 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;312 - Allen St &amp; E Houston St&quot;,
&quot;idx&quot;:            100,
&quot;lat&quot;:       40722055,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.92Z&quot;,
&quot;lng&quot;:      -73989111,
&quot;id&quot;:            100,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.96751,      40.696102 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;313 - Washington Ave &amp; Park Ave&quot;,
&quot;idx&quot;:            101,
&quot;lat&quot;:       40696102,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.921Z&quot;,
&quot;lng&quot;:      -73967510,
&quot;id&quot;:            101,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992159,      40.694246 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;314 - Montague St &amp; Clinton St&quot;,
&quot;idx&quot;:            102,
&quot;lat&quot;:       40694246,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.921Z&quot;,
&quot;lng&quot;:      -73992159,
&quot;id&quot;:            102,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006702,      40.703553 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             27,
&quot;name&quot;: &quot;315 - South St &amp; Gouverneur Ln&quot;,
&quot;idx&quot;:            103,
&quot;lat&quot;:       40703553,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.922Z&quot;,
&quot;lng&quot;:      -74006702,
&quot;id&quot;:            103,
&quot;free&quot;:              2 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006536,      40.709559 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;316 - Fulton St &amp; William St&quot;,
&quot;idx&quot;:            104,
&quot;lat&quot;:       40709559,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.923Z&quot;,
&quot;lng&quot;:      -74006536,
&quot;id&quot;:            104,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981854,      40.724537 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;317 - E 6 St &amp; Avenue B&quot;,
&quot;idx&quot;:            105,
&quot;lat&quot;:       40724537,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.924Z&quot;,
&quot;lng&quot;:      -73981854,
&quot;id&quot;:            105,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977987,      40.753201 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;318 - E 43 St &amp; Vanderbilt Ave&quot;,
&quot;idx&quot;:            106,
&quot;lat&quot;:       40753201,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.924Z&quot;,
&quot;lng&quot;:      -73977987,
&quot;id&quot;:            106,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009376,      40.713361 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;319 - Park Pl &amp; Church St&quot;,
&quot;idx&quot;:            107,
&quot;lat&quot;:       40713361,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.925Z&quot;,
&quot;lng&quot;:      -74009376,
&quot;id&quot;:            107,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005834,      40.717439 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;320 - Church St &amp; Leonard St&quot;,
&quot;idx&quot;:            108,
&quot;lat&quot;:       40717439,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.926Z&quot;,
&quot;lng&quot;:      -74005834,
&quot;id&quot;:            108,
&quot;free&quot;:             39 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989717,      40.699917 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;321 - Cadman Plaza E &amp; Red Cross Pl&quot;,
&quot;idx&quot;:            109,
&quot;lat&quot;:       40699917,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.926Z&quot;,
&quot;lng&quot;:      -73989717,
&quot;id&quot;:            109,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991218,      40.696192 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;322 - Clinton St &amp; Tillary St&quot;,
&quot;idx&quot;:            110,
&quot;lat&quot;:       40696192,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.927Z&quot;,
&quot;lng&quot;:      -73991218,
&quot;id&quot;:            110,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986317,      40.692361 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;323 - Lawrence St &amp; Willoughby St&quot;,
&quot;idx&quot;:            111,
&quot;lat&quot;:       40692361,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.928Z&quot;,
&quot;lng&quot;:      -73986317,
&quot;id&quot;:            111,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981013,      40.689888 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;324 - DeKalb Ave &amp; Hudson Ave&quot;,
&quot;idx&quot;:            112,
&quot;lat&quot;:       40689888,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.928Z&quot;,
&quot;lng&quot;:      -73981013,
&quot;id&quot;:            112,
&quot;free&quot;:             39 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984737,      40.736245 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;325 - E 19 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            113,
&quot;lat&quot;:       40736245,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.929Z&quot;,
&quot;lng&quot;:      -73984737,
&quot;id&quot;:            113,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984267,      40.729538 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;326 - E 11 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            114,
&quot;lat&quot;:       40729538,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.93Z&quot;,
&quot;lng&quot;:      -73984267,
&quot;id&quot;:            114,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.016583,      40.715337 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;327 - Vesey Pl &amp; River Terrace&quot;,
&quot;idx&quot;:            115,
&quot;lat&quot;:       40715337,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.93Z&quot;,
&quot;lng&quot;:      -74016583,
&quot;id&quot;:            115,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009659,      40.724055 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;328 - Watts St &amp; Greenwich St&quot;,
&quot;idx&quot;:            116,
&quot;lat&quot;:       40724055,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.931Z&quot;,
&quot;lng&quot;:      -74009659,
&quot;id&quot;:            116,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010206,      40.720434 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;329 - Greenwich St &amp; N Moore St&quot;,
&quot;idx&quot;:            117,
&quot;lat&quot;:       40720434,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.932Z&quot;,
&quot;lng&quot;:      -74010206,
&quot;id&quot;:            117,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005627,      40.714504 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;330 - Reade St &amp; Broadway&quot;,
&quot;idx&quot;:            118,
&quot;lat&quot;:       40714504,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.932Z&quot;,
&quot;lng&quot;:      -74005627,
&quot;id&quot;:            118,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99193,      40.711731 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;331 - Pike St &amp; Monroe St&quot;,
&quot;idx&quot;:            119,
&quot;lat&quot;:       40711731,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.933Z&quot;,
&quot;lng&quot;:      -73991930,
&quot;id&quot;:            119,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979481,      40.712199 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;332 - Cherry St&quot;,
&quot;idx&quot;:            120,
&quot;lat&quot;:       40712199,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.934Z&quot;,
&quot;lng&quot;:      -73979481,
&quot;id&quot;:            120,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997262,      40.742387 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;334 - W 20 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            121,
&quot;lat&quot;:       40742387,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.935Z&quot;,
&quot;lng&quot;:      -73997262,
&quot;id&quot;:            121,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994046,      40.729039 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;335 - Washington Pl &amp; Broadway&quot;,
&quot;idx&quot;:            122,
&quot;lat&quot;:       40729039,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.936Z&quot;,
&quot;lng&quot;:      -73994046,
&quot;id&quot;:            122,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008386,      40.703799 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;337 - Old Slip &amp; Front St&quot;,
&quot;idx&quot;:            123,
&quot;lat&quot;:       40703799,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.937Z&quot;,
&quot;lng&quot;:      -74008386,
&quot;id&quot;:            123,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974224,      40.725806 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;339 - Avenue D &amp; E 12 St&quot;,
&quot;idx&quot;:            124,
&quot;lat&quot;:       40725806,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.938Z&quot;,
&quot;lng&quot;:      -73974224,
&quot;id&quot;:            124,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987763,       40.71269 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;340 - Madison St &amp; Clinton St&quot;,
&quot;idx&quot;:            125,
&quot;lat&quot;:       40712690,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.938Z&quot;,
&quot;lng&quot;:      -73987763,
&quot;id&quot;:            125,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976289,      40.717821 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;341 - Stanton St &amp; Mangin St&quot;,
&quot;idx&quot;:            126,
&quot;lat&quot;:       40717821,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.939Z&quot;,
&quot;lng&quot;:      -73976289,
&quot;id&quot;:            126,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980165,      40.717399 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;342 - Columbia St &amp; Rivington St&quot;,
&quot;idx&quot;:            127,
&quot;lat&quot;:       40717399,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.94Z&quot;,
&quot;lng&quot;:      -73980165,
&quot;id&quot;:            127,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969868,       40.69794 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;343 - Clinton Ave &amp; Flushing Ave&quot;,
&quot;idx&quot;:            128,
&quot;lat&quot;:       40697940,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.94Z&quot;,
&quot;lng&quot;:      -73969868,
&quot;id&quot;:            128,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.953809,      40.685144 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;344 - Monroe St &amp; Bedford Ave&quot;,
&quot;idx&quot;:            129,
&quot;lat&quot;:       40685144,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.941Z&quot;,
&quot;lng&quot;:      -73953809,
&quot;id&quot;:            129,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997043,      40.736494 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;345 - W 13 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            130,
&quot;lat&quot;:       40736494,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.942Z&quot;,
&quot;lng&quot;:      -73997043,
&quot;id&quot;:            130,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00618,      40.736528 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;346 - Bank St &amp; Hudson St&quot;,
&quot;idx&quot;:            131,
&quot;lat&quot;:       40736528,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.942Z&quot;,
&quot;lng&quot;:      -74006180,
&quot;id&quot;:            131,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007488,      40.728738 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;347 - W Houston St &amp; Hudson St&quot;,
&quot;idx&quot;:            132,
&quot;lat&quot;:       40728738,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.944Z&quot;,
&quot;lng&quot;:      -74007488,
&quot;id&quot;:            132,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001547,      40.724909 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             36,
&quot;name&quot;: &quot;348 - W Broadway &amp; Spring St&quot;,
&quot;idx&quot;:            133,
&quot;lat&quot;:       40724909,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.945Z&quot;,
&quot;lng&quot;:      -74001547,
&quot;id&quot;:            133,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983298,      40.718502 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;349 - Rivington St &amp; Ridge St&quot;,
&quot;idx&quot;:            134,
&quot;lat&quot;:       40718502,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.946Z&quot;,
&quot;lng&quot;:      -73983298,
&quot;id&quot;:            134,
&quot;free&quot;:             18 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987029,      40.715595 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;350 - Clinton St &amp; Grand St&quot;,
&quot;idx&quot;:            135,
&quot;lat&quot;:       40715595,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.946Z&quot;,
&quot;lng&quot;:      -73987029,
&quot;id&quot;:            135,
&quot;free&quot;:             10 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006125,      40.705309 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;351 - Front St &amp; Maiden Ln&quot;,
&quot;idx&quot;:            136,
&quot;lat&quot;:       40705309,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.947Z&quot;,
&quot;lng&quot;:      -74006125,
&quot;id&quot;:            136,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977224,      40.763406 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;352 - W 56 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            137,
&quot;lat&quot;:       40763406,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.948Z&quot;,
&quot;lng&quot;:      -73977224,
&quot;id&quot;:            137,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974314,      40.685395 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;353 - S Portland Ave &amp; Hanson Pl&quot;,
&quot;idx&quot;:            138,
&quot;lat&quot;:       40685395,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.948Z&quot;,
&quot;lng&quot;:      -73974314,
&quot;id&quot;:            138,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.962235,      40.693631 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;354 - Emerson Pl &amp; Myrtle Ave&quot;,
&quot;idx&quot;:            139,
&quot;lat&quot;:       40693631,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.949Z&quot;,
&quot;lng&quot;:      -73962235,
&quot;id&quot;:            139,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999743,      40.716021 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             33,
&quot;name&quot;: &quot;355 - Bayard St &amp; Baxter St&quot;,
&quot;idx&quot;:            140,
&quot;lat&quot;:       40716021,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.95Z&quot;,
&quot;lng&quot;:      -73999743,
&quot;id&quot;:            140,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982612,      40.716226 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;356 - Bialystoker Pl &amp; Delancey St&quot;,
&quot;idx&quot;:            141,
&quot;lat&quot;:       40716226,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.95Z&quot;,
&quot;lng&quot;:      -73982612,
&quot;id&quot;:            141,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99158,      40.732617 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;357 - E 11 St &amp; Broadway&quot;,
&quot;idx&quot;:            142,
&quot;lat&quot;:       40732617,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.951Z&quot;,
&quot;lng&quot;:      -73991580,
&quot;id&quot;:            142,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007113,      40.732915 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             35,
&quot;name&quot;: &quot;358 - Christopher St &amp; Greenwich St&quot;,
&quot;idx&quot;:            143,
&quot;lat&quot;:       40732915,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.952Z&quot;,
&quot;lng&quot;:      -74007113,
&quot;id&quot;:            143,
&quot;free&quot;:              1 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974986,      40.755102 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;359 - E 47 St &amp; Park Av&quot;,
&quot;idx&quot;:            144,
&quot;lat&quot;:       40755102,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.952Z&quot;,
&quot;lng&quot;:      -73974986,
&quot;id&quot;:            144,
&quot;free&quot;:             53 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008873,      40.707179 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;360 - William St &amp; Pine St&quot;,
&quot;idx&quot;:            145,
&quot;lat&quot;:       40707179,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.953Z&quot;,
&quot;lng&quot;:      -74008873,
&quot;id&quot;:            145,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991907,      40.716058 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;361 - Allen St &amp; Hester St&quot;,
&quot;idx&quot;:            146,
&quot;lat&quot;:       40716058,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.954Z&quot;,
&quot;lng&quot;:      -73991907,
&quot;id&quot;:            146,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987535,      40.751726 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;362 - Broadway &amp; W 37 St&quot;,
&quot;idx&quot;:            147,
&quot;lat&quot;:       40751726,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.954Z&quot;,
&quot;lng&quot;:      -73987535,
&quot;id&quot;:            147,
&quot;free&quot;:             57 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.017134,      40.708346 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;363 - West Thames St&quot;,
&quot;idx&quot;:            148,
&quot;lat&quot;:       40708346,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.955Z&quot;,
&quot;lng&quot;:      -74017134,
&quot;id&quot;:            148,
&quot;free&quot;:             34 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.960238,      40.689004 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;364 - Lafayette Ave &amp; Classon Ave&quot;,
&quot;idx&quot;:            149,
&quot;lat&quot;:       40689004,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.956Z&quot;,
&quot;lng&quot;:      -73960238,
&quot;id&quot;:            149,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.961458,      40.682231 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;365 - Fulton St &amp; Grand Ave&quot;,
&quot;idx&quot;:            150,
&quot;lat&quot;:       40682231,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.956Z&quot;,
&quot;lng&quot;:      -73961458,
&quot;id&quot;:            150,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.968896,      40.693261 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;366 - Clinton Ave &amp; Myrtle Ave&quot;,
&quot;idx&quot;:            151,
&quot;lat&quot;:       40693261,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.957Z&quot;,
&quot;lng&quot;:      -73968896,
&quot;id&quot;:            151,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.970694,       40.75828 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;367 - E 53 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:            152,
&quot;lat&quot;:       40758280,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.958Z&quot;,
&quot;lng&quot;:      -73970694,
&quot;id&quot;:            152,
&quot;free&quot;:             33 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002149,      40.730385 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;368 - Carmine St &amp; 6 Ave&quot;,
&quot;idx&quot;:            153,
&quot;lat&quot;:       40730385,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.958Z&quot;,
&quot;lng&quot;:      -74002149,
&quot;id&quot;:            153,
&quot;free&quot;:             31 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.000263,      40.732241 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;369 - Washington Pl &amp; 6 Ave&quot;,
&quot;idx&quot;:            154,
&quot;lat&quot;:       40732241,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.959Z&quot;,
&quot;lng&quot;:      -74000263,
&quot;id&quot;:            154,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.958089,      40.694528 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;372 - Franklin Ave &amp; Myrtle Ave&quot;,
&quot;idx&quot;:            155,
&quot;lat&quot;:       40694528,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.96Z&quot;,
&quot;lng&quot;:      -73958089,
&quot;id&quot;:            155,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.953819,      40.693317 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;373 - Willoughby Ave &amp; Walworth St&quot;,
&quot;idx&quot;:            156,
&quot;lat&quot;:       40693317,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.96Z&quot;,
&quot;lng&quot;:      -73953819,
&quot;id&quot;:            156,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99695,      40.726794 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;375 - Mercer St &amp; Bleecker St&quot;,
&quot;idx&quot;:            157,
&quot;lat&quot;:       40726794,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.961Z&quot;,
&quot;lng&quot;:      -73996950,
&quot;id&quot;:            157,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007221,      40.708621 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;376 - John St &amp; William St&quot;,
&quot;idx&quot;:            158,
&quot;lat&quot;:       40708621,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.962Z&quot;,
&quot;lng&quot;:      -74007221,
&quot;id&quot;:            158,
&quot;free&quot;:             41 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005664,      40.722437 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;377 - 6 Ave &amp; Canal St&quot;,
&quot;idx&quot;:            159,
&quot;lat&quot;:       40722437,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.962Z&quot;,
&quot;lng&quot;:      -74005664,
&quot;id&quot;:            159,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9916,      40.749156 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             35,
&quot;name&quot;: &quot;379 - W 31 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            160,
&quot;lat&quot;:       40749156,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.963Z&quot;,
&quot;lng&quot;:      -73991600,
&quot;id&quot;:            160,
&quot;free&quot;:              2 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002938,      40.734011 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;380 - W 4 St &amp; 7 Ave S&quot;,
&quot;idx&quot;:            161,
&quot;lat&quot;:       40734011,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.964Z&quot;,
&quot;lng&quot;:      -74002938,
&quot;id&quot;:            161,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992005,      40.734926 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;382 - University Pl &amp; E 14 St&quot;,
&quot;idx&quot;:            162,
&quot;lat&quot;:       40734926,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.964Z&quot;,
&quot;lng&quot;:      -73992005,
&quot;id&quot;:            162,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.000271,      40.735238 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             34,
&quot;name&quot;: &quot;383 - Greenwich Ave &amp; Charles St&quot;,
&quot;idx&quot;:            163,
&quot;lat&quot;:       40735238,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.965Z&quot;,
&quot;lng&quot;:      -74000271,
&quot;id&quot;:            163,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965964,      40.683178 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;384 - Fulton St &amp; Waverly Ave&quot;,
&quot;idx&quot;:            164,
&quot;lat&quot;:       40683178,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.966Z&quot;,
&quot;lng&quot;:      -73965964,
&quot;id&quot;:            164,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.966033,      40.757973 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;385 - E 55 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            165,
&quot;lat&quot;:       40757973,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.966Z&quot;,
&quot;lng&quot;:      -73966033,
&quot;id&quot;:            165,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002344,      40.714948 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;386 - Centre St &amp; Worth St&quot;,
&quot;idx&quot;:            166,
&quot;lat&quot;:       40714948,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.967Z&quot;,
&quot;lng&quot;:      -74002344,
&quot;id&quot;:            166,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004607,      40.712732 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;387 - Centre St &amp; Chambers St&quot;,
&quot;idx&quot;:            167,
&quot;lat&quot;:       40712732,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.968Z&quot;,
&quot;lng&quot;:      -74004607,
&quot;id&quot;:            167,
&quot;free&quot;:             37 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00295,      40.749717 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;388 - W 26 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            168,
&quot;lat&quot;:       40749717,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.968Z&quot;,
&quot;lng&quot;:      -74002950,
&quot;id&quot;:            168,
&quot;free&quot;:             34 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.96525,      40.710445 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;389 - Broadway &amp; Berry St&quot;,
&quot;idx&quot;:            169,
&quot;lat&quot;:       40710445,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.969Z&quot;,
&quot;lng&quot;:      -73965250,
&quot;id&quot;:            169,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984284,      40.692215 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;390 - Duffield St &amp; Willoughby St&quot;,
&quot;idx&quot;:            170,
&quot;lat&quot;:       40692215,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.97Z&quot;,
&quot;lng&quot;:      -73984284,
&quot;id&quot;:            170,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993445,      40.697601 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;391 - Clark St &amp; Henry St&quot;,
&quot;idx&quot;:            171,
&quot;lat&quot;:       40697601,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.97Z&quot;,
&quot;lng&quot;:      -73993445,
&quot;id&quot;:            171,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987167,      40.695065 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;392 - Jay St &amp; Tech Pl&quot;,
&quot;idx&quot;:            172,
&quot;lat&quot;:       40695065,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.971Z&quot;,
&quot;lng&quot;:      -73987167,
&quot;id&quot;:            172,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979954,      40.722992 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;393 - E 5 St &amp; Avenue C&quot;,
&quot;idx&quot;:            173,
&quot;lat&quot;:       40722992,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.972Z&quot;,
&quot;lng&quot;:      -73979954,
&quot;id&quot;:            173,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977687,      40.725213 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;394 - E 9 St &amp; Avenue C&quot;,
&quot;idx&quot;:            174,
&quot;lat&quot;:       40725213,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.972Z&quot;,
&quot;lng&quot;:      -73977687,
&quot;id&quot;:            174,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984106,       40.68807 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;395 - Bond St &amp; Schermerhorn St&quot;,
&quot;idx&quot;:            175,
&quot;lat&quot;:       40688070,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.973Z&quot;,
&quot;lng&quot;:      -73984106,
&quot;id&quot;:            175,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.955768,      40.680342 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;396 - Lefferts Pl &amp; Franklin Ave&quot;,
&quot;idx&quot;:            176,
&quot;lat&quot;:       40680342,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.974Z&quot;,
&quot;lng&quot;:      -73955768,
&quot;id&quot;:            176,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969222,      40.684157 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;397 - Fulton St &amp; Clermont Ave&quot;,
&quot;idx&quot;:            177,
&quot;lat&quot;:       40684157,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.975Z&quot;,
&quot;lng&quot;:      -73969222,
&quot;id&quot;:            177,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999978,      40.691651 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;398 - Atlantic Ave &amp; Furman St&quot;,
&quot;idx&quot;:            178,
&quot;lat&quot;:       40691651,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.975Z&quot;,
&quot;lng&quot;:      -73999978,
&quot;id&quot;:            178,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.964762,      40.688515 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;399 - Lafayette Ave &amp; St James Pl&quot;,
&quot;idx&quot;:            179,
&quot;lat&quot;:       40688515,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.978Z&quot;,
&quot;lng&quot;:      -73964762,
&quot;id&quot;:            179,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98178,       40.71926 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;400 - Pitt St &amp; Stanton St&quot;,
&quot;idx&quot;:            180,
&quot;lat&quot;:       40719260,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.979Z&quot;,
&quot;lng&quot;:      -73981780,
&quot;id&quot;:            180,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989978,      40.720195 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;401 - Allen St &amp; Rivington St&quot;,
&quot;idx&quot;:            181,
&quot;lat&quot;:       40720195,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.98Z&quot;,
&quot;lng&quot;:      -73989978,
&quot;id&quot;:            181,
&quot;free&quot;:             15 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989551,      40.740343 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;402 - Broadway &amp; E 22 St&quot;,
&quot;idx&quot;:            182,
&quot;lat&quot;:       40740343,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.981Z&quot;,
&quot;lng&quot;:      -73989551,
&quot;id&quot;:            182,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990696,      40.725028 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;403 - E 2 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            183,
&quot;lat&quot;:       40725028,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.981Z&quot;,
&quot;lng&quot;:      -73990696,
&quot;id&quot;:            183,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005508,      40.740582 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;404 - 9 Ave &amp; W 14 St&quot;,
&quot;idx&quot;:            184,
&quot;lat&quot;:       40740582,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.982Z&quot;,
&quot;lng&quot;:      -74005508,
&quot;id&quot;:            184,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008119,      40.739323 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;405 - Washington St &amp; Gansevoort St&quot;,
&quot;idx&quot;:            185,
&quot;lat&quot;:       40739323,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.983Z&quot;,
&quot;lng&quot;:      -74008119,
&quot;id&quot;:            185,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99595,      40.695128 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;406 - Hicks St &amp; Montague St&quot;,
&quot;idx&quot;:            186,
&quot;lat&quot;:       40695128,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.983Z&quot;,
&quot;lng&quot;:      -73995950,
&quot;id&quot;:            186,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991454,      40.700469 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;407 - Henry St &amp; Poplar St&quot;,
&quot;idx&quot;:            187,
&quot;lat&quot;:       40700469,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.984Z&quot;,
&quot;lng&quot;:      -73991454,
&quot;id&quot;:            187,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994003,      40.710762 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;408 - Market St &amp; Cherry St&quot;,
&quot;idx&quot;:            188,
&quot;lat&quot;:       40710762,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.985Z&quot;,
&quot;lng&quot;:      -73994003,
&quot;id&quot;:            188,
&quot;free&quot;:             15 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.956431,      40.690649 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;409 - DeKalb Ave &amp; Skillman St&quot;,
&quot;idx&quot;:            189,
&quot;lat&quot;:       40690649,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.986Z&quot;,
&quot;lng&quot;:      -73956431,
&quot;id&quot;:            189,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.985179,      40.720664 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;410 - Suffolk St &amp; Stanton St&quot;,
&quot;idx&quot;:            190,
&quot;lat&quot;:       40720664,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.987Z&quot;,
&quot;lng&quot;:      -73985179,
&quot;id&quot;:            190,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976687,       40.72228 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;411 - E 6 St &amp; Avenue D&quot;,
&quot;idx&quot;:            191,
&quot;lat&quot;:       40722280,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.987Z&quot;,
&quot;lng&quot;:      -73976687,
&quot;id&quot;:            191,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994223,      40.715815 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;412 - Forsyth St &amp; Canal St&quot;,
&quot;idx&quot;:            192,
&quot;lat&quot;:       40715815,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.999Z&quot;,
&quot;lng&quot;:      -73994223,
&quot;id&quot;:            192,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987657,      40.702818 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;414 - Pearl St &amp; Anchorage Pl&quot;,
&quot;idx&quot;:            193,
&quot;lat&quot;:       40702818,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07Z&quot;,
&quot;lng&quot;:      -73987657,
&quot;id&quot;:            193,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00926,      40.704717 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;415 - Pearl St &amp; Hanover Square&quot;,
&quot;idx&quot;:            194,
&quot;lat&quot;:       40704717,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07Z&quot;,
&quot;lng&quot;:      -74009260,
&quot;id&quot;:            194,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972651,      40.687534 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;416 - Cumberland St &amp; Lafayette Ave&quot;,
&quot;idx&quot;:            195,
&quot;lat&quot;:       40687534,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.001Z&quot;,
&quot;lng&quot;:      -73972651,
&quot;id&quot;:            195,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010202,      40.712912 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;417 - Barclay St &amp; Church St&quot;,
&quot;idx&quot;:            196,
&quot;lat&quot;:       40712912,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.002Z&quot;,
&quot;lng&quot;:      -74010202,
&quot;id&quot;:            196,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982578,       40.70224 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;418 - Front St &amp; Gold St&quot;,
&quot;idx&quot;:            197,
&quot;lat&quot;:       40702240,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.002Z&quot;,
&quot;lng&quot;:      -73982578,
&quot;id&quot;:            197,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973555,      40.695807 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;419 - Carlton Ave &amp; Park Ave&quot;,
&quot;idx&quot;:            198,
&quot;lat&quot;:       40695807,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.003Z&quot;,
&quot;lng&quot;:      -73973555,
&quot;id&quot;:            198,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969689,      40.687644 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;420 - Clermont Ave &amp; Lafayette Ave&quot;,
&quot;idx&quot;:            199,
&quot;lat&quot;:       40687644,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.004Z&quot;,
&quot;lng&quot;:      -73969689,
&quot;id&quot;:            199,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971296,      40.695733 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;421 - Clermont Ave &amp; Park Ave&quot;,
&quot;idx&quot;:            200,
&quot;lat&quot;:       40695733,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.004Z&quot;,
&quot;lng&quot;:      -73971296,
&quot;id&quot;:            200,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988038,      40.770513 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;422 - W 59 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            201,
&quot;lat&quot;:       40770513,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.01Z&quot;,
&quot;lng&quot;:      -73988038,
&quot;id&quot;:            201,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986905,      40.765849 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;423 - W 54 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            202,
&quot;lat&quot;:       40765849,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.011Z&quot;,
&quot;lng&quot;:      -73986905,
&quot;id&quot;:            202,
&quot;free&quot;:             33 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.01322,      40.717548 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;426 - West St &amp; Chambers St&quot;,
&quot;idx&quot;:            203,
&quot;lat&quot;:       40717548,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.012Z&quot;,
&quot;lng&quot;:      -74013220,
&quot;id&quot;:            203,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.01427,      40.702515 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             21,
&quot;name&quot;: &quot;427 - State St&quot;,
&quot;idx&quot;:            204,
&quot;lat&quot;:       40702515,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.012Z&quot;,
&quot;lng&quot;:      -74014270,
&quot;id&quot;:            204,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987834,      40.724677 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;428 - E 3 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            205,
&quot;lat&quot;:       40724677,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.013Z&quot;,
&quot;lng&quot;:      -73987834,
&quot;id&quot;:            205,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986569,      40.701485 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;430 - York St &amp; Jay St&quot;,
&quot;idx&quot;:            206,
&quot;lat&quot;:       40701485,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.014Z&quot;,
&quot;lng&quot;:      -73986569,
&quot;id&quot;:            206,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982634,      40.688646 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;431 - Hanover Pl &amp; Livingston St&quot;,
&quot;idx&quot;:            207,
&quot;lat&quot;:       40688646,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.014Z&quot;,
&quot;lng&quot;:      -73982634,
&quot;id&quot;:            207,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983798,      40.726217 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;432 - E 7 St &amp; Avenue A&quot;,
&quot;idx&quot;:            208,
&quot;lat&quot;:       40726217,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.015Z&quot;,
&quot;lng&quot;:      -73983798,
&quot;id&quot;:            208,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980572,      40.729553 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;433 - E 13 St &amp; Avenue A&quot;,
&quot;idx&quot;:            209,
&quot;lat&quot;:       40729553,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.016Z&quot;,
&quot;lng&quot;:      -73980572,
&quot;id&quot;:            209,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.003664,      40.743174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;434 - 9 Ave &amp; W 18 St&quot;,
&quot;idx&quot;:            210,
&quot;lat&quot;:       40743174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.017Z&quot;,
&quot;lng&quot;:      -74003664,
&quot;id&quot;:            210,
&quot;free&quot;:              1 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994155,      40.741739 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;435 - W 21 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            211,
&quot;lat&quot;:       40741739,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.017Z&quot;,
&quot;lng&quot;:      -73994155,
&quot;id&quot;:            211,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.95399,      40.682165 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;436 - Hancock St &amp; Bedford Ave&quot;,
&quot;idx&quot;:            212,
&quot;lat&quot;:       40682165,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.018Z&quot;,
&quot;lng&quot;:      -73953990,
&quot;id&quot;:            212,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.950047,      40.680983 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;437 - Macon St &amp; Nostrand Ave&quot;,
&quot;idx&quot;:            213,
&quot;lat&quot;:       40680983,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.019Z&quot;,
&quot;lng&quot;:      -73950047,
&quot;id&quot;:            213,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.985649,      40.727791 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;438 - St Marks Pl &amp; 1 Ave&quot;,
&quot;idx&quot;:            214,
&quot;lat&quot;:       40727791,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.019Z&quot;,
&quot;lng&quot;:      -73985649,
&quot;id&quot;:            214,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98978,       40.72628 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;439 - E 4 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            215,
&quot;lat&quot;:       40726280,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.02Z&quot;,
&quot;lng&quot;:      -73989780,
&quot;id&quot;:            215,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972826,      40.752554 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;440 - E 45 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            216,
&quot;lat&quot;:       40752554,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.021Z&quot;,
&quot;lng&quot;:      -73972826,
&quot;id&quot;:            216,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967416,      40.756014 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;441 - E 52 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            217,
&quot;lat&quot;:       40756014,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.021Z&quot;,
&quot;lng&quot;:      -73967416,
&quot;id&quot;:            217,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993915,      40.746647 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;442 - W 27 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            218,
&quot;lat&quot;:       40746647,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.022Z&quot;,
&quot;lng&quot;:      -73993915,
&quot;id&quot;:            218,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.964089,       40.70853 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;443 - Bedford Ave &amp; S 9 St&quot;,
&quot;idx&quot;:            219,
&quot;lat&quot;:       40708530,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.023Z&quot;,
&quot;lng&quot;:      -73964089,
&quot;id&quot;:            219,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98915,      40.742354 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;444 - Broadway &amp; W 24 St&quot;,
&quot;idx&quot;:            220,
&quot;lat&quot;:       40742354,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.023Z&quot;,
&quot;lng&quot;:      -73989150,
&quot;id&quot;:            220,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98142,      40.727407 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             42,
&quot;name&quot;: &quot;445 - E 10 St &amp; Avenue A&quot;,
&quot;idx&quot;:            221,
&quot;lat&quot;:       40727407,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.031Z&quot;,
&quot;lng&quot;:      -73981420,
&quot;id&quot;:            221,
&quot;free&quot;:              0 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995298,      40.744876 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;446 - W 24 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            222,
&quot;lat&quot;:       40744876,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.031Z&quot;,
&quot;lng&quot;:      -73995298,
&quot;id&quot;:            222,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.985161,      40.763707 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;447 - 8 Ave &amp; W 52 St&quot;,
&quot;idx&quot;:            223,
&quot;lat&quot;:       40763707,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.032Z&quot;,
&quot;lng&quot;:      -73985161,
&quot;id&quot;:            223,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9979,      40.756603 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;448 - W 37 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            224,
&quot;lat&quot;:       40756603,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.033Z&quot;,
&quot;lng&quot;:      -73997900,
&quot;id&quot;:            224,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987894,      40.764618 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;449 - W 52 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            225,
&quot;lat&quot;:       40764618,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.033Z&quot;,
&quot;lng&quot;:      -73987894,
&quot;id&quot;:            225,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987882,      40.762272 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;450 - W 49 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            226,
&quot;lat&quot;:       40762272,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.034Z&quot;,
&quot;lng&quot;:      -73987882,
&quot;id&quot;:            226,
&quot;free&quot;:             53 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999153,      40.744751 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;453 - W 22 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            227,
&quot;lat&quot;:       40744751,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.035Z&quot;,
&quot;lng&quot;:      -73999153,
&quot;id&quot;:            227,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965929,      40.754557 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;454 - E 51 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            228,
&quot;lat&quot;:       40754557,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.035Z&quot;,
&quot;lng&quot;:      -73965929,
&quot;id&quot;:            228,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969053,      40.750019 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;455 - 1 Ave &amp; E 44 St&quot;,
&quot;idx&quot;:            229,
&quot;lat&quot;:       40750019,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.036Z&quot;,
&quot;lng&quot;:      -73969053,
&quot;id&quot;:            229,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974023,       40.75971 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;456 - E 53 St &amp; Madison Ave&quot;,
&quot;idx&quot;:            230,
&quot;lat&quot;:       40759710,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.037Z&quot;,
&quot;lng&quot;:      -73974023,
&quot;id&quot;:            230,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981693,      40.766953 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;457 - Broadway &amp; W 58 St&quot;,
&quot;idx&quot;:            231,
&quot;lat&quot;:       40766953,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.037Z&quot;,
&quot;lng&quot;:      -73981693,
&quot;id&quot;:            231,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005226,      40.751396 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;458 - 11 Ave &amp; W 27 St&quot;,
&quot;idx&quot;:            232,
&quot;lat&quot;:       40751396,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.038Z&quot;,
&quot;lng&quot;:      -74005226,
&quot;id&quot;:            232,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007756,      40.746745 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;459 - W 20 St &amp; 11 Ave&quot;,
&quot;idx&quot;:            233,
&quot;lat&quot;:       40746745,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.039Z&quot;,
&quot;lng&quot;:      -74007756,
&quot;id&quot;:            233,
&quot;free&quot;:             34 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965902,      40.712858 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;460 - S 4 St &amp; Wythe Ave&quot;,
&quot;idx&quot;:            234,
&quot;lat&quot;:       40712858,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.039Z&quot;,
&quot;lng&quot;:      -73965902,
&quot;id&quot;:            234,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98205,      40.735876 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;461 - E 20 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            235,
&quot;lat&quot;:       40735876,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.04Z&quot;,
&quot;lng&quot;:      -73982050,
&quot;id&quot;:            235,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004518,      40.746919 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;462 - W 22 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            236,
&quot;lat&quot;:       40746919,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.041Z&quot;,
&quot;lng&quot;:      -74004518,
&quot;id&quot;:            236,
&quot;free&quot;:             34 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004431,      40.742065 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;463 - 9 Ave &amp; W 16 St&quot;,
&quot;idx&quot;:            237,
&quot;lat&quot;:       40742065,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.041Z&quot;,
&quot;lng&quot;:      -74004431,
&quot;id&quot;:            237,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967596,      40.759345 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;464 - E 56 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            238,
&quot;lat&quot;:       40759345,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.042Z&quot;,
&quot;lng&quot;:      -73967596,
&quot;id&quot;:            238,
&quot;free&quot;:             27 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98658,      40.755135 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;465 - Broadway &amp; W 41 St&quot;,
&quot;idx&quot;:            239,
&quot;lat&quot;:       40755135,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.046Z&quot;,
&quot;lng&quot;:      -73986580,
&quot;id&quot;:            239,
&quot;free&quot;:             39 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991448,      40.743954 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;466 - W 25 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            240,
&quot;lat&quot;:       40743954,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.047Z&quot;,
&quot;lng&quot;:      -73991448,
&quot;id&quot;:            240,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978951,      40.683124 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;467 - Dean St &amp; 4 Ave&quot;,
&quot;idx&quot;:            241,
&quot;lat&quot;:       40683124,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.048Z&quot;,
&quot;lng&quot;:      -73978951,
&quot;id&quot;:            241,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981923,      40.765265 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;468 - Broadway &amp; W 55 St&quot;,
&quot;idx&quot;:            242,
&quot;lat&quot;:       40765265,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.049Z&quot;,
&quot;lng&quot;:      -73981923,
&quot;id&quot;:            242,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982681,       40.76344 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;469 - Broadway &amp; W 53 St&quot;,
&quot;idx&quot;:            243,
&quot;lat&quot;:       40763440,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.049Z&quot;,
&quot;lng&quot;:      -73982681,
&quot;id&quot;:            243,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00004,      40.743453 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;470 - W 20 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            244,
&quot;lat&quot;:       40743453,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.05Z&quot;,
&quot;lng&quot;:      -74000040,
&quot;id&quot;:            244,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.956981,      40.712868 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;471 - Grand St &amp; Havemeyer St&quot;,
&quot;idx&quot;:            245,
&quot;lat&quot;:       40712868,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.051Z&quot;,
&quot;lng&quot;:      -73956981,
&quot;id&quot;:            245,
&quot;free&quot;:              1 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981948,      40.745712 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;472 - E 32 St &amp; Park Ave&quot;,
&quot;idx&quot;:            246,
&quot;lat&quot;:       40745712,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.051Z&quot;,
&quot;lng&quot;:      -73981948,
&quot;id&quot;:            246,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991925,        40.7211 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;473 - Rivington St &amp; Chrystie St&quot;,
&quot;idx&quot;:            247,
&quot;lat&quot;:       40721100,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.052Z&quot;,
&quot;lng&quot;:      -73991925,
&quot;id&quot;:            247,
&quot;free&quot;:             28 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98683,      40.745167 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;474 - 5 Ave &amp; E 29 St&quot;,
&quot;idx&quot;:            248,
&quot;lat&quot;:       40745167,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.053Z&quot;,
&quot;lng&quot;:      -73986830,
&quot;id&quot;:            248,
&quot;free&quot;:             41 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987585,      40.735242 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;475 - E 16 St &amp; Irving Pl&quot;,
&quot;idx&quot;:            249,
&quot;lat&quot;:       40735242,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.053Z&quot;,
&quot;lng&quot;:      -73987585,
&quot;id&quot;:            249,
&quot;free&quot;:             14 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.97966,      40.743943 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             43,
&quot;name&quot;: &quot;476 - E 31 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            250,
&quot;lat&quot;:       40743943,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.054Z&quot;,
&quot;lng&quot;:      -73979660,
&quot;id&quot;:            250,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990026,      40.756405 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             42,
&quot;name&quot;: &quot;477 - W 41 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            251,
&quot;lat&quot;:       40756405,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.055Z&quot;,
&quot;lng&quot;:      -73990026,
&quot;id&quot;:            251,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998842,        40.7603 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;478 - 11 Ave &amp; W 41 St&quot;,
&quot;idx&quot;:            252,
&quot;lat&quot;:       40760300,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.055Z&quot;,
&quot;lng&quot;:      -73998842,
&quot;id&quot;:            252,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991255,      40.760192 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;479 - 9 Ave &amp; W 45 St&quot;,
&quot;idx&quot;:            253,
&quot;lat&quot;:       40760192,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.056Z&quot;,
&quot;lng&quot;:      -73991255,
&quot;id&quot;:            253,
&quot;free&quot;:             10 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990617,      40.766696 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;480 - W 53 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            254,
&quot;lat&quot;:       40766696,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.057Z&quot;,
&quot;lng&quot;:      -73990617,
&quot;id&quot;:            254,
&quot;free&quot;:              7 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.962644,      40.712604 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;481 - S 3 St &amp; Bedford Ave&quot;,
&quot;idx&quot;:            255,
&quot;lat&quot;:       40712604,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.057Z&quot;,
&quot;lng&quot;:      -73962644,
&quot;id&quot;:            255,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999317,      40.739355 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;482 - W 15 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            256,
&quot;lat&quot;:       40739355,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.058Z&quot;,
&quot;lng&quot;:      -73999317,
&quot;id&quot;:            256,
&quot;free&quot;:              9 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988899,      40.732232 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;483 - E 12 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            257,
&quot;lat&quot;:       40732232,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.059Z&quot;,
&quot;lng&quot;:      -73988899,
&quot;id&quot;:            257,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980144,      40.755002 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;484 - W 44 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            258,
&quot;lat&quot;:       40755002,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.059Z&quot;,
&quot;lng&quot;:      -73980144,
&quot;id&quot;:            258,
&quot;free&quot;:             44 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983389,       40.75038 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;485 - W 37 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            259,
&quot;lat&quot;:       40750380,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.066Z&quot;,
&quot;lng&quot;:      -73983389,
&quot;id&quot;:            259,
&quot;free&quot;:             33 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988557,        40.7462 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;486 - Broadway &amp; W 29 St&quot;,
&quot;idx&quot;:            260,
&quot;lat&quot;:       40746200,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.067Z&quot;,
&quot;lng&quot;:      -73988557,
&quot;id&quot;:            260,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.975738,      40.733142 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;487 - E 20 St &amp; FDR Drive&quot;,
&quot;idx&quot;:            261,
&quot;lat&quot;:       40733142,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.068Z&quot;,
&quot;lng&quot;:      -73975738,
&quot;id&quot;:            261,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993722,      40.756458 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;488 - W 39 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            262,
&quot;lat&quot;:       40756458,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.069Z&quot;,
&quot;lng&quot;:      -73993722,
&quot;id&quot;:            262,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001768,      40.750663 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;489 - 10 Ave &amp; W 28 St&quot;,
&quot;idx&quot;:            263,
&quot;lat&quot;:       40750663,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.069Z&quot;,
&quot;lng&quot;:      -74001768,
&quot;id&quot;:            263,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993934,      40.751551 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;490 - 8 Ave &amp; W 33 St&quot;,
&quot;idx&quot;:            264,
&quot;lat&quot;:       40751551,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.07Z&quot;,
&quot;lng&quot;:      -73993934,
&quot;id&quot;:            264,
&quot;free&quot;:             33 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986022,      40.740963 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;491 - E 24 St &amp; Park Ave S&quot;,
&quot;idx&quot;:            265,
&quot;lat&quot;:       40740963,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.071Z&quot;,
&quot;lng&quot;:      -73986022,
&quot;id&quot;:            265,
&quot;free&quot;:             50 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99093,      40.750199 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             47,
&quot;name&quot;: &quot;492 - W 33 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            266,
&quot;lat&quot;:       40750199,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.071Z&quot;,
&quot;lng&quot;:      -73990930,
&quot;id&quot;:            266,
&quot;free&quot;:              2 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982911,        40.7568 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;493 - W 45 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            267,
&quot;lat&quot;:       40756800,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.072Z&quot;,
&quot;lng&quot;:      -73982911,
&quot;id&quot;:            267,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997235,      40.747348 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;494 - W 26 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            268,
&quot;lat&quot;:       40747348,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.073Z&quot;,
&quot;lng&quot;:      -73997235,
&quot;id&quot;:            268,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993012,      40.762698 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             21,
&quot;name&quot;: &quot;495 - W 47 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            269,
&quot;lat&quot;:       40762698,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.073Z&quot;,
&quot;lng&quot;:      -73993012,
&quot;id&quot;:            269,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992389,      40.737261 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;496 - E 16 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            270,
&quot;lat&quot;:       40737261,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.074Z&quot;,
&quot;lng&quot;:      -73992389,
&quot;id&quot;:            270,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990092,      40.737049 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             36,
&quot;name&quot;: &quot;497 - E 17 St &amp; Broadway&quot;,
&quot;idx&quot;:            271,
&quot;lat&quot;:       40737049,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.075Z&quot;,
&quot;lng&quot;:      -73990092,
&quot;id&quot;:            271,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988084,      40.748548 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;498 - Broadway &amp; W 32 St&quot;,
&quot;idx&quot;:            272,
&quot;lat&quot;:       40748548,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.075Z&quot;,
&quot;lng&quot;:      -73988084,
&quot;id&quot;:            272,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981918,      40.769155 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;499 - Broadway &amp; W 60 St&quot;,
&quot;idx&quot;:            273,
&quot;lat&quot;:       40769155,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.076Z&quot;,
&quot;lng&quot;:      -73981918,
&quot;id&quot;:            273,
&quot;free&quot;:             25 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983361,      40.762288 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;500 - Broadway &amp; W 51 St&quot;,
&quot;idx&quot;:            274,
&quot;lat&quot;:       40762288,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.077Z&quot;,
&quot;lng&quot;:      -73983361,
&quot;id&quot;:            274,
&quot;free&quot;:             42 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971212,      40.744219 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;501 - FDR Drive &amp; E 35 St&quot;,
&quot;idx&quot;:            275,
&quot;lat&quot;:       40744219,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.077Z&quot;,
&quot;lng&quot;:      -73971212,
&quot;id&quot;:            275,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981346,      40.714215 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;502 - Henry St &amp; Grand St&quot;,
&quot;idx&quot;:            276,
&quot;lat&quot;:       40714215,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.078Z&quot;,
&quot;lng&quot;:      -73981346,
&quot;id&quot;:            276,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987519,      40.738274 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;503 - E 20 St &amp; Park Ave&quot;,
&quot;idx&quot;:            277,
&quot;lat&quot;:       40738274,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.079Z&quot;,
&quot;lng&quot;:      -73987519,
&quot;id&quot;:            277,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981655,      40.732218 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;504 - 1 Ave &amp; E 15 St&quot;,
&quot;idx&quot;:            278,
&quot;lat&quot;:       40732218,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.079Z&quot;,
&quot;lng&quot;:      -73981655,
&quot;id&quot;:            278,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988483,      40.749012 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;505 - 6 Ave &amp; W 33 St&quot;,
&quot;idx&quot;:            279,
&quot;lat&quot;:       40749012,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.083Z&quot;,
&quot;lng&quot;:      -73988483,
&quot;id&quot;:            279,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979737,      40.739126 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;507 - E 25 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            280,
&quot;lat&quot;:       40739126,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.084Z&quot;,
&quot;lng&quot;:      -73979737,
&quot;id&quot;:            280,
&quot;free&quot;:             32 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996674,      40.763413 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;508 - W 46 St &amp; 11 Ave&quot;,
&quot;idx&quot;:            281,
&quot;lat&quot;:       40763413,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.084Z&quot;,
&quot;lng&quot;:      -73996674,
&quot;id&quot;:            281,
&quot;free&quot;:              7 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001971,      40.745497 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;509 - 9 Ave &amp; W 22 St&quot;,
&quot;idx&quot;:            282,
&quot;lat&quot;:       40745497,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.085Z&quot;,
&quot;lng&quot;:      -74001971,
&quot;id&quot;:            282,
&quot;free&quot;:              6 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98042,      40.760659 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;510 - W 51 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            283,
&quot;lat&quot;:       40760659,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.086Z&quot;,
&quot;lng&quot;:      -73980420,
&quot;id&quot;:            283,
&quot;free&quot;:             49 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977724,      40.729386 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             21,
&quot;name&quot;: &quot;511 - E 14 St &amp; Avenue B&quot;,
&quot;idx&quot;:            284,
&quot;lat&quot;:       40729386,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.086Z&quot;,
&quot;lng&quot;:      -73977724,
&quot;id&quot;:            284,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998392,      40.750072 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;512 - W 29 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            285,
&quot;lat&quot;:       40750072,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.087Z&quot;,
&quot;lng&quot;:      -73998392,
&quot;id&quot;:            285,
&quot;free&quot;:             18 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988639,      40.768254 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;513 - W 56 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            286,
&quot;lat&quot;:       40768254,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.088Z&quot;,
&quot;lng&quot;:      -73988639,
&quot;id&quot;:            286,
&quot;free&quot;:             22 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002776,      40.760875 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;514 - 12 Ave &amp; W 40 St&quot;,
&quot;idx&quot;:            287,
&quot;lat&quot;:       40760875,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.089Z&quot;,
&quot;lng&quot;:      -74002776,
&quot;id&quot;:            287,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994618,      40.760094 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             27,
&quot;name&quot;: &quot;515 - W 43 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            288,
&quot;lat&quot;:       40760094,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.089Z&quot;,
&quot;lng&quot;:      -73994618,
&quot;id&quot;:            288,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967843,      40.752068 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;516 - E 47 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            289,
&quot;lat&quot;:       40752068,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.09Z&quot;,
&quot;lng&quot;:      -73967843,
&quot;id&quot;:            289,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977988,      40.751492 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             42,
&quot;name&quot;: &quot;517 - Pershing Square S&quot;,
&quot;idx&quot;:            290,
&quot;lat&quot;:       40751492,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.091Z&quot;,
&quot;lng&quot;:      -73977988,
&quot;id&quot;:            290,
&quot;free&quot;:             13 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973441,      40.747803 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;518 - E 39 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            291,
&quot;lat&quot;:       40747803,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.091Z&quot;,
&quot;lng&quot;:      -73973441,
&quot;id&quot;:            291,
&quot;free&quot;:             37 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977701,      40.751884 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             36,
&quot;name&quot;: &quot;519 - Pershing Square N&quot;,
&quot;idx&quot;:            292,
&quot;lat&quot;:       40751884,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.092Z&quot;,
&quot;lng&quot;:      -73977701,
&quot;id&quot;:            292,
&quot;free&quot;:             23 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976485,      40.759922 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;520 - W 52 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            293,
&quot;lat&quot;:       40759922,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.095Z&quot;,
&quot;lng&quot;:      -73976485,
&quot;id&quot;:            293,
&quot;free&quot;:             39 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99481,      40.750449 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;521 - 8 Ave &amp; W 31 St&quot;,
&quot;idx&quot;:            294,
&quot;lat&quot;:       40750449,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.096Z&quot;,
&quot;lng&quot;:      -73994810,
&quot;id&quot;:            294,
&quot;free&quot;:             47 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972078,      40.757147 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;522 - E 51 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:            295,
&quot;lat&quot;:       40757147,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.096Z&quot;,
&quot;lng&quot;:      -73972078,
&quot;id&quot;:            295,
&quot;free&quot;:             50 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991381,      40.754665 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;523 - W 38 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            296,
&quot;lat&quot;:       40754665,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.097Z&quot;,
&quot;lng&quot;:      -73991381,
&quot;id&quot;:            296,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983169,      40.755273 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;524 - W 43 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            297,
&quot;lat&quot;:       40755273,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.098Z&quot;,
&quot;lng&quot;:      -73983169,
&quot;id&quot;:            297,
&quot;free&quot;:             54 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002116,      40.755941 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;525 - W 34 St &amp; 11 Ave&quot;,
&quot;idx&quot;:            298,
&quot;lat&quot;:       40755941,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.098Z&quot;,
&quot;lng&quot;:      -74002116,
&quot;id&quot;:            298,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984907,      40.747659 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;526 - E 33 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            299,
&quot;lat&quot;:       40747659,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.099Z&quot;,
&quot;lng&quot;:      -73984907,
&quot;id&quot;:            299,
&quot;free&quot;:             36 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974347,      40.743155 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             48,
&quot;name&quot;: &quot;527 - E 33 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            300,
&quot;lat&quot;:       40743155,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.1Z&quot;,
&quot;lng&quot;:      -73974347,
&quot;id&quot;:            300,
&quot;free&quot;:              9 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.97706,      40.742909 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;528 - 2 Ave &amp; E 30 St&quot;,
&quot;idx&quot;:            301,
&quot;lat&quot;:       40742909,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.101Z&quot;,
&quot;lng&quot;:      -73977060,
&quot;id&quot;:            301,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990985,      40.757569 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;529 - W 42 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            302,
&quot;lat&quot;:       40757569,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.101Z&quot;,
&quot;lng&quot;:      -73990985,
&quot;id&quot;:            302,
&quot;free&quot;:             10 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992662,      40.718939 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;531 - Forsyth St &amp; Broome St&quot;,
&quot;idx&quot;:            303,
&quot;lat&quot;:       40718939,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.102Z&quot;,
&quot;lng&quot;:      -73992662,
&quot;id&quot;:            303,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.960876,      40.710451 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             40,
&quot;name&quot;: &quot;532 - S 5 Pl &amp; S 4 St&quot;,
&quot;idx&quot;:            304,
&quot;lat&quot;:       40710451,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.107Z&quot;,
&quot;lng&quot;:      -73960876,
&quot;id&quot;:            304,
&quot;free&quot;:              2 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987216,      40.752996 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;533 - Broadway &amp; W 39 St&quot;,
&quot;idx&quot;:            305,
&quot;lat&quot;:       40752996,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.107Z&quot;,
&quot;lng&quot;:      -73987216,
&quot;id&quot;:            305,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.012723,       40.70255 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;534 - Water - Whitehall Plaza&quot;,
&quot;idx&quot;:            306,
&quot;lat&quot;:       40702550,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.108Z&quot;,
&quot;lng&quot;:      -74012723,
&quot;id&quot;:            306,
&quot;free&quot;:             29 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.97536,      40.741443 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;536 - 1 Ave &amp; E 30 St&quot;,
&quot;idx&quot;:            307,
&quot;lat&quot;:       40741443,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.109Z&quot;,
&quot;lng&quot;:      -73975360,
&quot;id&quot;:            307,
&quot;free&quot;:             21 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984092,      40.740258 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;537 - Lexington Ave &amp; E 24 St&quot;,
&quot;idx&quot;:            308,
&quot;lat&quot;:       40740258,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.11Z&quot;,
&quot;lng&quot;:      -73984092,
&quot;id&quot;:            308,
&quot;free&quot;:             34 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977876,      40.757952 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;538 - W 49 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            309,
&quot;lat&quot;:       40757952,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.11Z&quot;,
&quot;lng&quot;:      -73977876,
&quot;id&quot;:            309,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.960241,      40.715348 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;539 - Metropolitan Ave &amp; Bedford Ave&quot;,
&quot;idx&quot;:            310,
&quot;lat&quot;:       40715348,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.111Z&quot;,
&quot;lng&quot;:      -73960241,
&quot;id&quot;:            310,
&quot;free&quot;:              3 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983209,      40.741472 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;540 - Lexington Ave &amp; E 26 St&quot;,
&quot;idx&quot;:            311,
&quot;lat&quot;:       40741472,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.112Z&quot;,
&quot;lng&quot;:      -73983209,
&quot;id&quot;:            311,
&quot;free&quot;:             24 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978094,      40.736502 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;545 - E 23 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            312,
&quot;lat&quot;:       40736502,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.112Z&quot;,
&quot;lng&quot;:      -73978094,
&quot;id&quot;:            312,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983035,      40.744449 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;546 - E 30 St &amp; Park Ave S&quot;,
&quot;idx&quot;:            313,
&quot;lat&quot;:       40744449,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.113Z&quot;,
&quot;lng&quot;:      -73983035,
&quot;id&quot;:            313,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989402,       40.70255 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;2000 - Front St &amp; Washington St&quot;,
&quot;idx&quot;:            314,
&quot;lat&quot;:       40702550,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.114Z&quot;,
&quot;lng&quot;:      -73989402,
&quot;id&quot;:            314,
&quot;free&quot;:             16 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973329,       40.69892 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;2001 - 7 Ave &amp; Farragut St&quot;,
&quot;idx&quot;:            315,
&quot;lat&quot;:       40698920,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.114Z&quot;,
&quot;lng&quot;:      -73973329,
&quot;id&quot;:            315,
&quot;free&quot;:              5 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.963198,      40.716887 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;2002 - Wythe Ave &amp; Metropolitan Ave&quot;,
&quot;idx&quot;:            316,
&quot;lat&quot;:       40716887,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.115Z&quot;,
&quot;lng&quot;:      -73963198,
&quot;id&quot;:            316,
&quot;free&quot;:              8 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980242,       40.73416 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;2003 - 1 Ave &amp; E 18 St&quot;,
&quot;idx&quot;:            317,
&quot;lat&quot;:       40734160,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.116Z&quot;,
&quot;lng&quot;:      -73980242,
&quot;id&quot;:            317,
&quot;free&quot;:             17 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004704,      40.724399 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;2004 - 6 Ave &amp; Broome St&quot;,
&quot;idx&quot;:            318,
&quot;lat&quot;:       40724399,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.117Z&quot;,
&quot;lng&quot;:      -74004704,
&quot;id&quot;:            318,
&quot;free&quot;:             19 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [        -73.971,      40.705311 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;2005 - Railroad Ave &amp; Kay Ave&quot;,
&quot;idx&quot;:            319,
&quot;lat&quot;:       40705311,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.117Z&quot;,
&quot;lng&quot;:      -73971000,
&quot;id&quot;:            319,
&quot;free&quot;:              1 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976341,      40.765909 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;2006 - Central Park S &amp; 6 Ave&quot;,
&quot;idx&quot;:            320,
&quot;lat&quot;:       40765909,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.118Z&quot;,
&quot;lng&quot;:      -73976341,
&quot;id&quot;:            320,
&quot;free&quot;:             41 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.016776,      40.705692 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;2008 - Little West St &amp; 1 Pl&quot;,
&quot;idx&quot;:            321,
&quot;lat&quot;:       40705692,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.119Z&quot;,
&quot;lng&quot;:      -74016776,
&quot;id&quot;:            321,
&quot;free&quot;:              1 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996826,      40.711174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;2009 - Catherine St &amp; Monroe St&quot;,
&quot;idx&quot;:            322,
&quot;lat&quot;:       40711174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.119Z&quot;,
&quot;lng&quot;:      -73996826,
&quot;id&quot;:            322,
&quot;free&quot;:             26 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002347,      40.721654 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;2010 - Grand St &amp; Greene St&quot;,
&quot;idx&quot;:            323,
&quot;lat&quot;:       40721654,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.12Z&quot;,
&quot;lng&quot;:      -74002347,
&quot;id&quot;:            323,
&quot;free&quot;:             34 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976806,      40.739445 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;2012 - E 27 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            324,
&quot;lat&quot;:       40739445,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.121Z&quot;,
&quot;lng&quot;:      -73976806,
&quot;id&quot;:            324,
&quot;free&quot;:             30 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971214,      40.750223 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;2017 - E 43 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            325,
&quot;lat&quot;:       40750223,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.127Z&quot;,
&quot;lng&quot;:      -73971214,
&quot;id&quot;:            325,
&quot;free&quot;:             38 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988596,      40.759291 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             31,
&quot;name&quot;: &quot;2021 - W 45 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            326,
&quot;lat&quot;:       40759291,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.128Z&quot;,
&quot;lng&quot;:      -73988596,
&quot;id&quot;:            326,
&quot;free&quot;:             12 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.959206,      40.758491 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             27,
&quot;name&quot;: &quot;2022 - E 59 St &amp; Sutton Pl&quot;,
&quot;idx&quot;:            327,
&quot;lat&quot;:       40758491,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.128Z&quot;,
&quot;lng&quot;:      -73959206,
&quot;id&quot;:            327,
&quot;free&quot;:              4 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.970313,       40.75968 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;2023 - E 55 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:            328,
&quot;lat&quot;:       40759680,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.129Z&quot;,
&quot;lng&quot;:      -73970313,
&quot;id&quot;:            328,
&quot;free&quot;:             35 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.015756,      40.711512 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;3002 - South End Ave &amp; Liberty St&quot;,
&quot;idx&quot;:            329,
&quot;lat&quot;:       40711512,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.13Z&quot;,
&quot;lng&quot;:      -74015756,
&quot;id&quot;:            329,
&quot;free&quot;:             20 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99906,      40.730477 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;336 - Sullivan St &amp; Washington Sq&quot;,
&quot;idx&quot;:            330,
&quot;lat&quot;:       40730477,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.936Z&quot;,
&quot;lng&quot;:      -73999060,
&quot;id&quot;:            330,
&quot;free&quot;:             11 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978474,      40.687979 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;243 - Fulton St &amp; Rockwell Pl&quot;,
&quot;idx&quot;:            331,
&quot;lat&quot;:       40687979,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.881Z&quot;,
&quot;lng&quot;:      -73978474,
&quot;id&quot;:            331,
&quot;free&quot;:             31 
} 
} 
] 
}

  var map = L.map(spec.dom, spec.mapOpts)
  
    map.setView(spec.center, spec.zoom);

    if (spec.provider){
      L.tileLayer.provider(spec.provider).addTo(map)    
    } else {
		  L.tileLayer(spec.urlTemplate, spec.layerOpts).addTo(map)
    }
     
    
    
    
    
    
    if (spec.circle2){
      for (var c in spec.circle2){
        var circle = L.circle(c.center, c.radius, c.opts)
         .addTo(map);
      }
    }
    
    
    
    
    var geojsonLayer = L.geoJson(spec.features 
        , 
         false
      ).addTo(map)
    
   
   
   
&lt;/script&gt;
    
  &lt;/body&gt;
&lt;/html&gt;
' scrolling='no' seamless class='rChart 
leaflet
 '
id='iframe-chart136453dba1d00'>
</iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>


## Step 5

Let us now add some fill color to the points so that it is easier to visualize bike availabilities at a glance. We do this by computing the percentage of available bikes at all stations, breaking them into quantiles, and assigning a color from a palette.


```r
bike <- lapply(bike, function(station){within(station, { 
  fillColor = cut(
    as.numeric(bikes)/(as.numeric(bikes)+as.numeric(free)), 
    breaks = c(0, 0.20, 0.40, 0.60, 0.80, 1), 
    labels = brewer.pal(5, 'RdYlGn'),
    include.lowest = TRUE
  )})
})
```


## Step 6



```r
L1$geoJson(toGeoJSON(bike), 
  pointToLayer =  "#! function(feature, latlng){
    return L.circleMarker(latlng, {
      radius: 4,
      fillColor: feature.properties.fillColor || 'red',    
      color: '#000',
      weight: 1,
      fillOpacity: 0.8
    })
  }!#"
)
L1
```

<iframe srcdoc='
&lt;!doctype HTML&gt;
&lt;meta charset = &#039;utf-8&#039;&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;link rel=&#039;stylesheet&#039; href=&#039;http://cdn.leafletjs.com/leaflet-0.5.1/leaflet.css&#039;&gt;
    
    &lt;script src=&#039;http://cdn.leafletjs.com/leaflet-0.5.1/leaflet.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    &lt;script src=&#039;https://rawgithub.com/leaflet-extras/leaflet-providers/gh-pages/leaflet-providers.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    &lt;script src=&#039;http://harrywood.co.uk/maps/examples/leaflet/leaflet-plugins/layer/vector/KML.js&#039; type=&#039;text/javascript&#039;&gt;&lt;/script&gt;
    
    &lt;style&gt;
    .rChart {
      display: block;
      margin-left: auto; 
      margin-right: auto;
      width: 1200px;
      height: 200px;
    }  
    &lt;/style&gt;
    
  &lt;/head&gt;
  &lt;body &gt;
    
    &lt;div id = &#039;chart136453dba1d00&#039; class = &#039;rChart leaflet&#039;&gt;&lt;/div&gt;    
    &lt;script&gt;
  var spec = {
 &quot;dom&quot;: &quot;chart136453dba1d00&quot;,
&quot;width&quot;:           1200,
&quot;height&quot;:            200,
&quot;urlTemplate&quot;: &quot;http://{s}.tile.osm.org/{z}/{x}/{y}.png&quot;,
&quot;layerOpts&quot;: {
 &quot;attribution&quot;: &quot;Map data&lt;a href=\&quot;http://openstreetmap.org\&quot;&gt;OpenStreetMap&lt;/a&gt;\n         contributors, Imagery&lt;a href=\&quot;http://mapbox.com\&quot;&gt;MapBox&lt;/a&gt;&quot; 
},
&quot;provider&quot;: &quot;Stamen.TonerLite&quot;,
&quot;center&quot;: [       40.73029,      -73.99076 ],
&quot;zoom&quot;:             13,
&quot;id&quot;: &quot;chart136453dba1d00&quot;,
&quot;features&quot;: [
 {
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993928,      40.767272 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;72 - W 52 St &amp; 11 Ave&quot;,
&quot;idx&quot;:              0,
&quot;lat&quot;:       40767272,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.849Z&quot;,
&quot;lng&quot;:      -73993928,
&quot;id&quot;:              0,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006666,      40.719115 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;79 - Franklin St &amp; W Broadway&quot;,
&quot;idx&quot;:              1,
&quot;lat&quot;:       40719115,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.85Z&quot;,
&quot;lng&quot;:      -74006666,
&quot;id&quot;:              1,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.000165,      40.711174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;82 - St James Pl &amp; Pearl St&quot;,
&quot;idx&quot;:              2,
&quot;lat&quot;:       40711174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.851Z&quot;,
&quot;lng&quot;:      -74000165,
&quot;id&quot;:              2,
&quot;free&quot;:             18,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976323,      40.683826 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             33,
&quot;name&quot;: &quot;83 - Atlantic Ave &amp; Fort Greene Pl&quot;,
&quot;idx&quot;:              3,
&quot;lat&quot;:       40683826,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.851Z&quot;,
&quot;lng&quot;:      -73976323,
&quot;id&quot;:              3,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001497,      40.741776 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;116 - W 17 St &amp; 8 Ave&quot;,
&quot;idx&quot;:              4,
&quot;lat&quot;:       40741776,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.852Z&quot;,
&quot;lng&quot;:      -74001497,
&quot;id&quot;:              4,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978034,      40.696089 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;119 - Park Ave &amp; St Edwards St&quot;,
&quot;idx&quot;:              5,
&quot;lat&quot;:       40696089,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.853Z&quot;,
&quot;lng&quot;:      -73978034,
&quot;id&quot;:              5,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.959281,      40.686767 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;120 - Lexington Ave &amp; Classon Ave&quot;,
&quot;idx&quot;:              6,
&quot;lat&quot;:       40686767,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.853Z&quot;,
&quot;lng&quot;:      -73959281,
&quot;id&quot;:              6,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006744,      40.731724 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;127 - Barrow St &amp; Hudson St&quot;,
&quot;idx&quot;:              7,
&quot;lat&quot;:       40731724,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.854Z&quot;,
&quot;lng&quot;:      -74006744,
&quot;id&quot;:              7,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00297,      40.727102 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;128 - MacDougal St &amp; Prince St&quot;,
&quot;idx&quot;:              8,
&quot;lat&quot;:       40727102,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.855Z&quot;,
&quot;lng&quot;:      -74002970,
&quot;id&quot;:              8,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972924,      40.761628 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;137 - E 56 St &amp; Madison Ave&quot;,
&quot;idx&quot;:              9,
&quot;lat&quot;:       40761628,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.856Z&quot;,
&quot;lng&quot;:      -73972924,
&quot;id&quot;:              9,
&quot;free&quot;:             46,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993379,      40.692395 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;143 - Clinton St &amp; Joralemon St&quot;,
&quot;idx&quot;:             10,
&quot;lat&quot;:       40692395,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.856Z&quot;,
&quot;lng&quot;:      -73993379,
&quot;id&quot;:             10,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980689,      40.698398 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;144 - Nassau St &amp; Navy St&quot;,
&quot;idx&quot;:             11,
&quot;lat&quot;:       40698398,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.857Z&quot;,
&quot;lng&quot;:      -73980689,
&quot;id&quot;:             11,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009105,       40.71625 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;146 - Hudson St &amp; Reade St&quot;,
&quot;idx&quot;:             12,
&quot;lat&quot;:       40716250,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.858Z&quot;,
&quot;lng&quot;:      -74009105,
&quot;id&quot;:             12,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.011219,      40.715421 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;147 - Greenwich St &amp; Warren St&quot;,
&quot;idx&quot;:             13,
&quot;lat&quot;:       40715421,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.858Z&quot;,
&quot;lng&quot;:      -74011219,
&quot;id&quot;:             13,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980857,      40.720873 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;150 - E 2 St &amp; Avenue C&quot;,
&quot;idx&quot;:             14,
&quot;lat&quot;:       40720873,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.859Z&quot;,
&quot;lng&quot;:      -73980857,
&quot;id&quot;:             14,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997203,      40.721815 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;151 - Cleveland Pl &amp; Spring St&quot;,
&quot;idx&quot;:             15,
&quot;lat&quot;:       40721815,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.86Z&quot;,
&quot;lng&quot;:      -73997203,
&quot;id&quot;:             15,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009106,      40.714739 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;152 - Warren St &amp; Church St&quot;,
&quot;idx&quot;:             16,
&quot;lat&quot;:       40714739,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.86Z&quot;,
&quot;lng&quot;:      -74009106,
&quot;id&quot;:             16,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981632,      40.752062 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;153 - E 40 St &amp; 5 Ave&quot;,
&quot;idx&quot;:             17,
&quot;lat&quot;:       40752062,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.862Z&quot;,
&quot;lng&quot;:      -73981632,
&quot;id&quot;:             17,
&quot;free&quot;:             55,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996123,      40.690892 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;157 - Henry St &amp; Atlantic Ave&quot;,
&quot;idx&quot;:             18,
&quot;lat&quot;:       40690892,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.863Z&quot;,
&quot;lng&quot;:      -73996123,
&quot;id&quot;:             18,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978311,      40.748238 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;160 - E 37 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:             19,
&quot;lat&quot;:       40748238,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.864Z&quot;,
&quot;lng&quot;:      -73978311,
&quot;id&quot;:             19,
&quot;free&quot;:             18,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998102,       40.72917 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;161 - LaGuardia Pl &amp; W 3 St&quot;,
&quot;idx&quot;:             20,
&quot;lat&quot;:       40729170,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.865Z&quot;,
&quot;lng&quot;:      -73998102,
&quot;id&quot;:             20,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.970325,       40.75323 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;164 - E 47 St &amp; 2 Ave&quot;,
&quot;idx&quot;:             21,
&quot;lat&quot;:       40753230,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.865Z&quot;,
&quot;lng&quot;:      -73970325,
&quot;id&quot;:             21,
&quot;free&quot;:             43,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976048,        40.7489 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;167 - E 39 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             22,
&quot;lat&quot;:       40748900,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.866Z&quot;,
&quot;lng&quot;:      -73976048,
&quot;id&quot;:             22,
&quot;free&quot;:             42,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994564,      40.739713 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;168 - W 18 St &amp; 6 Ave&quot;,
&quot;idx&quot;:             23,
&quot;lat&quot;:       40739713,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.867Z&quot;,
&quot;lng&quot;:      -73994564,
&quot;id&quot;:             23,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984426,      40.760646 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;173 - Broadway &amp; W 49 St&quot;,
&quot;idx&quot;:             24,
&quot;lat&quot;:       40760646,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.867Z&quot;,
&quot;lng&quot;:      -73984426,
&quot;id&quot;:             24,
&quot;free&quot;:             41,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977386,      40.738176 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;174 - E 25 St &amp; 1 Ave&quot;,
&quot;idx&quot;:             25,
&quot;lat&quot;:       40738176,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.868Z&quot;,
&quot;lng&quot;:      -73977386,
&quot;id&quot;:             25,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010433,      40.709056 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;195 - Liberty St &amp; Broadway&quot;,
&quot;idx&quot;:             26,
&quot;lat&quot;:       40709056,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.869Z&quot;,
&quot;lng&quot;:      -74010433,
&quot;id&quot;:             26,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006817,      40.743349 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;212 - W 16 St &amp; The High Line&quot;,
&quot;idx&quot;:             27,
&quot;lat&quot;:       40743349,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.869Z&quot;,
&quot;lng&quot;:      -74006817,
&quot;id&quot;:             27,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99548,      40.700378 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;216 - Columbia Heights &amp; Cranberry St&quot;,
&quot;idx&quot;:             28,
&quot;lat&quot;:       40700378,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.87Z&quot;,
&quot;lng&quot;:      -73995480,
&quot;id&quot;:             28,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993836,      40.702771 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             37,
&quot;name&quot;: &quot;217 - Old Fulton St&quot;,
&quot;idx&quot;:             29,
&quot;lat&quot;:       40702771,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.871Z&quot;,
&quot;lng&quot;:      -73993836,
&quot;id&quot;:             29,
&quot;free&quot;:              2,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987071,      40.690284 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;218 - Gallatin Pl &amp; Livingston St&quot;,
&quot;idx&quot;:             30,
&quot;lat&quot;:       40690284,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.871Z&quot;,
&quot;lng&quot;:      -73987071,
&quot;id&quot;:             30,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999946,      40.737815 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;223 - W 13 St &amp; 7 Ave&quot;,
&quot;idx&quot;:             31,
&quot;lat&quot;:       40737815,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.872Z&quot;,
&quot;lng&quot;:      -73999946,
&quot;id&quot;:             31,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005524,      40.711463 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;224 - Spruce St &amp; Nassau St&quot;,
&quot;idx&quot;:             32,
&quot;lat&quot;:       40711463,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.873Z&quot;,
&quot;lng&quot;:      -74005524,
&quot;id&quot;:             32,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00803,      40.741951 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;225 - W 14 St &amp; The High Line&quot;,
&quot;idx&quot;:             33,
&quot;lat&quot;:       40741951,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.873Z&quot;,
&quot;lng&quot;:      -74008030,
&quot;id&quot;:             33,
&quot;free&quot;:             15,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971878,      40.754601 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;228 - E 48 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             34,
&quot;lat&quot;:       40754601,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.874Z&quot;,
&quot;lng&quot;:      -73971878,
&quot;id&quot;:             34,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99379,      40.727434 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;229 - Great Jones St&quot;,
&quot;idx&quot;:             35,
&quot;lat&quot;:       40727434,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.875Z&quot;,
&quot;lng&quot;:      -73993790,
&quot;id&quot;:             35,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990148,      40.695976 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;232 - Cadman Plaza E &amp; Tillary St&quot;,
&quot;idx&quot;:             36,
&quot;lat&quot;:       40695976,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.876Z&quot;,
&quot;lng&quot;:      -73990148,
&quot;id&quot;:             36,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989639,      40.692462 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;233 - Joralemon St &amp; Adams St&quot;,
&quot;idx&quot;:             37,
&quot;lat&quot;:       40692462,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.876Z&quot;,
&quot;lng&quot;:      -73989639,
&quot;id&quot;:             37,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987139,      40.728418 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;236 - St Marks Pl &amp; 2 Ave&quot;,
&quot;idx&quot;:             38,
&quot;lat&quot;:       40728418,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.877Z&quot;,
&quot;lng&quot;:      -73987139,
&quot;id&quot;:             38,
&quot;free&quot;:              9,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986723,      40.730473 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;237 - E 11 St &amp; 2 Ave&quot;,
&quot;idx&quot;:             39,
&quot;lat&quot;:       40730473,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.878Z&quot;,
&quot;lng&quot;:      -73986723,
&quot;id&quot;:             39,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008592,      40.736196 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;238 - Bank St &amp; Washington St&quot;,
&quot;idx&quot;:             40,
&quot;lat&quot;:       40736196,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.878Z&quot;,
&quot;lng&quot;:      -74008592,
&quot;id&quot;:             40,
&quot;free&quot;:             18,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981301,      40.691965 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;239 - Willoughby St &amp; Fleet St&quot;,
&quot;idx&quot;:             41,
&quot;lat&quot;:       40691965,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.879Z&quot;,
&quot;lng&quot;:      -73981301,
&quot;id&quot;:             41,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974931,       40.68981 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;241 - DeKalb Ave &amp; S Portland Ave&quot;,
&quot;idx&quot;:             42,
&quot;lat&quot;:       40689810,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.88Z&quot;,
&quot;lng&quot;:      -73974931,
&quot;id&quot;:             42,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973503,      40.697883 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;242 - Flushing Ave &amp; Carlton Ave&quot;,
&quot;idx&quot;:             43,
&quot;lat&quot;:       40697883,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.881Z&quot;,
&quot;lng&quot;:      -73973503,
&quot;id&quot;:             43,
&quot;free&quot;:              9,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979244,      40.688265 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;243 - Fulton St &amp; Rockwell Pl&quot;,
&quot;idx&quot;:             44,
&quot;lat&quot;:       40688265,
&quot;timestamp&quot;: &quot;2014-01-31T17:08:01.291Z&quot;,
&quot;lng&quot;:      -73979244,
&quot;id&quot;:             44,
&quot;free&quot;:             31,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965368,       40.69196 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;244 - Willoughby Ave &amp; Hall St&quot;,
&quot;idx&quot;:             45,
&quot;lat&quot;:       40691960,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.882Z&quot;,
&quot;lng&quot;:      -73965368,
&quot;id&quot;:             45,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977038,       40.69327 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;245 - Myrtle Ave &amp; St Edwards St&quot;,
&quot;idx&quot;:             46,
&quot;lat&quot;:       40693270,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.883Z&quot;,
&quot;lng&quot;:      -73977038,
&quot;id&quot;:             46,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00483,      40.735353 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;247 - Perry St &amp; Bleecker St&quot;,
&quot;idx&quot;:             47,
&quot;lat&quot;:       40735353,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.883Z&quot;,
&quot;lng&quot;:      -74004830,
&quot;id&quot;:             47,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007717,      40.721853 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;248 - Laight St &amp; Hudson St&quot;,
&quot;idx&quot;:             48,
&quot;lat&quot;:       40721853,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.884Z&quot;,
&quot;lng&quot;:      -74007717,
&quot;id&quot;:             48,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [        -74.009,      40.718709 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;249 - Harrison St &amp; Hudson St&quot;,
&quot;idx&quot;:             49,
&quot;lat&quot;:       40718709,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.885Z&quot;,
&quot;lng&quot;:      -74009000,
&quot;id&quot;:             49,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995652,       40.72456 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;250 - Lafayette St &amp; Jersey St&quot;,
&quot;idx&quot;:             50,
&quot;lat&quot;:       40724560,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.886Z&quot;,
&quot;lng&quot;:      -73995652,
&quot;id&quot;:             50,
&quot;free&quot;:             40,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9948,      40.723179 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;251 - Mott St &amp; Prince St&quot;,
&quot;idx&quot;:             51,
&quot;lat&quot;:       40723179,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.886Z&quot;,
&quot;lng&quot;:      -73994800,
&quot;id&quot;:             51,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998522,      40.732263 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;252 - MacDougal St &amp; Washington Sq&quot;,
&quot;idx&quot;:             52,
&quot;lat&quot;:       40732263,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.887Z&quot;,
&quot;lng&quot;:      -73998522,
&quot;id&quot;:             52,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994539,      40.735439 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;253 - W 13 St &amp; 5 Ave&quot;,
&quot;idx&quot;:             53,
&quot;lat&quot;:       40735439,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.888Z&quot;,
&quot;lng&quot;:      -73994539,
&quot;id&quot;:             53,
&quot;free&quot;:             47,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998004,      40.735324 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;254 - W 11 St &amp; 6 Ave&quot;,
&quot;idx&quot;:             54,
&quot;lat&quot;:       40735324,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.888Z&quot;,
&quot;lng&quot;:      -73998004,
&quot;id&quot;:             54,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002472,      40.719392 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;257 - Lispenard St &amp; Broadway&quot;,
&quot;idx&quot;:             55,
&quot;lat&quot;:       40719392,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.889Z&quot;,
&quot;lng&quot;:      -74002472,
&quot;id&quot;:             55,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.968854,      40.689407 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;258 - DeKalb Ave &amp; Vanderbilt Ave&quot;,
&quot;idx&quot;:             56,
&quot;lat&quot;:       40689407,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.89Z&quot;,
&quot;lng&quot;:      -73968854,
&quot;id&quot;:             56,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.012342,      40.701221 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;259 - South St &amp; Whitehall St&quot;,
&quot;idx&quot;:             57,
&quot;lat&quot;:       40701221,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.89Z&quot;,
&quot;lng&quot;:      -74012342,
&quot;id&quot;:             57,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.011677,      40.703651 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;260 - Broad St &amp; Bridge St&quot;,
&quot;idx&quot;:             58,
&quot;lat&quot;:       40703651,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.891Z&quot;,
&quot;lng&quot;:      -74011677,
&quot;id&quot;:             58,
&quot;free&quot;:             10,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983624,      40.694748 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;261 - Johnson St &amp; Gold St&quot;,
&quot;idx&quot;:             59,
&quot;lat&quot;:       40694748,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.892Z&quot;,
&quot;lng&quot;:      -73983624,
&quot;id&quot;:             59,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973729,      40.691782 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;262 - Washington Park&quot;,
&quot;idx&quot;:             60,
&quot;lat&quot;:       40691782,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.892Z&quot;,
&quot;lng&quot;:      -73973729,
&quot;id&quot;:             60,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996375,       40.71729 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;263 - Elizabeth St &amp; Hester St&quot;,
&quot;idx&quot;:             61,
&quot;lat&quot;:       40717290,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.893Z&quot;,
&quot;lng&quot;:      -73996375,
&quot;id&quot;:             61,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007318,      40.707064 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;264 - Maiden Ln &amp; Pearl St&quot;,
&quot;idx&quot;:             62,
&quot;lat&quot;:       40707064,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.894Z&quot;,
&quot;lng&quot;:      -74007318,
&quot;id&quot;:             62,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991475,      40.722293 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;265 - Stanton St &amp; Chrystie St&quot;,
&quot;idx&quot;:             63,
&quot;lat&quot;:       40722293,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.894Z&quot;,
&quot;lng&quot;:      -73991475,
&quot;id&quot;:             63,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.975748,      40.723683 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;266 - Avenue D &amp; E 8 St&quot;,
&quot;idx&quot;:             64,
&quot;lat&quot;:       40723683,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.895Z&quot;,
&quot;lng&quot;:      -73975748,
&quot;id&quot;:             64,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987654,      40.750977 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;267 - Broadway &amp; W 36 St&quot;,
&quot;idx&quot;:             65,
&quot;lat&quot;:       40750977,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.896Z&quot;,
&quot;lng&quot;:      -73987654,
&quot;id&quot;:             65,
&quot;free&quot;:             33,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999733,      40.719105 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;268 - Howard St &amp; Centre St&quot;,
&quot;idx&quot;:             66,
&quot;lat&quot;:       40719105,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.896Z&quot;,
&quot;lng&quot;:      -73999733,
&quot;id&quot;:             66,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971789,      40.693082 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;270 - Adelphi St &amp; Myrtle Ave&quot;,
&quot;idx&quot;:             67,
&quot;lat&quot;:       40693082,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.897Z&quot;,
&quot;lng&quot;:      -73971789,
&quot;id&quot;:             67,
&quot;free&quot;:              7,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978058,      40.685281 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;271 - Ashland Pl &amp; Hanson Pl&quot;,
&quot;idx&quot;:             68,
&quot;lat&quot;:       40685281,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.898Z&quot;,
&quot;lng&quot;:      -73978058,
&quot;id&quot;:             68,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976682,      40.686918 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;274 - Lafayette Ave &amp; Fort Greene Pl&quot;,
&quot;idx&quot;:             69,
&quot;lat&quot;:       40686918,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.898Z&quot;,
&quot;lng&quot;:      -73976682,
&quot;id&quot;:             69,
&quot;free&quot;:              1,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965633,        40.6865 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;275 - Washington Ave &amp; Greene Ave&quot;,
&quot;idx&quot;:             70,
&quot;lat&quot;:       40686500,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.899Z&quot;,
&quot;lng&quot;:      -73965633,
&quot;id&quot;:             70,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010455,      40.717487 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;276 - Duane St &amp; Greenwich St&quot;,
&quot;idx&quot;:             71,
&quot;lat&quot;:       40717487,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.9Z&quot;,
&quot;lng&quot;:      -74010455,
&quot;id&quot;:             71,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984764,      40.697665 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;278 - Concord St &amp; Bridge St&quot;,
&quot;idx&quot;:             72,
&quot;lat&quot;:       40697665,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.9Z&quot;,
&quot;lng&quot;:      -73984764,
&quot;id&quot;:             72,
&quot;free&quot;:             15,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982719,      40.699869 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;279 - Sands St &amp; Gold St&quot;,
&quot;idx&quot;:             73,
&quot;lat&quot;:       40699869,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.901Z&quot;,
&quot;lng&quot;:      -73982719,
&quot;id&quot;:             73,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995101,      40.733319 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;280 - E 10 St &amp; 5 Ave&quot;,
&quot;idx&quot;:             74,
&quot;lat&quot;:       40733319,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.902Z&quot;,
&quot;lng&quot;:      -73995101,
&quot;id&quot;:             74,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973714,      40.764397 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;281 - Grand Army Plaza &amp; Central Park S&quot;,
&quot;idx&quot;:             75,
&quot;lat&quot;:       40764397,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.903Z&quot;,
&quot;lng&quot;:      -73973714,
&quot;id&quot;:             75,
&quot;free&quot;:             58,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.968341,      40.708272 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;282 - Kent Ave &amp; S 11 St&quot;,
&quot;idx&quot;:             76,
&quot;lat&quot;:       40708272,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.904Z&quot;,
&quot;lng&quot;:      -73968341,
&quot;id&quot;:             76,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002637,      40.739016 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;284 - Greenwich Ave &amp; 8 Ave&quot;,
&quot;idx&quot;:             77,
&quot;lat&quot;:       40739016,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.905Z&quot;,
&quot;lng&quot;:      -74002637,
&quot;id&quot;:             77,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990741,      40.734545 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             34,
&quot;name&quot;: &quot;285 - Broadway &amp; E 14 St&quot;,
&quot;idx&quot;:             78,
&quot;lat&quot;:       40734545,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.905Z&quot;,
&quot;lng&quot;:      -73990741,
&quot;id&quot;:             78,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.95881,      40.684568 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;289 - Monroe St &amp; Classon Ave&quot;,
&quot;idx&quot;:             79,
&quot;lat&quot;:       40684568,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.906Z&quot;,
&quot;lng&quot;:      -73958810,
&quot;id&quot;:             79,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.964784,      40.760202 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;290 - 2 Ave &amp; E 58 St&quot;,
&quot;idx&quot;:             80,
&quot;lat&quot;:       40760202,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.907Z&quot;,
&quot;lng&quot;:      -73964784,
&quot;id&quot;:             80,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984844,      40.713126 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;291 - Madison St &amp; Montgomery St&quot;,
&quot;idx&quot;:             81,
&quot;lat&quot;:       40713126,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.907Z&quot;,
&quot;lng&quot;:      -73984844,
&quot;id&quot;:             81,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990764,      40.730286 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;293 - Lafayette St &amp; E 8 St&quot;,
&quot;idx&quot;:             82,
&quot;lat&quot;:       40730286,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.908Z&quot;,
&quot;lng&quot;:      -73990764,
&quot;id&quot;:             82,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995721,      40.730493 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;294 - Washington Square E&quot;,
&quot;idx&quot;:             83,
&quot;lat&quot;:       40730493,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.909Z&quot;,
&quot;lng&quot;:      -73995721,
&quot;id&quot;:             83,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992939,      40.714066 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;295 - Pike St &amp; E Broadway&quot;,
&quot;idx&quot;:             84,
&quot;lat&quot;:       40714066,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.909Z&quot;,
&quot;lng&quot;:      -73992939,
&quot;id&quot;:             84,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997046,       40.71413 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;296 - Division St &amp; Bowery&quot;,
&quot;idx&quot;:             85,
&quot;lat&quot;:       40714130,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.91Z&quot;,
&quot;lng&quot;:      -73997046,
&quot;id&quot;:             85,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986923,      40.734232 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;297 - E 15 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             86,
&quot;lat&quot;:       40734232,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.911Z&quot;,
&quot;lng&quot;:      -73986923,
&quot;id&quot;:             86,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979677,      40.686832 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;298 - 3 Ave &amp; Schermerhorn St&quot;,
&quot;idx&quot;:             87,
&quot;lat&quot;:       40686832,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.911Z&quot;,
&quot;lng&quot;:      -73979677,
&quot;id&quot;:             87,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990214,      40.728145 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;300 - Shevchenko Pl &amp; E 6 St&quot;,
&quot;idx&quot;:             88,
&quot;lat&quot;:       40728145,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.912Z&quot;,
&quot;lng&quot;:      -73990214,
&quot;id&quot;:             88,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983687,      40.722174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;301 - E 2 St &amp; Avenue B&quot;,
&quot;idx&quot;:             89,
&quot;lat&quot;:       40722174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.913Z&quot;,
&quot;lng&quot;:      -73983687,
&quot;id&quot;:             89,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977931,      40.720828 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;302 - Avenue D &amp; E 3 St&quot;,
&quot;idx&quot;:             90,
&quot;lat&quot;:       40720828,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.913Z&quot;,
&quot;lng&quot;:      -73977931,
&quot;id&quot;:             90,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999496,      40.723627 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;303 - Mercer St &amp; Spring St&quot;,
&quot;idx&quot;:             91,
&quot;lat&quot;:       40723627,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.914Z&quot;,
&quot;lng&quot;:      -73999496,
&quot;id&quot;:             91,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.013617,      40.704633 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;304 - Broadway &amp; Battery Pl&quot;,
&quot;idx&quot;:             92,
&quot;lat&quot;:       40704633,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.915Z&quot;,
&quot;lng&quot;:      -74013617,
&quot;id&quot;:             92,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967244,      40.760957 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;305 - E 58 St &amp; 3 Ave&quot;,
&quot;idx&quot;:             93,
&quot;lat&quot;:       40760957,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.915Z&quot;,
&quot;lng&quot;:      -73967244,
&quot;id&quot;:             93,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -74.0053,      40.708235 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;306 - Cliff St &amp; Fulton St&quot;,
&quot;idx&quot;:             94,
&quot;lat&quot;:       40708235,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.916Z&quot;,
&quot;lng&quot;:      -74005300,
&quot;id&quot;:             94,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9899,      40.714274 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;307 - Canal St &amp; Rutgers St&quot;,
&quot;idx&quot;:             95,
&quot;lat&quot;:       40714274,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.917Z&quot;,
&quot;lng&quot;:      -73989900,
&quot;id&quot;:             95,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998511,      40.713079 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;308 - St James Pl &amp; Oliver St&quot;,
&quot;idx&quot;:             96,
&quot;lat&quot;:       40713079,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.917Z&quot;,
&quot;lng&quot;:      -73998511,
&quot;id&quot;:             96,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.013012,      40.714978 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;309 - Murray St &amp; West St&quot;,
&quot;idx&quot;:             97,
&quot;lat&quot;:       40714978,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.918Z&quot;,
&quot;lng&quot;:      -74013012,
&quot;id&quot;:             97,
&quot;free&quot;:             40,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989128,      40.689269 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;310 - State St &amp; Smith St&quot;,
&quot;idx&quot;:             98,
&quot;lat&quot;:       40689269,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.919Z&quot;,
&quot;lng&quot;:      -73989128,
&quot;id&quot;:             98,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98802,      40.717227 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;311 - Norfolk St &amp; Broome St&quot;,
&quot;idx&quot;:             99,
&quot;lat&quot;:       40717227,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.919Z&quot;,
&quot;lng&quot;:      -73988020,
&quot;id&quot;:             99,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989111,      40.722055 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;312 - Allen St &amp; E Houston St&quot;,
&quot;idx&quot;:            100,
&quot;lat&quot;:       40722055,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.92Z&quot;,
&quot;lng&quot;:      -73989111,
&quot;id&quot;:            100,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.96751,      40.696102 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;313 - Washington Ave &amp; Park Ave&quot;,
&quot;idx&quot;:            101,
&quot;lat&quot;:       40696102,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.921Z&quot;,
&quot;lng&quot;:      -73967510,
&quot;id&quot;:            101,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992159,      40.694246 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;314 - Montague St &amp; Clinton St&quot;,
&quot;idx&quot;:            102,
&quot;lat&quot;:       40694246,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.921Z&quot;,
&quot;lng&quot;:      -73992159,
&quot;id&quot;:            102,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006702,      40.703553 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             27,
&quot;name&quot;: &quot;315 - South St &amp; Gouverneur Ln&quot;,
&quot;idx&quot;:            103,
&quot;lat&quot;:       40703553,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.922Z&quot;,
&quot;lng&quot;:      -74006702,
&quot;id&quot;:            103,
&quot;free&quot;:              2,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006536,      40.709559 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;316 - Fulton St &amp; William St&quot;,
&quot;idx&quot;:            104,
&quot;lat&quot;:       40709559,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.923Z&quot;,
&quot;lng&quot;:      -74006536,
&quot;id&quot;:            104,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981854,      40.724537 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;317 - E 6 St &amp; Avenue B&quot;,
&quot;idx&quot;:            105,
&quot;lat&quot;:       40724537,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.924Z&quot;,
&quot;lng&quot;:      -73981854,
&quot;id&quot;:            105,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977987,      40.753201 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;318 - E 43 St &amp; Vanderbilt Ave&quot;,
&quot;idx&quot;:            106,
&quot;lat&quot;:       40753201,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.924Z&quot;,
&quot;lng&quot;:      -73977987,
&quot;id&quot;:            106,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009376,      40.713361 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;319 - Park Pl &amp; Church St&quot;,
&quot;idx&quot;:            107,
&quot;lat&quot;:       40713361,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.925Z&quot;,
&quot;lng&quot;:      -74009376,
&quot;id&quot;:            107,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005834,      40.717439 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;320 - Church St &amp; Leonard St&quot;,
&quot;idx&quot;:            108,
&quot;lat&quot;:       40717439,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.926Z&quot;,
&quot;lng&quot;:      -74005834,
&quot;id&quot;:            108,
&quot;free&quot;:             39,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989717,      40.699917 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;321 - Cadman Plaza E &amp; Red Cross Pl&quot;,
&quot;idx&quot;:            109,
&quot;lat&quot;:       40699917,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.926Z&quot;,
&quot;lng&quot;:      -73989717,
&quot;id&quot;:            109,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991218,      40.696192 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;322 - Clinton St &amp; Tillary St&quot;,
&quot;idx&quot;:            110,
&quot;lat&quot;:       40696192,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.927Z&quot;,
&quot;lng&quot;:      -73991218,
&quot;id&quot;:            110,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986317,      40.692361 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;323 - Lawrence St &amp; Willoughby St&quot;,
&quot;idx&quot;:            111,
&quot;lat&quot;:       40692361,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.928Z&quot;,
&quot;lng&quot;:      -73986317,
&quot;id&quot;:            111,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981013,      40.689888 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;324 - DeKalb Ave &amp; Hudson Ave&quot;,
&quot;idx&quot;:            112,
&quot;lat&quot;:       40689888,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.928Z&quot;,
&quot;lng&quot;:      -73981013,
&quot;id&quot;:            112,
&quot;free&quot;:             39,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984737,      40.736245 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;325 - E 19 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            113,
&quot;lat&quot;:       40736245,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.929Z&quot;,
&quot;lng&quot;:      -73984737,
&quot;id&quot;:            113,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984267,      40.729538 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;326 - E 11 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            114,
&quot;lat&quot;:       40729538,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.93Z&quot;,
&quot;lng&quot;:      -73984267,
&quot;id&quot;:            114,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.016583,      40.715337 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;327 - Vesey Pl &amp; River Terrace&quot;,
&quot;idx&quot;:            115,
&quot;lat&quot;:       40715337,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.93Z&quot;,
&quot;lng&quot;:      -74016583,
&quot;id&quot;:            115,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.009659,      40.724055 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;328 - Watts St &amp; Greenwich St&quot;,
&quot;idx&quot;:            116,
&quot;lat&quot;:       40724055,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.931Z&quot;,
&quot;lng&quot;:      -74009659,
&quot;id&quot;:            116,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010206,      40.720434 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;329 - Greenwich St &amp; N Moore St&quot;,
&quot;idx&quot;:            117,
&quot;lat&quot;:       40720434,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.932Z&quot;,
&quot;lng&quot;:      -74010206,
&quot;id&quot;:            117,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005627,      40.714504 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;330 - Reade St &amp; Broadway&quot;,
&quot;idx&quot;:            118,
&quot;lat&quot;:       40714504,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.932Z&quot;,
&quot;lng&quot;:      -74005627,
&quot;id&quot;:            118,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99193,      40.711731 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;331 - Pike St &amp; Monroe St&quot;,
&quot;idx&quot;:            119,
&quot;lat&quot;:       40711731,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.933Z&quot;,
&quot;lng&quot;:      -73991930,
&quot;id&quot;:            119,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979481,      40.712199 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;332 - Cherry St&quot;,
&quot;idx&quot;:            120,
&quot;lat&quot;:       40712199,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.934Z&quot;,
&quot;lng&quot;:      -73979481,
&quot;id&quot;:            120,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997262,      40.742387 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;334 - W 20 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            121,
&quot;lat&quot;:       40742387,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.935Z&quot;,
&quot;lng&quot;:      -73997262,
&quot;id&quot;:            121,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994046,      40.729039 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;335 - Washington Pl &amp; Broadway&quot;,
&quot;idx&quot;:            122,
&quot;lat&quot;:       40729039,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.936Z&quot;,
&quot;lng&quot;:      -73994046,
&quot;id&quot;:            122,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008386,      40.703799 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;337 - Old Slip &amp; Front St&quot;,
&quot;idx&quot;:            123,
&quot;lat&quot;:       40703799,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.937Z&quot;,
&quot;lng&quot;:      -74008386,
&quot;id&quot;:            123,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974224,      40.725806 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;339 - Avenue D &amp; E 12 St&quot;,
&quot;idx&quot;:            124,
&quot;lat&quot;:       40725806,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.938Z&quot;,
&quot;lng&quot;:      -73974224,
&quot;id&quot;:            124,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987763,       40.71269 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;340 - Madison St &amp; Clinton St&quot;,
&quot;idx&quot;:            125,
&quot;lat&quot;:       40712690,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.938Z&quot;,
&quot;lng&quot;:      -73987763,
&quot;id&quot;:            125,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976289,      40.717821 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;341 - Stanton St &amp; Mangin St&quot;,
&quot;idx&quot;:            126,
&quot;lat&quot;:       40717821,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.939Z&quot;,
&quot;lng&quot;:      -73976289,
&quot;id&quot;:            126,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980165,      40.717399 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;342 - Columbia St &amp; Rivington St&quot;,
&quot;idx&quot;:            127,
&quot;lat&quot;:       40717399,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.94Z&quot;,
&quot;lng&quot;:      -73980165,
&quot;id&quot;:            127,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969868,       40.69794 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;343 - Clinton Ave &amp; Flushing Ave&quot;,
&quot;idx&quot;:            128,
&quot;lat&quot;:       40697940,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.94Z&quot;,
&quot;lng&quot;:      -73969868,
&quot;id&quot;:            128,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.953809,      40.685144 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;344 - Monroe St &amp; Bedford Ave&quot;,
&quot;idx&quot;:            129,
&quot;lat&quot;:       40685144,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.941Z&quot;,
&quot;lng&quot;:      -73953809,
&quot;id&quot;:            129,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997043,      40.736494 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;345 - W 13 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            130,
&quot;lat&quot;:       40736494,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.942Z&quot;,
&quot;lng&quot;:      -73997043,
&quot;id&quot;:            130,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00618,      40.736528 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;346 - Bank St &amp; Hudson St&quot;,
&quot;idx&quot;:            131,
&quot;lat&quot;:       40736528,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.942Z&quot;,
&quot;lng&quot;:      -74006180,
&quot;id&quot;:            131,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007488,      40.728738 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;347 - W Houston St &amp; Hudson St&quot;,
&quot;idx&quot;:            132,
&quot;lat&quot;:       40728738,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.944Z&quot;,
&quot;lng&quot;:      -74007488,
&quot;id&quot;:            132,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001547,      40.724909 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             36,
&quot;name&quot;: &quot;348 - W Broadway &amp; Spring St&quot;,
&quot;idx&quot;:            133,
&quot;lat&quot;:       40724909,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.945Z&quot;,
&quot;lng&quot;:      -74001547,
&quot;id&quot;:            133,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983298,      40.718502 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;349 - Rivington St &amp; Ridge St&quot;,
&quot;idx&quot;:            134,
&quot;lat&quot;:       40718502,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.946Z&quot;,
&quot;lng&quot;:      -73983298,
&quot;id&quot;:            134,
&quot;free&quot;:             18,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987029,      40.715595 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;350 - Clinton St &amp; Grand St&quot;,
&quot;idx&quot;:            135,
&quot;lat&quot;:       40715595,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.946Z&quot;,
&quot;lng&quot;:      -73987029,
&quot;id&quot;:            135,
&quot;free&quot;:             10,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.006125,      40.705309 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;351 - Front St &amp; Maiden Ln&quot;,
&quot;idx&quot;:            136,
&quot;lat&quot;:       40705309,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.947Z&quot;,
&quot;lng&quot;:      -74006125,
&quot;id&quot;:            136,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977224,      40.763406 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;352 - W 56 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            137,
&quot;lat&quot;:       40763406,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.948Z&quot;,
&quot;lng&quot;:      -73977224,
&quot;id&quot;:            137,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974314,      40.685395 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;353 - S Portland Ave &amp; Hanson Pl&quot;,
&quot;idx&quot;:            138,
&quot;lat&quot;:       40685395,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.948Z&quot;,
&quot;lng&quot;:      -73974314,
&quot;id&quot;:            138,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.962235,      40.693631 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;354 - Emerson Pl &amp; Myrtle Ave&quot;,
&quot;idx&quot;:            139,
&quot;lat&quot;:       40693631,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.949Z&quot;,
&quot;lng&quot;:      -73962235,
&quot;id&quot;:            139,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999743,      40.716021 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             33,
&quot;name&quot;: &quot;355 - Bayard St &amp; Baxter St&quot;,
&quot;idx&quot;:            140,
&quot;lat&quot;:       40716021,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.95Z&quot;,
&quot;lng&quot;:      -73999743,
&quot;id&quot;:            140,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982612,      40.716226 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;356 - Bialystoker Pl &amp; Delancey St&quot;,
&quot;idx&quot;:            141,
&quot;lat&quot;:       40716226,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.95Z&quot;,
&quot;lng&quot;:      -73982612,
&quot;id&quot;:            141,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99158,      40.732617 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;357 - E 11 St &amp; Broadway&quot;,
&quot;idx&quot;:            142,
&quot;lat&quot;:       40732617,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.951Z&quot;,
&quot;lng&quot;:      -73991580,
&quot;id&quot;:            142,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007113,      40.732915 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             35,
&quot;name&quot;: &quot;358 - Christopher St &amp; Greenwich St&quot;,
&quot;idx&quot;:            143,
&quot;lat&quot;:       40732915,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.952Z&quot;,
&quot;lng&quot;:      -74007113,
&quot;id&quot;:            143,
&quot;free&quot;:              1,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974986,      40.755102 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;359 - E 47 St &amp; Park Av&quot;,
&quot;idx&quot;:            144,
&quot;lat&quot;:       40755102,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.952Z&quot;,
&quot;lng&quot;:      -73974986,
&quot;id&quot;:            144,
&quot;free&quot;:             53,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008873,      40.707179 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;360 - William St &amp; Pine St&quot;,
&quot;idx&quot;:            145,
&quot;lat&quot;:       40707179,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.953Z&quot;,
&quot;lng&quot;:      -74008873,
&quot;id&quot;:            145,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991907,      40.716058 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;361 - Allen St &amp; Hester St&quot;,
&quot;idx&quot;:            146,
&quot;lat&quot;:       40716058,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.954Z&quot;,
&quot;lng&quot;:      -73991907,
&quot;id&quot;:            146,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987535,      40.751726 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;362 - Broadway &amp; W 37 St&quot;,
&quot;idx&quot;:            147,
&quot;lat&quot;:       40751726,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.954Z&quot;,
&quot;lng&quot;:      -73987535,
&quot;id&quot;:            147,
&quot;free&quot;:             57,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.017134,      40.708346 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;363 - West Thames St&quot;,
&quot;idx&quot;:            148,
&quot;lat&quot;:       40708346,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.955Z&quot;,
&quot;lng&quot;:      -74017134,
&quot;id&quot;:            148,
&quot;free&quot;:             34,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.960238,      40.689004 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;364 - Lafayette Ave &amp; Classon Ave&quot;,
&quot;idx&quot;:            149,
&quot;lat&quot;:       40689004,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.956Z&quot;,
&quot;lng&quot;:      -73960238,
&quot;id&quot;:            149,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.961458,      40.682231 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;365 - Fulton St &amp; Grand Ave&quot;,
&quot;idx&quot;:            150,
&quot;lat&quot;:       40682231,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.956Z&quot;,
&quot;lng&quot;:      -73961458,
&quot;id&quot;:            150,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.968896,      40.693261 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;366 - Clinton Ave &amp; Myrtle Ave&quot;,
&quot;idx&quot;:            151,
&quot;lat&quot;:       40693261,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.957Z&quot;,
&quot;lng&quot;:      -73968896,
&quot;id&quot;:            151,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.970694,       40.75828 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;367 - E 53 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:            152,
&quot;lat&quot;:       40758280,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.958Z&quot;,
&quot;lng&quot;:      -73970694,
&quot;id&quot;:            152,
&quot;free&quot;:             33,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002149,      40.730385 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;368 - Carmine St &amp; 6 Ave&quot;,
&quot;idx&quot;:            153,
&quot;lat&quot;:       40730385,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.958Z&quot;,
&quot;lng&quot;:      -74002149,
&quot;id&quot;:            153,
&quot;free&quot;:             31,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.000263,      40.732241 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;369 - Washington Pl &amp; 6 Ave&quot;,
&quot;idx&quot;:            154,
&quot;lat&quot;:       40732241,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.959Z&quot;,
&quot;lng&quot;:      -74000263,
&quot;id&quot;:            154,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.958089,      40.694528 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;372 - Franklin Ave &amp; Myrtle Ave&quot;,
&quot;idx&quot;:            155,
&quot;lat&quot;:       40694528,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.96Z&quot;,
&quot;lng&quot;:      -73958089,
&quot;id&quot;:            155,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.953819,      40.693317 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;373 - Willoughby Ave &amp; Walworth St&quot;,
&quot;idx&quot;:            156,
&quot;lat&quot;:       40693317,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.96Z&quot;,
&quot;lng&quot;:      -73953819,
&quot;id&quot;:            156,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99695,      40.726794 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;375 - Mercer St &amp; Bleecker St&quot;,
&quot;idx&quot;:            157,
&quot;lat&quot;:       40726794,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.961Z&quot;,
&quot;lng&quot;:      -73996950,
&quot;id&quot;:            157,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007221,      40.708621 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;376 - John St &amp; William St&quot;,
&quot;idx&quot;:            158,
&quot;lat&quot;:       40708621,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.962Z&quot;,
&quot;lng&quot;:      -74007221,
&quot;id&quot;:            158,
&quot;free&quot;:             41,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005664,      40.722437 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;377 - 6 Ave &amp; Canal St&quot;,
&quot;idx&quot;:            159,
&quot;lat&quot;:       40722437,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.962Z&quot;,
&quot;lng&quot;:      -74005664,
&quot;id&quot;:            159,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9916,      40.749156 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             35,
&quot;name&quot;: &quot;379 - W 31 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            160,
&quot;lat&quot;:       40749156,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.963Z&quot;,
&quot;lng&quot;:      -73991600,
&quot;id&quot;:            160,
&quot;free&quot;:              2,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002938,      40.734011 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;380 - W 4 St &amp; 7 Ave S&quot;,
&quot;idx&quot;:            161,
&quot;lat&quot;:       40734011,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.964Z&quot;,
&quot;lng&quot;:      -74002938,
&quot;id&quot;:            161,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992005,      40.734926 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;382 - University Pl &amp; E 14 St&quot;,
&quot;idx&quot;:            162,
&quot;lat&quot;:       40734926,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.964Z&quot;,
&quot;lng&quot;:      -73992005,
&quot;id&quot;:            162,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.000271,      40.735238 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             34,
&quot;name&quot;: &quot;383 - Greenwich Ave &amp; Charles St&quot;,
&quot;idx&quot;:            163,
&quot;lat&quot;:       40735238,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.965Z&quot;,
&quot;lng&quot;:      -74000271,
&quot;id&quot;:            163,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965964,      40.683178 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;384 - Fulton St &amp; Waverly Ave&quot;,
&quot;idx&quot;:            164,
&quot;lat&quot;:       40683178,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.966Z&quot;,
&quot;lng&quot;:      -73965964,
&quot;id&quot;:            164,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.966033,      40.757973 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;385 - E 55 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            165,
&quot;lat&quot;:       40757973,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.966Z&quot;,
&quot;lng&quot;:      -73966033,
&quot;id&quot;:            165,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002344,      40.714948 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;386 - Centre St &amp; Worth St&quot;,
&quot;idx&quot;:            166,
&quot;lat&quot;:       40714948,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.967Z&quot;,
&quot;lng&quot;:      -74002344,
&quot;id&quot;:            166,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004607,      40.712732 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;387 - Centre St &amp; Chambers St&quot;,
&quot;idx&quot;:            167,
&quot;lat&quot;:       40712732,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.968Z&quot;,
&quot;lng&quot;:      -74004607,
&quot;id&quot;:            167,
&quot;free&quot;:             37,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00295,      40.749717 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;388 - W 26 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            168,
&quot;lat&quot;:       40749717,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.968Z&quot;,
&quot;lng&quot;:      -74002950,
&quot;id&quot;:            168,
&quot;free&quot;:             34,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.96525,      40.710445 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;389 - Broadway &amp; Berry St&quot;,
&quot;idx&quot;:            169,
&quot;lat&quot;:       40710445,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.969Z&quot;,
&quot;lng&quot;:      -73965250,
&quot;id&quot;:            169,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984284,      40.692215 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;390 - Duffield St &amp; Willoughby St&quot;,
&quot;idx&quot;:            170,
&quot;lat&quot;:       40692215,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.97Z&quot;,
&quot;lng&quot;:      -73984284,
&quot;id&quot;:            170,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993445,      40.697601 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;391 - Clark St &amp; Henry St&quot;,
&quot;idx&quot;:            171,
&quot;lat&quot;:       40697601,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.97Z&quot;,
&quot;lng&quot;:      -73993445,
&quot;id&quot;:            171,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987167,      40.695065 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;392 - Jay St &amp; Tech Pl&quot;,
&quot;idx&quot;:            172,
&quot;lat&quot;:       40695065,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.971Z&quot;,
&quot;lng&quot;:      -73987167,
&quot;id&quot;:            172,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979954,      40.722992 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;393 - E 5 St &amp; Avenue C&quot;,
&quot;idx&quot;:            173,
&quot;lat&quot;:       40722992,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.972Z&quot;,
&quot;lng&quot;:      -73979954,
&quot;id&quot;:            173,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977687,      40.725213 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;394 - E 9 St &amp; Avenue C&quot;,
&quot;idx&quot;:            174,
&quot;lat&quot;:       40725213,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.972Z&quot;,
&quot;lng&quot;:      -73977687,
&quot;id&quot;:            174,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984106,       40.68807 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;395 - Bond St &amp; Schermerhorn St&quot;,
&quot;idx&quot;:            175,
&quot;lat&quot;:       40688070,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.973Z&quot;,
&quot;lng&quot;:      -73984106,
&quot;id&quot;:            175,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.955768,      40.680342 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;396 - Lefferts Pl &amp; Franklin Ave&quot;,
&quot;idx&quot;:            176,
&quot;lat&quot;:       40680342,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.974Z&quot;,
&quot;lng&quot;:      -73955768,
&quot;id&quot;:            176,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969222,      40.684157 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;397 - Fulton St &amp; Clermont Ave&quot;,
&quot;idx&quot;:            177,
&quot;lat&quot;:       40684157,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.975Z&quot;,
&quot;lng&quot;:      -73969222,
&quot;id&quot;:            177,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999978,      40.691651 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;398 - Atlantic Ave &amp; Furman St&quot;,
&quot;idx&quot;:            178,
&quot;lat&quot;:       40691651,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.975Z&quot;,
&quot;lng&quot;:      -73999978,
&quot;id&quot;:            178,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.964762,      40.688515 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;399 - Lafayette Ave &amp; St James Pl&quot;,
&quot;idx&quot;:            179,
&quot;lat&quot;:       40688515,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.978Z&quot;,
&quot;lng&quot;:      -73964762,
&quot;id&quot;:            179,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98178,       40.71926 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;400 - Pitt St &amp; Stanton St&quot;,
&quot;idx&quot;:            180,
&quot;lat&quot;:       40719260,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.979Z&quot;,
&quot;lng&quot;:      -73981780,
&quot;id&quot;:            180,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989978,      40.720195 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;401 - Allen St &amp; Rivington St&quot;,
&quot;idx&quot;:            181,
&quot;lat&quot;:       40720195,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.98Z&quot;,
&quot;lng&quot;:      -73989978,
&quot;id&quot;:            181,
&quot;free&quot;:             15,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989551,      40.740343 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;402 - Broadway &amp; E 22 St&quot;,
&quot;idx&quot;:            182,
&quot;lat&quot;:       40740343,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.981Z&quot;,
&quot;lng&quot;:      -73989551,
&quot;id&quot;:            182,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990696,      40.725028 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;403 - E 2 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            183,
&quot;lat&quot;:       40725028,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.981Z&quot;,
&quot;lng&quot;:      -73990696,
&quot;id&quot;:            183,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005508,      40.740582 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;404 - 9 Ave &amp; W 14 St&quot;,
&quot;idx&quot;:            184,
&quot;lat&quot;:       40740582,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.982Z&quot;,
&quot;lng&quot;:      -74005508,
&quot;id&quot;:            184,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.008119,      40.739323 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;405 - Washington St &amp; Gansevoort St&quot;,
&quot;idx&quot;:            185,
&quot;lat&quot;:       40739323,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.983Z&quot;,
&quot;lng&quot;:      -74008119,
&quot;id&quot;:            185,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99595,      40.695128 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;406 - Hicks St &amp; Montague St&quot;,
&quot;idx&quot;:            186,
&quot;lat&quot;:       40695128,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.983Z&quot;,
&quot;lng&quot;:      -73995950,
&quot;id&quot;:            186,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991454,      40.700469 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;407 - Henry St &amp; Poplar St&quot;,
&quot;idx&quot;:            187,
&quot;lat&quot;:       40700469,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.984Z&quot;,
&quot;lng&quot;:      -73991454,
&quot;id&quot;:            187,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994003,      40.710762 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;408 - Market St &amp; Cherry St&quot;,
&quot;idx&quot;:            188,
&quot;lat&quot;:       40710762,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.985Z&quot;,
&quot;lng&quot;:      -73994003,
&quot;id&quot;:            188,
&quot;free&quot;:             15,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.956431,      40.690649 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;409 - DeKalb Ave &amp; Skillman St&quot;,
&quot;idx&quot;:            189,
&quot;lat&quot;:       40690649,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.986Z&quot;,
&quot;lng&quot;:      -73956431,
&quot;id&quot;:            189,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.985179,      40.720664 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;410 - Suffolk St &amp; Stanton St&quot;,
&quot;idx&quot;:            190,
&quot;lat&quot;:       40720664,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.987Z&quot;,
&quot;lng&quot;:      -73985179,
&quot;id&quot;:            190,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976687,       40.72228 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;411 - E 6 St &amp; Avenue D&quot;,
&quot;idx&quot;:            191,
&quot;lat&quot;:       40722280,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.987Z&quot;,
&quot;lng&quot;:      -73976687,
&quot;id&quot;:            191,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994223,      40.715815 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;412 - Forsyth St &amp; Canal St&quot;,
&quot;idx&quot;:            192,
&quot;lat&quot;:       40715815,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.999Z&quot;,
&quot;lng&quot;:      -73994223,
&quot;id&quot;:            192,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987657,      40.702818 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;414 - Pearl St &amp; Anchorage Pl&quot;,
&quot;idx&quot;:            193,
&quot;lat&quot;:       40702818,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07Z&quot;,
&quot;lng&quot;:      -73987657,
&quot;id&quot;:            193,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00926,      40.704717 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;415 - Pearl St &amp; Hanover Square&quot;,
&quot;idx&quot;:            194,
&quot;lat&quot;:       40704717,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07Z&quot;,
&quot;lng&quot;:      -74009260,
&quot;id&quot;:            194,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972651,      40.687534 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;416 - Cumberland St &amp; Lafayette Ave&quot;,
&quot;idx&quot;:            195,
&quot;lat&quot;:       40687534,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.001Z&quot;,
&quot;lng&quot;:      -73972651,
&quot;id&quot;:            195,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.010202,      40.712912 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;417 - Barclay St &amp; Church St&quot;,
&quot;idx&quot;:            196,
&quot;lat&quot;:       40712912,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.002Z&quot;,
&quot;lng&quot;:      -74010202,
&quot;id&quot;:            196,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982578,       40.70224 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;418 - Front St &amp; Gold St&quot;,
&quot;idx&quot;:            197,
&quot;lat&quot;:       40702240,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.002Z&quot;,
&quot;lng&quot;:      -73982578,
&quot;id&quot;:            197,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973555,      40.695807 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;419 - Carlton Ave &amp; Park Ave&quot;,
&quot;idx&quot;:            198,
&quot;lat&quot;:       40695807,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.003Z&quot;,
&quot;lng&quot;:      -73973555,
&quot;id&quot;:            198,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969689,      40.687644 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              7,
&quot;name&quot;: &quot;420 - Clermont Ave &amp; Lafayette Ave&quot;,
&quot;idx&quot;:            199,
&quot;lat&quot;:       40687644,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.004Z&quot;,
&quot;lng&quot;:      -73969689,
&quot;id&quot;:            199,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971296,      40.695733 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;421 - Clermont Ave &amp; Park Ave&quot;,
&quot;idx&quot;:            200,
&quot;lat&quot;:       40695733,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.004Z&quot;,
&quot;lng&quot;:      -73971296,
&quot;id&quot;:            200,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988038,      40.770513 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;422 - W 59 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            201,
&quot;lat&quot;:       40770513,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.01Z&quot;,
&quot;lng&quot;:      -73988038,
&quot;id&quot;:            201,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986905,      40.765849 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;423 - W 54 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            202,
&quot;lat&quot;:       40765849,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.011Z&quot;,
&quot;lng&quot;:      -73986905,
&quot;id&quot;:            202,
&quot;free&quot;:             33,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.01322,      40.717548 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;426 - West St &amp; Chambers St&quot;,
&quot;idx&quot;:            203,
&quot;lat&quot;:       40717548,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.012Z&quot;,
&quot;lng&quot;:      -74013220,
&quot;id&quot;:            203,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.01427,      40.702515 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             21,
&quot;name&quot;: &quot;427 - State St&quot;,
&quot;idx&quot;:            204,
&quot;lat&quot;:       40702515,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.012Z&quot;,
&quot;lng&quot;:      -74014270,
&quot;id&quot;:            204,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987834,      40.724677 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;428 - E 3 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            205,
&quot;lat&quot;:       40724677,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.013Z&quot;,
&quot;lng&quot;:      -73987834,
&quot;id&quot;:            205,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986569,      40.701485 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;430 - York St &amp; Jay St&quot;,
&quot;idx&quot;:            206,
&quot;lat&quot;:       40701485,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.014Z&quot;,
&quot;lng&quot;:      -73986569,
&quot;id&quot;:            206,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982634,      40.688646 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;431 - Hanover Pl &amp; Livingston St&quot;,
&quot;idx&quot;:            207,
&quot;lat&quot;:       40688646,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.014Z&quot;,
&quot;lng&quot;:      -73982634,
&quot;id&quot;:            207,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983798,      40.726217 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;432 - E 7 St &amp; Avenue A&quot;,
&quot;idx&quot;:            208,
&quot;lat&quot;:       40726217,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.015Z&quot;,
&quot;lng&quot;:      -73983798,
&quot;id&quot;:            208,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980572,      40.729553 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;433 - E 13 St &amp; Avenue A&quot;,
&quot;idx&quot;:            209,
&quot;lat&quot;:       40729553,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.016Z&quot;,
&quot;lng&quot;:      -73980572,
&quot;id&quot;:            209,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.003664,      40.743174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             25,
&quot;name&quot;: &quot;434 - 9 Ave &amp; W 18 St&quot;,
&quot;idx&quot;:            210,
&quot;lat&quot;:       40743174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.017Z&quot;,
&quot;lng&quot;:      -74003664,
&quot;id&quot;:            210,
&quot;free&quot;:              1,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994155,      40.741739 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;435 - W 21 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            211,
&quot;lat&quot;:       40741739,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.017Z&quot;,
&quot;lng&quot;:      -73994155,
&quot;id&quot;:            211,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.95399,      40.682165 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;436 - Hancock St &amp; Bedford Ave&quot;,
&quot;idx&quot;:            212,
&quot;lat&quot;:       40682165,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.018Z&quot;,
&quot;lng&quot;:      -73953990,
&quot;id&quot;:            212,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.950047,      40.680983 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;437 - Macon St &amp; Nostrand Ave&quot;,
&quot;idx&quot;:            213,
&quot;lat&quot;:       40680983,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.019Z&quot;,
&quot;lng&quot;:      -73950047,
&quot;id&quot;:            213,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.985649,      40.727791 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;438 - St Marks Pl &amp; 1 Ave&quot;,
&quot;idx&quot;:            214,
&quot;lat&quot;:       40727791,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.019Z&quot;,
&quot;lng&quot;:      -73985649,
&quot;id&quot;:            214,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98978,       40.72628 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;439 - E 4 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            215,
&quot;lat&quot;:       40726280,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.02Z&quot;,
&quot;lng&quot;:      -73989780,
&quot;id&quot;:            215,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972826,      40.752554 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;440 - E 45 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            216,
&quot;lat&quot;:       40752554,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.021Z&quot;,
&quot;lng&quot;:      -73972826,
&quot;id&quot;:            216,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967416,      40.756014 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;441 - E 52 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            217,
&quot;lat&quot;:       40756014,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.021Z&quot;,
&quot;lng&quot;:      -73967416,
&quot;id&quot;:            217,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993915,      40.746647 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;442 - W 27 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            218,
&quot;lat&quot;:       40746647,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.022Z&quot;,
&quot;lng&quot;:      -73993915,
&quot;id&quot;:            218,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.964089,       40.70853 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;443 - Bedford Ave &amp; S 9 St&quot;,
&quot;idx&quot;:            219,
&quot;lat&quot;:       40708530,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.023Z&quot;,
&quot;lng&quot;:      -73964089,
&quot;id&quot;:            219,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98915,      40.742354 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;444 - Broadway &amp; W 24 St&quot;,
&quot;idx&quot;:            220,
&quot;lat&quot;:       40742354,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.023Z&quot;,
&quot;lng&quot;:      -73989150,
&quot;id&quot;:            220,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98142,      40.727407 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             42,
&quot;name&quot;: &quot;445 - E 10 St &amp; Avenue A&quot;,
&quot;idx&quot;:            221,
&quot;lat&quot;:       40727407,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.031Z&quot;,
&quot;lng&quot;:      -73981420,
&quot;id&quot;:            221,
&quot;free&quot;:              0,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.995298,      40.744876 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;446 - W 24 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            222,
&quot;lat&quot;:       40744876,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.031Z&quot;,
&quot;lng&quot;:      -73995298,
&quot;id&quot;:            222,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.985161,      40.763707 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;447 - 8 Ave &amp; W 52 St&quot;,
&quot;idx&quot;:            223,
&quot;lat&quot;:       40763707,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.032Z&quot;,
&quot;lng&quot;:      -73985161,
&quot;id&quot;:            223,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [       -73.9979,      40.756603 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;448 - W 37 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            224,
&quot;lat&quot;:       40756603,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.033Z&quot;,
&quot;lng&quot;:      -73997900,
&quot;id&quot;:            224,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987894,      40.764618 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;449 - W 52 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            225,
&quot;lat&quot;:       40764618,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.033Z&quot;,
&quot;lng&quot;:      -73987894,
&quot;id&quot;:            225,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987882,      40.762272 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;450 - W 49 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            226,
&quot;lat&quot;:       40762272,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.034Z&quot;,
&quot;lng&quot;:      -73987882,
&quot;id&quot;:            226,
&quot;free&quot;:             53,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999153,      40.744751 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;453 - W 22 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            227,
&quot;lat&quot;:       40744751,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.035Z&quot;,
&quot;lng&quot;:      -73999153,
&quot;id&quot;:            227,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965929,      40.754557 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;454 - E 51 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            228,
&quot;lat&quot;:       40754557,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.035Z&quot;,
&quot;lng&quot;:      -73965929,
&quot;id&quot;:            228,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.969053,      40.750019 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;455 - 1 Ave &amp; E 44 St&quot;,
&quot;idx&quot;:            229,
&quot;lat&quot;:       40750019,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.036Z&quot;,
&quot;lng&quot;:      -73969053,
&quot;id&quot;:            229,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974023,       40.75971 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;456 - E 53 St &amp; Madison Ave&quot;,
&quot;idx&quot;:            230,
&quot;lat&quot;:       40759710,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.037Z&quot;,
&quot;lng&quot;:      -73974023,
&quot;id&quot;:            230,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981693,      40.766953 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;457 - Broadway &amp; W 58 St&quot;,
&quot;idx&quot;:            231,
&quot;lat&quot;:       40766953,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.037Z&quot;,
&quot;lng&quot;:      -73981693,
&quot;id&quot;:            231,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.005226,      40.751396 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;458 - 11 Ave &amp; W 27 St&quot;,
&quot;idx&quot;:            232,
&quot;lat&quot;:       40751396,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.038Z&quot;,
&quot;lng&quot;:      -74005226,
&quot;id&quot;:            232,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.007756,      40.746745 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;459 - W 20 St &amp; 11 Ave&quot;,
&quot;idx&quot;:            233,
&quot;lat&quot;:       40746745,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.039Z&quot;,
&quot;lng&quot;:      -74007756,
&quot;id&quot;:            233,
&quot;free&quot;:             34,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.965902,      40.712858 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;460 - S 4 St &amp; Wythe Ave&quot;,
&quot;idx&quot;:            234,
&quot;lat&quot;:       40712858,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.039Z&quot;,
&quot;lng&quot;:      -73965902,
&quot;id&quot;:            234,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98205,      40.735876 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;461 - E 20 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            235,
&quot;lat&quot;:       40735876,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.04Z&quot;,
&quot;lng&quot;:      -73982050,
&quot;id&quot;:            235,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004518,      40.746919 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;462 - W 22 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            236,
&quot;lat&quot;:       40746919,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.041Z&quot;,
&quot;lng&quot;:      -74004518,
&quot;id&quot;:            236,
&quot;free&quot;:             34,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004431,      40.742065 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;463 - 9 Ave &amp; W 16 St&quot;,
&quot;idx&quot;:            237,
&quot;lat&quot;:       40742065,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.041Z&quot;,
&quot;lng&quot;:      -74004431,
&quot;id&quot;:            237,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967596,      40.759345 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;464 - E 56 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            238,
&quot;lat&quot;:       40759345,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.042Z&quot;,
&quot;lng&quot;:      -73967596,
&quot;id&quot;:            238,
&quot;free&quot;:             27,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98658,      40.755135 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;465 - Broadway &amp; W 41 St&quot;,
&quot;idx&quot;:            239,
&quot;lat&quot;:       40755135,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.046Z&quot;,
&quot;lng&quot;:      -73986580,
&quot;id&quot;:            239,
&quot;free&quot;:             39,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991448,      40.743954 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;466 - W 25 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            240,
&quot;lat&quot;:       40743954,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.047Z&quot;,
&quot;lng&quot;:      -73991448,
&quot;id&quot;:            240,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978951,      40.683124 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;467 - Dean St &amp; 4 Ave&quot;,
&quot;idx&quot;:            241,
&quot;lat&quot;:       40683124,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.048Z&quot;,
&quot;lng&quot;:      -73978951,
&quot;id&quot;:            241,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981923,      40.765265 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;468 - Broadway &amp; W 55 St&quot;,
&quot;idx&quot;:            242,
&quot;lat&quot;:       40765265,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.049Z&quot;,
&quot;lng&quot;:      -73981923,
&quot;id&quot;:            242,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982681,       40.76344 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;469 - Broadway &amp; W 53 St&quot;,
&quot;idx&quot;:            243,
&quot;lat&quot;:       40763440,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.049Z&quot;,
&quot;lng&quot;:      -73982681,
&quot;id&quot;:            243,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -74.00004,      40.743453 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;470 - W 20 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            244,
&quot;lat&quot;:       40743453,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.05Z&quot;,
&quot;lng&quot;:      -74000040,
&quot;id&quot;:            244,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.956981,      40.712868 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             26,
&quot;name&quot;: &quot;471 - Grand St &amp; Havemeyer St&quot;,
&quot;idx&quot;:            245,
&quot;lat&quot;:       40712868,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.051Z&quot;,
&quot;lng&quot;:      -73956981,
&quot;id&quot;:            245,
&quot;free&quot;:              1,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981948,      40.745712 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              3,
&quot;name&quot;: &quot;472 - E 32 St &amp; Park Ave&quot;,
&quot;idx&quot;:            246,
&quot;lat&quot;:       40745712,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.051Z&quot;,
&quot;lng&quot;:      -73981948,
&quot;id&quot;:            246,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991925,        40.7211 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;473 - Rivington St &amp; Chrystie St&quot;,
&quot;idx&quot;:            247,
&quot;lat&quot;:       40721100,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.052Z&quot;,
&quot;lng&quot;:      -73991925,
&quot;id&quot;:            247,
&quot;free&quot;:             28,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98683,      40.745167 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;474 - 5 Ave &amp; E 29 St&quot;,
&quot;idx&quot;:            248,
&quot;lat&quot;:       40745167,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.053Z&quot;,
&quot;lng&quot;:      -73986830,
&quot;id&quot;:            248,
&quot;free&quot;:             41,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987585,      40.735242 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;475 - E 16 St &amp; Irving Pl&quot;,
&quot;idx&quot;:            249,
&quot;lat&quot;:       40735242,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.053Z&quot;,
&quot;lng&quot;:      -73987585,
&quot;id&quot;:            249,
&quot;free&quot;:             14,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.97966,      40.743943 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             43,
&quot;name&quot;: &quot;476 - E 31 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            250,
&quot;lat&quot;:       40743943,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.054Z&quot;,
&quot;lng&quot;:      -73979660,
&quot;id&quot;:            250,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990026,      40.756405 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             42,
&quot;name&quot;: &quot;477 - W 41 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            251,
&quot;lat&quot;:       40756405,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.055Z&quot;,
&quot;lng&quot;:      -73990026,
&quot;id&quot;:            251,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998842,        40.7603 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;478 - 11 Ave &amp; W 41 St&quot;,
&quot;idx&quot;:            252,
&quot;lat&quot;:       40760300,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.055Z&quot;,
&quot;lng&quot;:      -73998842,
&quot;id&quot;:            252,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991255,      40.760192 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             18,
&quot;name&quot;: &quot;479 - 9 Ave &amp; W 45 St&quot;,
&quot;idx&quot;:            253,
&quot;lat&quot;:       40760192,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.056Z&quot;,
&quot;lng&quot;:      -73991255,
&quot;id&quot;:            253,
&quot;free&quot;:             10,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990617,      40.766696 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;480 - W 53 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            254,
&quot;lat&quot;:       40766696,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.057Z&quot;,
&quot;lng&quot;:      -73990617,
&quot;id&quot;:            254,
&quot;free&quot;:              7,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.962644,      40.712604 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;481 - S 3 St &amp; Bedford Ave&quot;,
&quot;idx&quot;:            255,
&quot;lat&quot;:       40712604,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.057Z&quot;,
&quot;lng&quot;:      -73962644,
&quot;id&quot;:            255,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.999317,      40.739355 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;482 - W 15 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            256,
&quot;lat&quot;:       40739355,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.058Z&quot;,
&quot;lng&quot;:      -73999317,
&quot;id&quot;:            256,
&quot;free&quot;:              9,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988899,      40.732232 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             12,
&quot;name&quot;: &quot;483 - E 12 St &amp; 3 Ave&quot;,
&quot;idx&quot;:            257,
&quot;lat&quot;:       40732232,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.059Z&quot;,
&quot;lng&quot;:      -73988899,
&quot;id&quot;:            257,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980144,      40.755002 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;484 - W 44 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            258,
&quot;lat&quot;:       40755002,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.059Z&quot;,
&quot;lng&quot;:      -73980144,
&quot;id&quot;:            258,
&quot;free&quot;:             44,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983389,       40.75038 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;485 - W 37 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            259,
&quot;lat&quot;:       40750380,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.066Z&quot;,
&quot;lng&quot;:      -73983389,
&quot;id&quot;:            259,
&quot;free&quot;:             33,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988557,        40.7462 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;486 - Broadway &amp; W 29 St&quot;,
&quot;idx&quot;:            260,
&quot;lat&quot;:       40746200,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.067Z&quot;,
&quot;lng&quot;:      -73988557,
&quot;id&quot;:            260,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.975738,      40.733142 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             16,
&quot;name&quot;: &quot;487 - E 20 St &amp; FDR Drive&quot;,
&quot;idx&quot;:            261,
&quot;lat&quot;:       40733142,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.068Z&quot;,
&quot;lng&quot;:      -73975738,
&quot;id&quot;:            261,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993722,      40.756458 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;488 - W 39 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            262,
&quot;lat&quot;:       40756458,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.069Z&quot;,
&quot;lng&quot;:      -73993722,
&quot;id&quot;:            262,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001768,      40.750663 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;489 - 10 Ave &amp; W 28 St&quot;,
&quot;idx&quot;:            263,
&quot;lat&quot;:       40750663,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.069Z&quot;,
&quot;lng&quot;:      -74001768,
&quot;id&quot;:            263,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993934,      40.751551 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;490 - 8 Ave &amp; W 33 St&quot;,
&quot;idx&quot;:            264,
&quot;lat&quot;:       40751551,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.07Z&quot;,
&quot;lng&quot;:      -73993934,
&quot;id&quot;:            264,
&quot;free&quot;:             33,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.986022,      40.740963 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;491 - E 24 St &amp; Park Ave S&quot;,
&quot;idx&quot;:            265,
&quot;lat&quot;:       40740963,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.071Z&quot;,
&quot;lng&quot;:      -73986022,
&quot;id&quot;:            265,
&quot;free&quot;:             50,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99093,      40.750199 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             47,
&quot;name&quot;: &quot;492 - W 33 St &amp; 7 Ave&quot;,
&quot;idx&quot;:            266,
&quot;lat&quot;:       40750199,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.071Z&quot;,
&quot;lng&quot;:      -73990930,
&quot;id&quot;:            266,
&quot;free&quot;:              2,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.982911,        40.7568 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;493 - W 45 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            267,
&quot;lat&quot;:       40756800,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.072Z&quot;,
&quot;lng&quot;:      -73982911,
&quot;id&quot;:            267,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.997235,      40.747348 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;494 - W 26 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            268,
&quot;lat&quot;:       40747348,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.073Z&quot;,
&quot;lng&quot;:      -73997235,
&quot;id&quot;:            268,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.993012,      40.762698 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             21,
&quot;name&quot;: &quot;495 - W 47 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            269,
&quot;lat&quot;:       40762698,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.073Z&quot;,
&quot;lng&quot;:      -73993012,
&quot;id&quot;:            269,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992389,      40.737261 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;496 - E 16 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            270,
&quot;lat&quot;:       40737261,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.074Z&quot;,
&quot;lng&quot;:      -73992389,
&quot;id&quot;:            270,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990092,      40.737049 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             36,
&quot;name&quot;: &quot;497 - E 17 St &amp; Broadway&quot;,
&quot;idx&quot;:            271,
&quot;lat&quot;:       40737049,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.075Z&quot;,
&quot;lng&quot;:      -73990092,
&quot;id&quot;:            271,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988084,      40.748548 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;498 - Broadway &amp; W 32 St&quot;,
&quot;idx&quot;:            272,
&quot;lat&quot;:       40748548,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.075Z&quot;,
&quot;lng&quot;:      -73988084,
&quot;id&quot;:            272,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981918,      40.769155 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;499 - Broadway &amp; W 60 St&quot;,
&quot;idx&quot;:            273,
&quot;lat&quot;:       40769155,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.076Z&quot;,
&quot;lng&quot;:      -73981918,
&quot;id&quot;:            273,
&quot;free&quot;:             25,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983361,      40.762288 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;500 - Broadway &amp; W 51 St&quot;,
&quot;idx&quot;:            274,
&quot;lat&quot;:       40762288,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.077Z&quot;,
&quot;lng&quot;:      -73983361,
&quot;id&quot;:            274,
&quot;free&quot;:             42,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971212,      40.744219 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;501 - FDR Drive &amp; E 35 St&quot;,
&quot;idx&quot;:            275,
&quot;lat&quot;:       40744219,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.077Z&quot;,
&quot;lng&quot;:      -73971212,
&quot;id&quot;:            275,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981346,      40.714215 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;502 - Henry St &amp; Grand St&quot;,
&quot;idx&quot;:            276,
&quot;lat&quot;:       40714215,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.078Z&quot;,
&quot;lng&quot;:      -73981346,
&quot;id&quot;:            276,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987519,      40.738274 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;503 - E 20 St &amp; Park Ave&quot;,
&quot;idx&quot;:            277,
&quot;lat&quot;:       40738274,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.079Z&quot;,
&quot;lng&quot;:      -73987519,
&quot;id&quot;:            277,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.981655,      40.732218 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;504 - 1 Ave &amp; E 15 St&quot;,
&quot;idx&quot;:            278,
&quot;lat&quot;:       40732218,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.079Z&quot;,
&quot;lng&quot;:      -73981655,
&quot;id&quot;:            278,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988483,      40.749012 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             30,
&quot;name&quot;: &quot;505 - 6 Ave &amp; W 33 St&quot;,
&quot;idx&quot;:            279,
&quot;lat&quot;:       40749012,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.083Z&quot;,
&quot;lng&quot;:      -73988483,
&quot;id&quot;:            279,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.979737,      40.739126 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;507 - E 25 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            280,
&quot;lat&quot;:       40739126,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.084Z&quot;,
&quot;lng&quot;:      -73979737,
&quot;id&quot;:            280,
&quot;free&quot;:             32,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996674,      40.763413 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;508 - W 46 St &amp; 11 Ave&quot;,
&quot;idx&quot;:            281,
&quot;lat&quot;:       40763413,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.084Z&quot;,
&quot;lng&quot;:      -73996674,
&quot;id&quot;:            281,
&quot;free&quot;:              7,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.001971,      40.745497 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;509 - 9 Ave &amp; W 22 St&quot;,
&quot;idx&quot;:            282,
&quot;lat&quot;:       40745497,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.085Z&quot;,
&quot;lng&quot;:      -74001971,
&quot;id&quot;:            282,
&quot;free&quot;:              6,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.98042,      40.760659 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;510 - W 51 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            283,
&quot;lat&quot;:       40760659,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.086Z&quot;,
&quot;lng&quot;:      -73980420,
&quot;id&quot;:            283,
&quot;free&quot;:             49,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977724,      40.729386 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             21,
&quot;name&quot;: &quot;511 - E 14 St &amp; Avenue B&quot;,
&quot;idx&quot;:            284,
&quot;lat&quot;:       40729386,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.086Z&quot;,
&quot;lng&quot;:      -73977724,
&quot;id&quot;:            284,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.998392,      40.750072 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;512 - W 29 St &amp; 9 Ave&quot;,
&quot;idx&quot;:            285,
&quot;lat&quot;:       40750072,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.087Z&quot;,
&quot;lng&quot;:      -73998392,
&quot;id&quot;:            285,
&quot;free&quot;:             18,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988639,      40.768254 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;513 - W 56 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            286,
&quot;lat&quot;:       40768254,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.088Z&quot;,
&quot;lng&quot;:      -73988639,
&quot;id&quot;:            286,
&quot;free&quot;:             22,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002776,      40.760875 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;514 - 12 Ave &amp; W 40 St&quot;,
&quot;idx&quot;:            287,
&quot;lat&quot;:       40760875,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.089Z&quot;,
&quot;lng&quot;:      -74002776,
&quot;id&quot;:            287,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.994618,      40.760094 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             27,
&quot;name&quot;: &quot;515 - W 43 St &amp; 10 Ave&quot;,
&quot;idx&quot;:            288,
&quot;lat&quot;:       40760094,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.089Z&quot;,
&quot;lng&quot;:      -73994618,
&quot;id&quot;:            288,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.967843,      40.752068 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;516 - E 47 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            289,
&quot;lat&quot;:       40752068,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.09Z&quot;,
&quot;lng&quot;:      -73967843,
&quot;id&quot;:            289,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977988,      40.751492 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             42,
&quot;name&quot;: &quot;517 - Pershing Square S&quot;,
&quot;idx&quot;:            290,
&quot;lat&quot;:       40751492,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.091Z&quot;,
&quot;lng&quot;:      -73977988,
&quot;id&quot;:            290,
&quot;free&quot;:             13,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973441,      40.747803 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              2,
&quot;name&quot;: &quot;518 - E 39 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            291,
&quot;lat&quot;:       40747803,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.091Z&quot;,
&quot;lng&quot;:      -73973441,
&quot;id&quot;:            291,
&quot;free&quot;:             37,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977701,      40.751884 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             36,
&quot;name&quot;: &quot;519 - Pershing Square N&quot;,
&quot;idx&quot;:            292,
&quot;lat&quot;:       40751884,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.092Z&quot;,
&quot;lng&quot;:      -73977701,
&quot;id&quot;:            292,
&quot;free&quot;:             23,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976485,      40.759922 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;520 - W 52 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            293,
&quot;lat&quot;:       40759922,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.095Z&quot;,
&quot;lng&quot;:      -73976485,
&quot;id&quot;:            293,
&quot;free&quot;:             39,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99481,      40.750449 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             20,
&quot;name&quot;: &quot;521 - 8 Ave &amp; W 31 St&quot;,
&quot;idx&quot;:            294,
&quot;lat&quot;:       40750449,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.096Z&quot;,
&quot;lng&quot;:      -73994810,
&quot;id&quot;:            294,
&quot;free&quot;:             47,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.972078,      40.757147 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;522 - E 51 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:            295,
&quot;lat&quot;:       40757147,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.096Z&quot;,
&quot;lng&quot;:      -73972078,
&quot;id&quot;:            295,
&quot;free&quot;:             50,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.991381,      40.754665 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             23,
&quot;name&quot;: &quot;523 - W 38 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            296,
&quot;lat&quot;:       40754665,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.097Z&quot;,
&quot;lng&quot;:      -73991381,
&quot;id&quot;:            296,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983169,      40.755273 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;524 - W 43 St &amp; 6 Ave&quot;,
&quot;idx&quot;:            297,
&quot;lat&quot;:       40755273,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.098Z&quot;,
&quot;lng&quot;:      -73983169,
&quot;id&quot;:            297,
&quot;free&quot;:             54,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002116,      40.755941 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;525 - W 34 St &amp; 11 Ave&quot;,
&quot;idx&quot;:            298,
&quot;lat&quot;:       40755941,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.098Z&quot;,
&quot;lng&quot;:      -74002116,
&quot;id&quot;:            298,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984907,      40.747659 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;526 - E 33 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            299,
&quot;lat&quot;:       40747659,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.099Z&quot;,
&quot;lng&quot;:      -73984907,
&quot;id&quot;:            299,
&quot;free&quot;:             36,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.974347,      40.743155 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             48,
&quot;name&quot;: &quot;527 - E 33 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            300,
&quot;lat&quot;:       40743155,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.1Z&quot;,
&quot;lng&quot;:      -73974347,
&quot;id&quot;:            300,
&quot;free&quot;:              9,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.97706,      40.742909 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             32,
&quot;name&quot;: &quot;528 - 2 Ave &amp; E 30 St&quot;,
&quot;idx&quot;:            301,
&quot;lat&quot;:       40742909,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.101Z&quot;,
&quot;lng&quot;:      -73977060,
&quot;id&quot;:            301,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.990985,      40.757569 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             29,
&quot;name&quot;: &quot;529 - W 42 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            302,
&quot;lat&quot;:       40757569,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.101Z&quot;,
&quot;lng&quot;:      -73990985,
&quot;id&quot;:            302,
&quot;free&quot;:             10,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.992662,      40.718939 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;531 - Forsyth St &amp; Broome St&quot;,
&quot;idx&quot;:            303,
&quot;lat&quot;:       40718939,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.102Z&quot;,
&quot;lng&quot;:      -73992662,
&quot;id&quot;:            303,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.960876,      40.710451 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             40,
&quot;name&quot;: &quot;532 - S 5 Pl &amp; S 4 St&quot;,
&quot;idx&quot;:            304,
&quot;lat&quot;:       40710451,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.107Z&quot;,
&quot;lng&quot;:      -73960876,
&quot;id&quot;:            304,
&quot;free&quot;:              2,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.987216,      40.752996 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;533 - Broadway &amp; W 39 St&quot;,
&quot;idx&quot;:            305,
&quot;lat&quot;:       40752996,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.107Z&quot;,
&quot;lng&quot;:      -73987216,
&quot;id&quot;:            305,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.012723,       40.70255 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;534 - Water - Whitehall Plaza&quot;,
&quot;idx&quot;:            306,
&quot;lat&quot;:       40702550,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.108Z&quot;,
&quot;lng&quot;:      -74012723,
&quot;id&quot;:            306,
&quot;free&quot;:             29,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.97536,      40.741443 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;536 - 1 Ave &amp; E 30 St&quot;,
&quot;idx&quot;:            307,
&quot;lat&quot;:       40741443,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.109Z&quot;,
&quot;lng&quot;:      -73975360,
&quot;id&quot;:            307,
&quot;free&quot;:             21,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.984092,      40.740258 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              4,
&quot;name&quot;: &quot;537 - Lexington Ave &amp; E 24 St&quot;,
&quot;idx&quot;:            308,
&quot;lat&quot;:       40740258,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.11Z&quot;,
&quot;lng&quot;:      -73984092,
&quot;id&quot;:            308,
&quot;free&quot;:             34,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.977876,      40.757952 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;538 - W 49 St &amp; 5 Ave&quot;,
&quot;idx&quot;:            309,
&quot;lat&quot;:       40757952,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.11Z&quot;,
&quot;lng&quot;:      -73977876,
&quot;id&quot;:            309,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.960241,      40.715348 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             28,
&quot;name&quot;: &quot;539 - Metropolitan Ave &amp; Bedford Ave&quot;,
&quot;idx&quot;:            310,
&quot;lat&quot;:       40715348,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.111Z&quot;,
&quot;lng&quot;:      -73960241,
&quot;id&quot;:            310,
&quot;free&quot;:              3,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983209,      40.741472 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             15,
&quot;name&quot;: &quot;540 - Lexington Ave &amp; E 26 St&quot;,
&quot;idx&quot;:            311,
&quot;lat&quot;:       40741472,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.112Z&quot;,
&quot;lng&quot;:      -73983209,
&quot;id&quot;:            311,
&quot;free&quot;:             24,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978094,      40.736502 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;545 - E 23 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            312,
&quot;lat&quot;:       40736502,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.112Z&quot;,
&quot;lng&quot;:      -73978094,
&quot;id&quot;:            312,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.983035,      40.744449 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              6,
&quot;name&quot;: &quot;546 - E 30 St &amp; Park Ave S&quot;,
&quot;idx&quot;:            313,
&quot;lat&quot;:       40744449,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.113Z&quot;,
&quot;lng&quot;:      -73983035,
&quot;id&quot;:            313,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.989402,       40.70255 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             14,
&quot;name&quot;: &quot;2000 - Front St &amp; Washington St&quot;,
&quot;idx&quot;:            314,
&quot;lat&quot;:       40702550,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.114Z&quot;,
&quot;lng&quot;:      -73989402,
&quot;id&quot;:            314,
&quot;free&quot;:             16,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.973329,       40.69892 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             10,
&quot;name&quot;: &quot;2001 - 7 Ave &amp; Farragut St&quot;,
&quot;idx&quot;:            315,
&quot;lat&quot;:       40698920,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.114Z&quot;,
&quot;lng&quot;:      -73973329,
&quot;id&quot;:            315,
&quot;free&quot;:              5,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.963198,      40.716887 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             19,
&quot;name&quot;: &quot;2002 - Wythe Ave &amp; Metropolitan Ave&quot;,
&quot;idx&quot;:            316,
&quot;lat&quot;:       40716887,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.115Z&quot;,
&quot;lng&quot;:      -73963198,
&quot;id&quot;:            316,
&quot;free&quot;:              8,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.980242,       40.73416 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             13,
&quot;name&quot;: &quot;2003 - 1 Ave &amp; E 18 St&quot;,
&quot;idx&quot;:            317,
&quot;lat&quot;:       40734160,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.116Z&quot;,
&quot;lng&quot;:      -73980242,
&quot;id&quot;:            317,
&quot;free&quot;:             17,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.004704,      40.724399 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             17,
&quot;name&quot;: &quot;2004 - 6 Ave &amp; Broome St&quot;,
&quot;idx&quot;:            318,
&quot;lat&quot;:       40724399,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.117Z&quot;,
&quot;lng&quot;:      -74004704,
&quot;id&quot;:            318,
&quot;free&quot;:             19,
&quot;fillColor&quot;: &quot;#FFFFBF&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [        -73.971,      40.705311 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             11,
&quot;name&quot;: &quot;2005 - Railroad Ave &amp; Kay Ave&quot;,
&quot;idx&quot;:            319,
&quot;lat&quot;:       40705311,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.117Z&quot;,
&quot;lng&quot;:      -73971000,
&quot;id&quot;:            319,
&quot;free&quot;:              1,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976341,      40.765909 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              8,
&quot;name&quot;: &quot;2006 - Central Park S &amp; 6 Ave&quot;,
&quot;idx&quot;:            320,
&quot;lat&quot;:       40765909,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.118Z&quot;,
&quot;lng&quot;:      -73976341,
&quot;id&quot;:            320,
&quot;free&quot;:             41,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.016776,      40.705692 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             22,
&quot;name&quot;: &quot;2008 - Little West St &amp; 1 Pl&quot;,
&quot;idx&quot;:            321,
&quot;lat&quot;:       40705692,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.119Z&quot;,
&quot;lng&quot;:      -74016776,
&quot;id&quot;:            321,
&quot;free&quot;:              1,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.996826,      40.711174 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              9,
&quot;name&quot;: &quot;2009 - Catherine St &amp; Monroe St&quot;,
&quot;idx&quot;:            322,
&quot;lat&quot;:       40711174,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.119Z&quot;,
&quot;lng&quot;:      -73996826,
&quot;id&quot;:            322,
&quot;free&quot;:             26,
&quot;fillColor&quot;: &quot;#FDAE61&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.002347,      40.721654 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;2010 - Grand St &amp; Greene St&quot;,
&quot;idx&quot;:            323,
&quot;lat&quot;:       40721654,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.12Z&quot;,
&quot;lng&quot;:      -74002347,
&quot;id&quot;:            323,
&quot;free&quot;:             34,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.976806,      40.739445 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;2012 - E 27 St &amp; 1 Ave&quot;,
&quot;idx&quot;:            324,
&quot;lat&quot;:       40739445,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.121Z&quot;,
&quot;lng&quot;:      -73976806,
&quot;id&quot;:            324,
&quot;free&quot;:             30,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.971214,      40.750223 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;2017 - E 43 St &amp; 2 Ave&quot;,
&quot;idx&quot;:            325,
&quot;lat&quot;:       40750223,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.127Z&quot;,
&quot;lng&quot;:      -73971214,
&quot;id&quot;:            325,
&quot;free&quot;:             38,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.988596,      40.759291 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             31,
&quot;name&quot;: &quot;2021 - W 45 St &amp; 8 Ave&quot;,
&quot;idx&quot;:            326,
&quot;lat&quot;:       40759291,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.128Z&quot;,
&quot;lng&quot;:      -73988596,
&quot;id&quot;:            326,
&quot;free&quot;:             12,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.959206,      40.758491 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             27,
&quot;name&quot;: &quot;2022 - E 59 St &amp; Sutton Pl&quot;,
&quot;idx&quot;:            327,
&quot;lat&quot;:       40758491,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.128Z&quot;,
&quot;lng&quot;:      -73959206,
&quot;id&quot;:            327,
&quot;free&quot;:              4,
&quot;fillColor&quot;: &quot;#1A9641&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.970313,       40.75968 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              1,
&quot;name&quot;: &quot;2023 - E 55 St &amp; Lexington Ave&quot;,
&quot;idx&quot;:            328,
&quot;lat&quot;:       40759680,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.129Z&quot;,
&quot;lng&quot;:      -73970313,
&quot;id&quot;:            328,
&quot;free&quot;:             35,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -74.015756,      40.711512 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              5,
&quot;name&quot;: &quot;3002 - South End Ave &amp; Liberty St&quot;,
&quot;idx&quot;:            329,
&quot;lat&quot;:       40711512,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:07.13Z&quot;,
&quot;lng&quot;:      -74015756,
&quot;id&quot;:            329,
&quot;free&quot;:             20,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [      -73.99906,      40.730477 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:             24,
&quot;name&quot;: &quot;336 - Sullivan St &amp; Washington Sq&quot;,
&quot;idx&quot;:            330,
&quot;lat&quot;:       40730477,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.936Z&quot;,
&quot;lng&quot;:      -73999060,
&quot;id&quot;:            330,
&quot;free&quot;:             11,
&quot;fillColor&quot;: &quot;#A6D96A&quot; 
} 
},
{
 &quot;type&quot;: &quot;Feature&quot;,
&quot;geometry&quot;: {
 &quot;type&quot;: &quot;Point&quot;,
&quot;coordinates&quot;: [     -73.978474,      40.687979 ] 
},
&quot;properties&quot;: {
 &quot;bikes&quot;:              0,
&quot;name&quot;: &quot;243 - Fulton St &amp; Rockwell Pl&quot;,
&quot;idx&quot;:            331,
&quot;lat&quot;:       40687979,
&quot;timestamp&quot;: &quot;2014-02-01T02:52:06.881Z&quot;,
&quot;lng&quot;:      -73978474,
&quot;id&quot;:            331,
&quot;free&quot;:             31,
&quot;fillColor&quot;: &quot;#D7191C&quot; 
} 
} 
] 
}

  var map = L.map(spec.dom, spec.mapOpts)
  
    map.setView(spec.center, spec.zoom);

    if (spec.provider){
      L.tileLayer.provider(spec.provider).addTo(map)    
    } else {
		  L.tileLayer(spec.urlTemplate, spec.layerOpts).addTo(map)
    }
     
    
    
    
    
    
    if (spec.circle2){
      for (var c in spec.circle2){
        var circle = L.circle(c.center, c.radius, c.opts)
         .addTo(map);
      }
    }
    
    
    
    
    var geojsonLayer = L.geoJson(spec.features 
        , 
         {
 &quot;pointToLayer&quot;:  function(feature, latlng){
    return L.circleMarker(latlng, {
      radius: 4,
      fillColor: feature.properties.fillColor || &#039;red&#039;,    
      color: &#039;#000&#039;,
      weight: 1,
      fillOpacity: 0.8
    })
  } 
}
      ).addTo(map)
    
   
   
   
&lt;/script&gt;
    
  &lt;/body&gt;
&lt;/html&gt;
' scrolling='no' seamless class='rChart 
leaflet
 '
id='iframe-chart136453dba1d00'>
</iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>



<style>
  iframe.rChart {height: 200px}
  table.nofluid {width: auto; margin: 0 auto;}
  pre {margin-left: 0px;}
</style>