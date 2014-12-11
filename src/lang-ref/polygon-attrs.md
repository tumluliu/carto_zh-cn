### 面符号（polygon）的属性

##### polygon-fill `color`


默认值： rgba(128,128,128,1)
_(gray and fully opaque (alpha = 1), same as rgb(128,128,128))_

完全不透明的灰色

Fill color to assign to a polygon

设置面要素的填充色

* * *

##### polygon-opacity `float`


默认值： 1
_(opaque)_

不透明

The opacity of the polygon

面要素的透明度（0为完全透明，1为完全不透明）

* * *

##### polygon-gamma `float`


默认值： 1
_(fully antialiased)_

完全抗锯齿

Range: 0-1

取值范围：0到1

Level of antialiasing of polygon edges

设置面要素边缘的抗锯齿级别，影响绘制效果和速度。抗锯齿级别越高（最高为1），绘制效果越好，但绘制速度最慢；反之，绘制效果最差，但绘制速度最快。注意这里所说的绘制速度的快慢只是理论上的，实际效果与软硬件环境密切相关。

* * *

##### polygon-gamma-method `keyword`
`power``linear``none``threshold``multiply`

默认值： power
_(pow(x,gamma) is used to calculate pixel gamma, which produces slightly smoother line and polygon antialiasing than the &#x27;linear&#x27; method, while other methods are usually only used to disable AA)_

使用pow(x, gamma)来计算像素gamma值。与linear相比，应用power值绘制出来的线与面要素更加平滑。而其它的取值通常只是用来关闭抗锯齿。

An Antigrain Geometry specific rendering hint to control the quality of antialiasing. Under the hood in Mapnik this method is used in combination with the &#x27;gamma&#x27; value (which defaults to 1). The methods are in the AGG source at https:&#x2F;&#x2F;github.com&#x2F;mapnik&#x2F;mapnik&#x2F;blob&#x2F;master&#x2F;deps&#x2F;agg&#x2F;include&#x2F;agg_gamma_functions.h

设置抗锯齿的具体算法，控制绘制质量。

* * *

##### polygon-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_

几何要素会根据地图的地理范围进行切割。

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.

为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

* * *

##### polygon-simplify `float`


默认值： 0
_(geometry will not be simplified)_

不对面要素的边线进行简化

geometries are simplified by the given tolerance

如果要对面要素的边线按照地图综合的方法进行简化，那么通过该属性来设定阈值。（参见地图综合中的线简化算法，如Douglas-Peuker算法）

* * *

##### polygon-simplify-algorithm `keyword`
`radial-distance``zhao-saalfeld``visvalingam-whyatt`

默认值： radial-distance
_(geometry will not be simplified using the radial distance algorithm)_

不使用radial-distance算法进行简化（？为什么是不简化？）

geometries are simplified by the given algorithm

设置对面要素的边线进行综合的简化算法。

* * *

##### polygon-smooth `float`


默认值： 0
_(no smoothing)_

不对拐点进行平滑

Range: 0-1

取值范围：0到1

Smooths out geometry angles. 0 is no smoothing, 1 is fully smoothed. Values greater than 1 will produce wild, looping geometries.

对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。

* * *

##### polygon-geometry-transform `functions`


默认值： none
_(geometry will not be transformed)_

不对几何要素进行变换。

Allows transformation functions to be applied to the geometry.

为几何要素定义变换函数。

* * *

##### polygon-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_

将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.

这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

* * *

