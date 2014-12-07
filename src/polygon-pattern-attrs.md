## 2.9 面模式（polygon-pattern）的属性

##### polygon-pattern-file `uri`

默认值： none

说明：设置用于平铺填充面要素的图像文件。



##### polygon-pattern-alignment `keyword`

取值范围：`local` `global`

默认值： local

说明：设置填充时的对齐方式，local指在当前图层中对齐，global指在整个地图中对齐。



##### polygon-pattern-gamma `float`

取值范围：0到1

默认值： 1 _(完全抗锯齿)_

说明：设置面要素边缘的抗锯齿级别，影响绘制效果和速度。



##### polygon-pattern-opacity `float`

默认值： 1 _(保持填充图片的透明度不变)_

说明：设置填充图片的透明度。



##### polygon-pattern-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。



##### polygon-pattern-simplify `float`

默认值： 0 _(不对面要素的边线进行简化)_

说明：如果要对面要素的边线按照地图综合的方法进行简化，那么通过该属性来设定阈值。



##### polygon-pattern-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对面要素的边线进行综合的简化算法。



##### polygon-pattern-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。



##### polygon-pattern-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。



##### polygon-pattern-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

