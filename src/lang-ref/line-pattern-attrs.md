### 线图案（line-pattern）的属性

##### line-pattern-file `uri`

默认值： `none`

说明：设置沿线重复绘制的图像文件。

* * *

##### line-pattern-clip `boolean`

默认值： `true` _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为`false`而不采用这个策略。

* * * 
##### line-pattern-simplify `float`

默认值： `0` _(不对几何要素进行简化)_

说明：如果要对几何要素按照地图综合的方法进行简化，那么通过该属性来设定阈值。

* * *

##### line-pattern-simplify-algorithm `keyword`

取值范围：`radial-distance` `zhao-saalfeld` `visvalingam-whyatt`

默认值： `radial-distance` _(不使用radial-distance算法进行简化)_

说明：设置对线要素进行综合的简化算法。

* * * 
##### line-pattern-smooth `float`

取值范围：`0`到`1`

默认值： `0` _(不进行平滑处理)_

说明：对线的拐点进行平滑处理。`0`表示不进行平滑，`1`表示完全平滑。如果取值大于`1`，会导致绘制的几何要素扭曲变形。

* * *

##### line-pattern-offset `float`

默认值： `0` _(无偏移)_

说明：将线要素相对于其原有位置向左（沿着线的走向）或向右偏移一定量的像素绘制。正值表示左偏，负值表示右偏。

* * *

##### line-pattern-geometry-transform `functions`

默认值： `none` _(不对几何要素进行变换)_

说明：为几何要素定义变换函数。

* * *

##### line-pattern-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： `src-over` _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

* * *

