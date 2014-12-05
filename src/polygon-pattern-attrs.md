## 面模式（polygon-pattern）的属性

##### polygon-pattern-file `uri`


默认值： none


Image to use as a repeated pattern fill within a polygon
设置用于平铺填充面要素的图像文件。
* * *

##### polygon-pattern-alignment `keyword`
`local``global`

默认值： local


Specify whether to align pattern fills to the layer or to the map.
设置填充时的对齐方式，local指在当前图层中对齐，global指在整个地图中对齐。
* * *

##### polygon-pattern-gamma `float`


默认值： 1
_(fully antialiased)_
完全抗锯齿
Range: 0-1
Level of antialiasing of polygon pattern edges
* * *

##### polygon-pattern-opacity `float`


默认值： 1
_(The image is rendered without modifications)_

Apply an opacity level to the image used for the pattern
* * *

##### polygon-pattern-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.
* * *

##### polygon-pattern-simplify `float`


默认值： 0
_(geometry will not be simplified)_

geometries are simplified by the given tolerance
* * *

##### polygon-pattern-simplify-algorithm `keyword`
`radial-distance``zhao-saalfeld``visvalingam-whyatt`

默认值： radial-distance
_(geometry will not be simplified using the radial distance algorithm)_

geometries are simplified by the given algorithm
* * *

##### polygon-pattern-smooth `float`


默认值： 0
_(no smoothing)_
Range: 0-1
Smooths out geometry angles. 0 is no smoothing, 1 is fully smoothed. Values greater than 1 will produce wild, looping geometries.
* * *

##### polygon-pattern-geometry-transform `functions`


默认值： none
_(geometry will not be transformed)_

Allows transformation functions to be applied to the geometry.
* * *

##### polygon-pattern-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
* * *

