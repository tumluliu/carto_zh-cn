### 所有符号的公共属性

##### image-filters `functions`

默认值： `none` _(不使用图像过滤器)_

说明：以函数形式提供的一组图像过滤器。图像过滤器会作用于处于活动状态的画布。如果设置了多个图像过滤器，那么每增加一个过滤器都会触发创建一个新的画布，当这个新的画布被渲染完成后，再通过合成的方式与主画布合并。如果要直接在主画布上应用图像过滤器，那么需要使用`direct-image-filters`属性。

* * *

##### direct-image-filters `functions`

默认值： `none` _(不使用图像过滤器)_

说明：作用于主画布上的图像过滤器（参见`image-filters`）

* * *

##### comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： `src-over` _(把当前图层覆盖在其它图层之上)_

说明：合成操作。该属性用于定义当前图层与其相邻图层如何合成。关于合成操作，请参见本书基础用法一章中关于合成操作一节的内容。

* * *

##### opacity `float`

取值范围：`0`到`1`

默认值： `1` _(不透明)_

说明：设置透明度。为样式设置alpha值（首先，创建一个独立的缓冲区，在这个缓冲区中为所有要素应用alpha实现透明化，然后再把这个独立缓冲区合成到主缓冲区中）

* * *
