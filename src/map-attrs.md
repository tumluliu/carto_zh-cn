### 2.2 地图（map）的属性

与其它十种符号不同，本节列出的是用于配置地图整体样式的属性。

##### background-color `color`

默认值： none _(透明色)_

说明：设置地图的背景颜色。

##### background-image `uri`

默认值：_(透明色)_

说明：设置一张以平铺形式置于最底层的背景图片。

##### background-image-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： src-over _(背景图片被置于所设置的背景色上一层)_

说明：设置背景图片与背景颜色之间的合成操作方式。

##### background-image-opacity `float`

默认值： 1 _(背景图片的透明度保持不变)_

说明：设置背景图片的透明度。

##### srs `string`

默认值： +proj=longlat +ellps=WGS84 +datum=WGS84 +no\_defs _(这是EPSG:4326空间参考系的proj4表达形式。地图中所有图层的数据都会采用这同一种参考系来绘制。如果地图中的某一图层没有显式声明自己所使用的参考系，那么这个图层将被认为与地图的参考系相同，并且在绘制的时候不会对这个图层中包含的数据进行座标变换。)_

说明：设置地图的空间参考系（以proj4字符串表达）。

##### buffer-size `float`

默认值： 0 _(无缓冲区)_

说明：在地图周围增加一圈额外的绘制区域（以像素数表达）。这个属性的设置是为了保证那些出现在地图边界附近的文本标注不至于在渲染时被截断。注意这个属性不应该与`avoid-edges`同时使用。

##### base `string`

默认值：  _(工作路径默认为空。此时在样式定义中所有通过相对路径的方式引用的外部资源文件都会以应用程序所在的路径为父目录去寻址。)_

说明：如果map是从内存中加载的，那么所有以相对路径方式引用的外部资源则均为相对于由该base属性定义的路径。如果map是从文件系统中加载的，并且这个base属性没有被显式设置，那么base的值就是样式文件所在的目录。

##### font-directory `uri`

默认值： none _(不注册专门用于当前map的字体)_

说明：指定专门用于当前map的字体目录（自动加载的默认字体除外）