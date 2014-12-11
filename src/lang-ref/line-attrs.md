### 线符号（line）的属性

##### line-color `color`


默认值： rgba(0,0,0,1)
_(black and fully opaque (alpha = 1), same as rgb(0,0,0))_

完全不透明的黑色

The color of a drawn line

设置线要素的线条颜色。

* * *

##### line-width `float`


默认值： 1


The width of a line in pixels

设置线要素的线条宽度，单位为像素。

* * *

##### line-opacity `float`


默认值： 1
_(opaque)_

不透明

The opacity of a line

设置线要素的透明度。

* * *

##### line-join `keyword`
取值范围：`miter``round``bevel`

默认值： miter

The behavior of lines when joining

设置线要素之间在交汇点处如何绘制。三种绘制方法的比较如下图所示。

![](http://www.w3.org/TR/SVG/images/painting/linejoin.png)

_图片来源：[www.w3.org](http://www.w3.org/TR/SVG/painting.html "line join")_


* * *

##### line-cap `keyword`
`butt``round``square`

默认值： butt


The display of line endings

设置线要素的端点形状。
* * *

##### line-gamma `float`


默认值： 1
_(fully antialiased)_

完全抗锯齿

Range: 0-1

取值范围：0到1

Level of antialiasing of stroke line

设置绘制线要素时的抗锯齿级别。

* * *

##### line-gamma-method `keyword`
`power``linear``none``threshold``multiply`

默认值： power
_(pow(x,gamma) is used to calculate pixel gamma, which produces slightly smoother line and polygon antialiasing than the &#x27;linear&#x27; method, while other methods are usually only used to disable AA)_

使用pow(x, gamma)来计算像素gamma值。与linear相比，应用power值绘制出来的线与面要素更加平滑。而其它的取值通常只是用来关闭抗锯齿。

An Antigrain Geometry specific rendering hint to control the quality of antialiasing. Under the hood in Mapnik this method is used in combination with the &#x27;gamma&#x27; value (which defaults to 1). The methods are in the AGG source at https:&#x2F;&#x2F;github.com&#x2F;mapnik&#x2F;mapnik&#x2F;blob&#x2F;master&#x2F;deps&#x2F;agg&#x2F;include&#x2F;agg_gamma_functions.h

设置抗锯齿的具体算法，控制绘制质量。

* * *

##### line-dasharray `numbers`


默认值： none
_(solid line)_

实线（无虚线效果）

A pair of length values [a,b], where (a) is the dash length and (b) is the gap length respectively. More than two values are supported for more complex patterns.

通过设置[a,b]的值设置虚线样式。其中a为虚线段的长度，b为虚线段间隔的长度。此外，还可以设置更多的值，从而获得更加复杂的绘制效果。

* * *

##### line-miterlimit `float`


默认值： 4
_(Will auto-convert miters to bevel line joins when theta is less than 29 degrees as per the SVG spec: &#x27;miterLength &#x2F; stroke-width = 1 &#x2F; sin ( theta &#x2F; 2 )&#x27;)_

当theta角小于29度时，自动将线要素交汇样式从miter改为bevel

The limit on the ratio of the miter length to the stroke-width. Used to automatically convert miter joins to bevel joins for sharp angles to avoid the miter extending beyond the thickness of the stroking path. Normally will not need to be set, but a larger value can sometimes help avoid jaggy artifacts.

设置线端的切角长度与线宽的比例上限。当线要素交汇处出现尖锐的锐角时，由于切角与线宽比例失调会导致错误的绘制结果。设置了该属性则会在出现上述情况时自动将线要素交汇样式从miter改为bevel。一般情况下，这个属性不需要显式设置，但有时可以通过设定一个较大的值可以避免出现参差不齐的不良绘制效果。

* * *

##### line-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_

几何要素会根据地图的地理范围进行切割。

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.

为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

* * *

##### line-simplify `float`


默认值： 0
_(geometry will not be simplified)_

不对几何要素进行简化

geometries are simplified by the given tolerance

如果要对几何要素按照地图综合的方法进行简化，那么通过该属性来设定阈值。

* * *

##### line-simplify-algorithm `keyword`
`radial-distance``zhao-saalfeld``visvalingam-whyatt`

默认值： radial-distance
_(geometry will not be simplified using the radial distance algorithm)_

不使用radial-distance算法进行简化（？）

geometries are simplified by the given algorithm

设置对线要素进行综合的简化算法。

* * *

##### line-smooth `float`


默认值： 0
_(no smoothing)_

不对拐点进行平滑

Range: 0-1

取值范围：0到1

Smooths out geometry angles. 0 is no smoothing, 1 is fully smoothed. Values greater than 1 will produce wild, looping geometries.

对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。


* * *

##### line-offset `float`


默认值： 0
_(no offset)_

无偏移

Offsets a line a number of pixels parallel to its actual path. Positive values move the line left, negative values move it right (relative to the directionality of the line).

将线要素相对于其原有位置向左（沿着线的走向）或向右偏移一定量的像素绘制。正值表示左偏，负值表示右偏。

* * *

##### line-rasterizer `keyword`
`full``fast`

默认值： full

Exposes an alternate AGG rendering method that sacrifices some accuracy for speed.

设置线渲染方式，可以通过牺牲部分精确度以换取绘制速度。

* * *

##### line-geometry-transform `functions`


默认值： none
_(geometry will not be transformed)_

不对几何要素进行变换

Allows transformation functions to be applied to the geometry.

为几何要素定义变换函数。

* * *

##### line-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_

将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.

这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。
* * *

