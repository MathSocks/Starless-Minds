---
publish: true
created: 2026-07-11T16:08:11.261+10:00
modified: 2026-07-09T14:33:18.746+10:00
---

> [!NOTE]- Quick Calculator\
> Map Height in Pixels: `INPUT[number:map_height_y]`\
> Map Width in Pixels: `INPUT[number:map_width_x]`\
> lat: `VIEW[{map_height_y} / 2][math]`\
> long: `VIEW[{map_width_x} / 2][math]`\
> How Many Pixels In Scale: `INPUT[number:scale_pixels]`\
> How Many Units in Scale: `INPUT[number:scale_pixels_range]`\
> Scale: `VIEW[1/({scale_pixels}/{scale_pixels_range})][math:mapCalc1]`

```leaflet
id: Map_Westtown ### Must be unique with no spaces  
image: [[MAPWesttown.png]] ### Link to the map image file  
bounds: [[0,0], [752, 618]] ### Size of the map in px Height_y, Width_x. Ignore 0,0  
height: 618px ### Size of the leaflet embed in px on your screen  
width: 90% ### Size of the leaflet embed in your note  
lat: 376 ### To center the map, make this half of the map height.  
long: 309 ### To center the map, make this half of the map width.  
minZoom: -0.5 ### Controls how far away from the map you can zoom out. Hover over the target icon to see the current level.  
maxZoom: 1 ### Controls how far towards the map you can zoom in. Hover over the target icon to see the current level.  
defaultZoom: -0.2 ### Sets the default zoom level when the map loads. Hover over the target icon to see the current level.  
zoomDelta: 0.7 ### Adjust how much the zoom changes when you zoom in or out.  
unit: ft ### The value displayed when measuring so you know what type of unit is being measure.  
scale: 0.3710575139146568 ### Real units/px (resolution) of your map  
recenter: false  
darkmode: false ### marker
```
