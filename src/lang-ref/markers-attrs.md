## 注记符号（markers）的属性

##### marker-file `uri`


默认值： 
_(An ellipse or circle, if width equals height)_

一个椭圆或正圆形符号

An SVG file that this marker shows at each placement. If no file is given, the marker will show an ellipse.

设置绘制注记符号所使用的SVG文件。如果没有指定具体的文件，则默认使用一个椭圆形符号对每个位置的注记进行渲染。

* * *

##### marker-opacity `float`


默认值： 1
_(The stroke-opacity and fill-opacity will be used)_

边缘和填充均不透明

The overall opacity of the marker, if set, overrides both the opacity of both the fill and stroke

设置注记符号整体的透明度。如果设置了该属性，则会覆盖在marker-fill-opacity和marker-line-opacity属性中设置的透明度。

* * *

##### marker-fill-opacity `float`


默认值： 1
_(opaque)_

注记符号填充部分为不透明

The fill opacity of the marker

设置注记符号填充部分的透明度。

* * *

##### marker-line-color `color`


默认值： black


The color of the stroke around a marker shape.

设置注记符号边线的颜色。

* * *

##### marker-line-width `float`


默认值： undefined


The width of the stroke around a marker shape, in pixels. This is positioned on the boundary, so high values can cover the area itself.

设置注记符号边线的宽度，以像素为单位。但如果该值设置过大会导致注记本身被过粗的边线覆盖。

* * *

##### marker-line-opacity `float`


默认值： 1
_(opaque)_

注记符号边线完全不透明

The opacity of a line

设置注记符号边线的透明度。
* * *

##### marker-placement `keyword`
`point``line``interior`

默认值： point
_(Place markers at the center point (centroid) of the geometry)_

注记符号被置于几何要素的重心（形心）位置

Attempt to place markers on a point, in the center of a polygon, or if markers-placement:line, then multiple times along a line. &#x27;interior&#x27; placement can be used to ensure that points placed on polygons are forced to be inside the polygon interior

设置注记在几何要素上的放置方式，可以位于点要素上，或者在面要素的中心位置，还可以沿着线要素反复出现（通过设置markers-placement:line实现）。如果取值interior，那么可以确保注记符号被绘制在多边形的内部。

* * *

##### marker-multi-policy `keyword`
`each``whole``largest`

默认值： each
_(If a feature contains multiple geometries and the placement type is either point or interior then a marker will be rendered for each)_

如果一个要素包含了多个几何形状，而且放置方式是point或interior，那么注记符号就会在每个几何形状处都会被绘制一次

A special setting to allow the user to control rendering behavior for &#x27;multi-geometries&#x27; (when a feature contains multiple geometries). This setting does not apply to markers placed along lines. The &#x27;each&#x27; policy is default and means all geometries will get a marker. The &#x27;whole&#x27; policy means that the aggregate centroid between all geometries will be used. The &#x27;largest&#x27; policy means that only the largest (by bounding box areas) feature will get a rendered marker (this is how text labeling behaves by default).

该属性是为包含多个几何形状的地理（multi-geometries）要素准备的，对沿线放置的注记符号不起作用。其默认值为each，也就是每个几何形状上都会被绘制一个注记；whole表示注记将被绘制在所有几何形状组合后的重心位置；而largest表示注记将被绘制在最大（依据最小包围框的面积）的那个几何形状上（这也同样是文本标注在多几何要素上绘制的默认方法）。

* * *

##### marker-type `keyword`
`arrow``ellipse`

默认值： ellipse


The default marker-type. If a SVG file is not given as the marker-file parameter, the renderer provides either an arrow or an ellipse (a circle if height is equal to width)

设置默认的注记符号类型。如果没有指定用于渲染注记的SVG文件，那么内置的渲染引擎可以提供两种选择：箭头或椭圆。

* * *

##### marker-width `expression`


默认值： 10


The width of the marker, if using one of the default types.

设置注记符号的宽度。这个属性只适用于两种内置的默认注记样式。
* * * 
##### marker-height `expression`


默认值： 10


The height of the marker, if using one of the default types.

设置注记符号的高度。这个属性只适用于两种内置的默认注记样式。

* * *

##### marker-fill `color`


默认值： blue


The color of the area of the marker.

设置注记的填充色。

* * *

##### marker-allow-overlap `boolean`


默认值： false
_(Do not allow makers to overlap with each other - overlapping markers will not be shown.)_

不允许注记符号相互压盖（被压盖的注记将不被显式）

Control whether overlapping markers are shown or hidden.

设置被压盖的注记符号是否被显式在地图上。

* * *

##### marker-ignore-placement `boolean`


默认值： false
_(do not store the bbox of this geometry in the collision detector cache)_

不在冲突检测器的缓存中存储几何形状的外包框

value to control whether the placement of the feature will prevent the placement of other features

设置是否允许在与当前要素重叠的位置放置其它要素。（？）

* * *

##### marker-spacing `float`


默认值： 100


Space between repeated markers in pixels. If the spacing is less than the marker size or larger than the line segment length then no marker will be placed

设置重复绘制的注记之间的间距，单位为像素。如果设定的间距小于注记符号本身的尺寸，或者大于线要素的长度，那么注记就绘制不出来。

* * *

##### marker-max-error `float`


默认值： 0.2


The maximum difference between actual marker placement and the marker-spacing parameter. Setting a high value can allow the renderer to try to resolve placement conflicts with other symbolizers.

设置实际的注记位置与marker-spacing属性值之间的最大误差。如果将该属性值调高，那么渲染引擎就会尝试处理与其它注记符号之间的位置冲突。

* * *

##### marker-transform `functions`


默认值： 
_(No transformation)_

无变换

SVG transformation definition

设置SVG图形的变换方法
* * *

##### marker-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_

几何要素会根据地图的地理范围进行切割。

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.

为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。
* * *

##### marker-smooth `float`


默认值： 0
_(no smoothing)_
不对拐点进行平滑

Range: 0-1
取值范围：0到1

Smooths out geometry angles. 0 is no smoothing, 1 is fully smoothed. Values greater than 1 will produce wild, looping geometries.
对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。
* * *

##### marker-geometry-transform `functions`


默认值： none
_(geometry will not be transformed)_
不对几何要素进行变换

Allows transformation functions to be applied to the geometry.
为几何要素定义变换函数。
* * *

##### marker-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_
将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。
* * *

