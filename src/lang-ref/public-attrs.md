# CartoCSS参考手册

The following is a list of properties provided in CartoCSS that you can apply to map elements.

CartoCSS提供了一系列用于定义地图样式的属性。以下列表中包含了这些属性的含义和所有可取的值。

## 所有符号的公共属性

##### image-filters `functions`


默认值： none
_(不使用图像过滤器)_

A list of image filters that will be applied to the active rendering canvas for a given style. The presence of one more more `image-filters` will trigger a new canvas to be created before starting to render a style and then this canvas will be composited back into the main canvas after rendering all features and after all `image-filters` have been applied. See `direct-image-filters` if you want to apply a filter directly to the main canvas.

以函数形式提供的一组图像过滤器。图像过滤器会作用于处于活动状态的画布。如果设置了多个图像过滤器，那么每增加一个过滤器都会触发创建一个新的画布，当这个新的画布被渲染完成后，再通过合成的方式与主画布合并。如果要直接在主画布上应用图像过滤器，那么需要使用`direct-image-filters`属性。

* * *

##### direct-image-filters `functions`


默认值： none
_(不使用图像过滤器)_

A list of image filters to apply to the main canvas (see the `image-filters` doc for how they work on a separate canvas)

作用于主画布上的图像过滤器（参见`image-filters`）

* * *

##### comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(把当前图层覆盖在其它图层之上)_

Composite operation. This defines how this layer should behave relative to layers atop or below it.

合成操作。该属性用于定义当前图层与其相邻图层如何合成。关于合成运算，请参见图像合成（Image Compositing）技术的相关内容。

* * *

##### opacity `float`


默认值： 1
_(no separate buffer will be used and no alpha will be applied to the style after rendering)_

不使用单独缓冲区，不设置alpha通道

An alpha value for the style (which means an alpha applied to all features in separate buffer and then composited back to main buffer)

为样式设置alpha值（首先，创建一个独立的缓冲区，在这个缓冲区中为所有要素应用alpha实现透明化，然后再把这个独立缓冲区合成到主缓冲区中）

* * *
