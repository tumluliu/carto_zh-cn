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
设置面要素边缘的抗锯齿级别，影响绘制效果和速度。
* * *

##### polygon-pattern-opacity `float`


默认值： 1
_(The image is rendered without modifications)_
保持填充图片的透明度不变

Apply an opacity level to the image used for the pattern
设置填充图片的透明度。
* * * 
##### polygon-pattern-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_
几何要素会根据地图的地理范围进行切割

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.
为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

* * *

##### polygon-pattern-simplify `float`


默认值： 0
_(geometry will not be simplified)_
不对面要素的边线进行简化

geometries are simplified by the given tolerance
如果要对面要素的边线按照地图综合的方法进行简化，那么通过该属性来设定阈值。
* * *

##### polygon-pattern-simplify-algorithm `keyword`
`radial-distance``zhao-saalfeld``visvalingam-whyatt`

默认值： radial-distance
_(geometry will not be simplified using the radial distance algorithm)_
不使用radial-distance算法进行简化（？）

geometries are simplified by the given algorithm
设置对面要素的边线进行综合的简化算法。
* * *

##### polygon-pattern-smooth `float`


默认值： 0
_(no smoothing)_
不对拐点进行平滑

Range: 0-1
取值范围：0到1

Smooths out geometry angles. 0 is no smoothing, 1 is fully smoothed. Values greater than 1 will produce wild, looping geometries.
对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。
* * *

##### polygon-pattern-geometry-transform `functions`


默认值： none
_(geometry will not be transformed)_
不对几何要素进行变换。

Allows transformation functions to be applied to the geometry.
为几何要素定义变换函数。
* * *

##### polygon-pattern-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_
将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。
* * *

