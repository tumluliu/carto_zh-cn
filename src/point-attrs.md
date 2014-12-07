## 2.4 点符号（point）的属性

##### point-file `uri`

默认值： none

说明：设置用于绘制点符号的图像文件。

* * *

##### point-allow-overlap `boolean`

默认值： false _(不允许点符号相互压盖)_

说明：设置是否显式相互压盖的点符号。

* * *

##### point-ignore-placement `boolean`

默认值： false _(不在冲突检测器的缓存中存储几何形状的外包框)_

说明：设置是否允许在与当前要素重叠的位置放置其它要素。

* * *

##### point-opacity `float`

取值范围：0到1

默认值： 1 _(完全不透明)_

说明：设置点符号的透明度。

* * *

##### point-placement `keyword`

取值范围：`centroid``interior`

默认值： centroid

说明：设置点符号的放置方式。

* * *

##### point-transform `functions`

默认值： _(无变换)_

说明：设置SVG图形的变换方法。

* * *

##### point-comp-op `keyword`

取值范围：`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

* * *