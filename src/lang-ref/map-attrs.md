## 地图（map）的属性

##### background-color `color`


默认值： none
_(透明色)_

Map Background color

设置地图的背景颜色

* * *

##### background-image `uri`


默认值： 
_(透明色)_

An image that is repeated below all features on a map as a background.

一张以平铺形式置于最底层的背景图片。

* * *

##### background-image-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(The background-image will be blended with the background normally (placed on top of any existing background-color))_

背景图片被置于所设置的背景色上一层。

Set the compositing operation used to blend the image into the background

设置背景图片与背景颜色之间的合成操作方式。

* * *

##### background-image-opacity `float`


默认值： 1
_(The image opacity will not be changed when applied to the map background)_

背景图片的透明度保持不变。

Set the opacity of the image

设置图片的透明度

* * *

##### srs `string`


默认值： +proj=longlat +ellps=WGS84 +datum=WGS84 +no\_defs
_(The proj4 literal of EPSG:4326 is assumed to be the Map&#x27;s spatial reference and all data from layers within this map will be plotted using this coordinate system. If any layers do not declare an srs value then they will be assumed to be in the same srs as the Map and not transformations will be needed to plot them in the Map&#x27;s coordinate space)_

这是EPSG:4326空间参考系的proj4表达形式。地图中所有图层的数据都会采用这同一种参考系来绘制。如果地图中的某一图层没有显式声明自己所使用的参考系，那么这个图层将被认为与地图的参考系相同，并且在绘制的时候不会对这个图层中包含的数据进行座标变换。

Map spatial reference (proj4 string)

地图的空间参考系（以proj4字符串表达）

* * *

##### buffer-size `float`


默认值： 0
_(无缓冲区)_

Extra tolerance around the map (in pixels) used to ensure labels crossing tile boundaries are equally rendered in each tile (e.g. cut in each tile). Not intended to be used in combination with &quot;avoid-edges&quot;.

在地图周围增加一圈额外的绘制区域（以像素数表达）。这个属性的设置是为了保证那些出现在地图边界附近的文本标注不至于在渲染时被截断。注意这个属性不应该与“avoid-edges”同时使用。

* * *

##### base `string`


默认值： 
_(This base path defaults to an empty string meaning that any relative paths to files referenced in styles or layers will be interpreted relative to the application process.)_

工作路径默认为空。此时在样式定义中所有通过相对路径的方式引用的外部资源文件都会以应用程序所在的路径为父目录去寻址。

Any relative paths used to reference files will be understood as relative to this directory path if the map is loaded from an in memory object rather than from the filesystem. If the map is loaded from the filesystem and this option is not provided it will be set to the directory of the stylesheet.

如果map是从内存中加载的（？），那么所有以相对路径方式引用的外部资源则均为相对于由该base属性定义的路径。如果map是从文件系统中加载的，并且这个base属性没有被显式设置，那么base的值就是样式文件所在的目录。

* * *

##### font-directory `uri`


默认值： none
_(No map-specific fonts will be registered)_

不注册专门用于当前map的字体

Path to a directory which holds fonts which should be registered when the Map is loaded (in addition to any fonts that may be automatically registered).

指定专门用于当前map的字体目录（自动加载的默认字体除外）

* * *


