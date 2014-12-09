## 线模式（line-pattern）的属性

##### line-pattern-file `uri`


默认值： none


An image file to be repeated and warped along a line
设置沿线重复绘制的图像文件。
* * *

##### line-pattern-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_

几何要素会根据地图的地理范围进行切割。

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.

为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。
* * *

##### line-pattern-simplify `float`


默认值： 0
_(geometry will not be simplified)_
不对几何要素进行简化

geometries are simplified by the given tolerance
如果要对几何要素按照地图综合的方法进行简化，那么通过该属性来设定阈值。
* * *

##### line-pattern-simplify-algorithm `keyword`
`radial-distance``zhao-saalfeld``visvalingam-whyatt`

默认值： radial-distance
_(geometry will not be simplified using the radial distance algorithm)_

不使用radial-distance算法进行简化

geometries are simplified by the given algorithm

设置对线要素进行综合的简化算法。
* * *

##### line-pattern-smooth `float`


默认值： 0
_(no smoothing)_
不进行平滑处理

Range: 0-1
取值范围：0到1

Smooths out geometry angles. 0 is no smoothing, 1 is fully smoothed. Values greater than 1 will produce wild, looping geometries.
对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。

* * *

##### line-pattern-offset `float`


默认值： 0
_(no offset)_
无偏移

Offsets a line a number of pixels parallel to its actual path. Positive values move the line left, negative values move it right (relative to the directionality of the line).
将线要素相对于其原有位置向左（沿着线的走向）或向右偏移一定量的像素绘制。正值表示左偏，负值表示右偏。

* * *

##### line-pattern-geometry-transform `functions`


默认值： none
_(geometry will not be transformed)_
不对几何要素进行变换

Allows transformation functions to be applied to the geometry.
为几何要素定义变换函数。
* * *

##### line-pattern-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_
将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。
* * *

