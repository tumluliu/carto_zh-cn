### 2.10 栅格符号（raster）的属性

##### raster-opacity `float`

默认值： 1 _(不透明)_

说明：设置栅格符号的透明度。



##### raster-filter-factor `float`

默认值： -1 _(允许数据源自行选用缩小图像尺寸的方法)_

说明：用于栅格或GDAL数据源，对图像尺寸进行预先缩小。将该值调高可以得到更好的缩略图效果，但相应的处理时间也会变长。



##### raster-scaling `keyword`

取值范围：`near` `fast` `bilinear` `bilinear8` `bicubic` `spline16` `spline36` `hanning` `hamming` `hermite` `kaiser` `quadric` `catrom` `gaussian` `bessel` `mitchell` `sinc` `lanczos` `blackman`

默认值： near

说明：设置对栅格数据进行重采样的算法。Bilinear可以在速度和质量方面得到不错的平衡，而lanczos则能够得到最高的绘制质量。



##### raster-mesh-size `unsigned`

默认值： 16 _(取原始图像分辨率的1/16作为栅格的重投影网格大小)_

说明：在对原始图像进行重投影时，是先将图像切分成若干网格，对网格中的小图像片分别重投影。如果设定该值使得网格的尺寸变大（即把该属性的值调小），那么重投影的速度会加快，但可能会导致图像变形。



##### raster-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

