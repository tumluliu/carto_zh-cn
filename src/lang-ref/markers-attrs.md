### 注记符号（markers）的属性

##### marker-file `uri`

默认值：  _(一个椭圆或正圆形符号)_

说明：设置绘制注记符号所使用的SVG文件。如果没有指定具体的文件，则默认使用一个椭圆形符号对每个位置的注记进行渲染。

* * *

##### marker-opacity `float`

默认值： `1` _(边缘和填充均不透明)_

说明：设置注记符号整体的透明度。如果设置了该属性，则会覆盖在`marker-fill-opacity`和`marker-line-opacity`属性中设置的透明度。

* * *

##### marker-fill-opacity `float`

默认值： `1` _(注记符号填充部分为不透明)_

说明：设置注记符号填充部分的透明度。

* * *

##### marker-line-color `color`

默认值： `black`

说明：设置注记符号边线的颜色。

* * *

##### marker-line-width `float`

默认值： undefined

说明：设置注记符号边线的宽度，以像素为单位。但如果该值设置过大会导致注记本身被过粗的边线覆盖。

* * *

##### marker-line-opacity `float`

默认值： `1` _(注记符号边线完全不透明)_

说明：设置注记符号边线的透明度。

* * * 
##### marker-placement `keyword`

取值范围：`point` `line` `interior`

默认值： `point` _(注记符号被置于几何要素的重心（形心）位置)_

说明：设置注记在几何要素上的放置方式，可以位于点要素上，或者在面要素的中心位置，还可以沿着线要素反复出现（通过设置`marker-placement:line`实现）。如果取值interior，那么可以确保注记符号被绘制在多边形的内部。

* * *

##### marker-multi-policy `keyword`

取值范围：`each` `whole` `largest`

默认值： `each` _(如果一个要素包含了多个几何形状，而且放置方式是point或interior，那么注记符号就会在每个几何形状处都会被绘制一次)_

说明：该属性是为包含多个几何形状的地理（multi-geometries）要素准备的，对沿线放置的注记符号不起作用。其默认值为`each`，也就是每个几何形状上都会被绘制一个注记；`whole`表示注记将被绘制在所有几何形状组合后的重心位置；而`largest`表示注记将被绘制在最大（依据最小包围框的面积）的那个几何形状上（这也同样是文本标注在多几何要素上绘制的默认方法）。

* * *

##### marker-type `keyword`

取值范围：`arrow` `ellipse`

默认值： `ellipse`

说明：设置默认的注记符号类型。如果没有指定用于渲染注记的SVG文件，那么内置的渲染引擎可以提供两种选择：箭头或椭圆。

* * *

##### marker-width `expression`

默认值： `10`

说明：设置注记符号的宽度。这个属性只适用于两种内置的默认注记样式。

* * *

##### marker-height `expression`

默认值： `10`

说明：设置注记符号的高度。这个属性只适用于两种内置的默认注记样式。

* * *

##### marker-fill `color`

默认值： `blue`

说明：设置注记的填充色。

* * *

##### marker-allow-overlap `boolean`

默认值： `false` _(不允许注记符号相互压盖（被压盖的注记将不被显式）)_

说明：设置被压盖的注记符号是否被显式在地图上。

* * *

##### marker-ignore-placement `boolean`

默认值： `false` _(不在冲突检测器的缓存中存储几何形状的外包框)_

说明：设置是否允许在与当前要素重叠的位置放置其它要素。

* * *

##### marker-spacing `float`

默认值： `100`

说明：设置重复绘制的注记之间的间距，单位为像素。如果设定的间距小于注记符号本身的尺寸，或者大于线要素的长度，那么注记就绘制不出来。

* * *

##### marker-max-error `float`

默认值： `0.2`

说明：设置实际的注记位置与marker-spacing属性值之间的最大误差。如果将该属性值调高，那么渲染引擎就会尝试处理与其它注记符号之间的位置冲突。

* * *

##### marker-transform `functions`

默认值： _(无变换)_

说明：设置SVG图形的变换方法。

* * * 
##### marker-clip `boolean`

默认值： `true` _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为`false`而不采用这个策略。

* * * 
##### marker-smooth `float`

取值范围：`0`到`1`

默认值： `0` _(不对拐点进行平滑)_

说明：对线的拐点进行平滑处理。`0`表示不进行平滑，`1`表示完全平滑。如果取值大于`1`，会导致绘制的几何要素扭曲变形。

* * *

##### marker-geometry-transform `functions`

默认值： `none` _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。

* * *

##### marker-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： `src-over` _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

* * *

