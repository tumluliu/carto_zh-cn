## 栅格符号（raster）的属性

##### raster-opacity `float`


默认值： 1
_(opaque)_

The opacity of the raster symbolizer on top of other symbolizers.
* * *

##### raster-filter-factor `float`


默认值： -1
_(Allow the datasource to choose appropriate downscaling.)_

This is used by the Raster or Gdal datasources to pre-downscale images using overviews. Higher numbers can sometimes cause much better scaled image output, at the cost of speed.
* * *

##### raster-scaling `keyword`
`near``fast``bilinear``bilinear8``bicubic``spline16``spline36``hanning``hamming``hermite``kaiser``quadric``catrom``gaussian``bessel``mitchell``sinc``lanczos``blackman`

默认值： near


The scaling algorithm used to making different resolution versions of this raster layer. Bilinear is a good compromise between speed and accuracy, while lanczos gives the highest quality.
* * *

##### raster-mesh-size `unsigned`


默认值： 16
_(Reprojection mesh will be 1&#x2F;16 of the resolution of the source image)_

A reduced resolution mesh is used for raster reprojection, and the total image size is divided by the mesh-size to determine the quality of that mesh. Values for mesh-size larger than the default will result in faster reprojection but might lead to distortion.
* * *

##### raster-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
* * *

##### raster-colorizer-default-mode `keyword`
`discrete``linear``exact`

默认值： undefined


TODO
* * *

##### raster-colorizer-default-color `color`


默认值： undefined


TODO
* * *

##### raster-colorizer-epsilon `float`


默认值： undefined


TODO
* * *

##### raster-colorizer-stops `tags`


默认值： undefined


TODO
* * *

