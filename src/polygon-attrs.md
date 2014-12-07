## 2.4 面符号（polygon）的属性

##### polygon-fill `color`

默认值： rgba(128,128,128,1) _(完全不透明的灰色)_

说明：设置面要素的填充色

##### polygon-opacity `float`

默认值： 1 _(不透明)_

说明：面要素的透明度（0为完全透明，1为完全不透明）

##### polygon-gamma `float`

取值范围：0到1

默认值： 1 _(完全抗锯齿)_

说明：设置面要素边缘的抗锯齿级别，影响绘制效果和速度。抗锯齿级别越高（最高为1），绘制效果越好，但绘制速度最慢；反之，绘制效果最差，但绘制速度最快。注意这里所说的绘制速度的快慢只是理论上的，实际效果与软硬件环境密切相关。

##### polygon-gamma-method `keyword`

取值范围：`power` `linear` `none` `threshold` `multiply`

默认值： power _(使用pow(x, gamma)来计算像素gamma值。与linear相比，应用power值绘制出来的线与面要素更加平滑。而其它的取值通常只是用来关闭抗锯齿。)_

说明：设置抗锯齿的具体算法，控制绘制质量。

##### polygon-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

##### polygon-simplify `float`

默认值： 0 _(不对面要素的边线进行简化)_

说明：如果要对面要素的边线按照地图综合的方法进行简化，那么通过该属性来设定阈值。（参见地图综合中的线简化算法，如Douglas-Peuker算法）

##### polygon-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： radial-distance _(不使用radial-distance算法进行简化)_

说明：设置对面要素的边线进行综合的简化算法。

##### polygon-smooth `float`

取值范围：0到1

默认值： 0 _(不对拐点进行平滑)_

说明：对边线的拐点进行平滑处理。0表示不进行平滑，1表示完全平滑。如果取值大于1，会导致绘制的几何要素扭曲变形。

##### polygon-geometry-transform `functions`

默认值： none _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。

##### polygon-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。