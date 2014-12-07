## 2.3 线符号（line）的属性

##### line-color `color`

默认值： rgba(0,0,0,1) _(完全不透明的黑色)_

说明：设置线要素的线条颜色。

##### line-width `float`

默认值： 1

说明：设置线要素的线条宽度，单位为像素。

##### line-opacity `float`

默认值： 1 _(不透明)_

说明：设置线要素的透明度。

##### line-join `keyword`

取值范围：`miter` `round` `bevel`

默认值： miter

说明：设置线要素之间在交汇点处如何绘制。三种绘制方法的比较如下图所示。

![](linejoin.png)
_图片来源：[w3.org](http://www.w3.org/TR/SVG/painting.html "line join")_

##### line-cap `keyword`

取值范围：`butt` `round` `square`

默认值： butt

说明：设置线要素的端点形状。

##### line-gamma `float`

取值范围：0到1

默认值： 1 _(完全抗锯齿)_

说明：设置绘制线要素时的抗锯齿级别。

##### line-gamma-method `keyword`

取值范围：`power` `linear` `none` `threshold` `multiply`

默认值： power _(使用pow(x, gamma)来计算像素gamma值。与linear相比，应用power值绘制出来的线与面要素更加平滑。而其它的取值通常只是用来关闭抗锯齿。)_

说明：设置抗锯齿的具体算法，控制绘制质量。

##### line-dasharray `numbers`

默认值： none _(实线（无虚线效果）)_

说明：通过设置[a,b]的值设置虚线样式。其中a为虚线段的长度，b为虚线段间隔的长度。此外，还可以设置更多的值，从而获得更加复杂的绘制效果。

##### line-miterlimit `float`

默认值： 4 _(当theta角小于29度时，自动将线要素交汇样式从miter改为bevel)_

说明：设置线端的切角长度与线宽的比例上限。当线要素交汇处出现尖锐的锐角时，由于切角与线宽比例失调会导致错误的绘制结果。设置了该属性则会在出现上述情况时自动将线要素交汇样式从miter改为bevel。一般情况下，这个属性不需要显式设置，但有时可以通过设定一个较大的值可以避免出现参差不齐的不良绘制效果。

##### line-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

##### line-simplify `float`

默认值： 0 _(不对几何要素进行简化)_

说明：如果要对几何要素按照地图综合的方法进行简化，那么通过该属性来设定阈值。

##### line-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对线要素进行综合的简化算法。

##### line-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。

##### line-offset `float`

默认值： 0 _(无偏移)_

说明：将线要素相对于其原有位置向左（沿着线的走向）或向右偏移一定量的像素绘制。正值表示左偏，负值表示右偏。

##### line-rasterizer `keyword`

取值范围：`full` `fast`

默认值： full

说明：设置线渲染方式，可以通过牺牲部分精确度以换取绘制速度。

##### line-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。

##### line-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。