### 文本符号（text）的属性

##### text-name `expression`

默认值： 

说明：设置文本符号上显示的文字。可以通过用中括号括起来的字段名来指定要使用的数据字段，例如[column\_name]。

* * *

##### text-face-name `string`

默认值： undefined

说明：设置文本符号所使用的字体。

* * *

##### text-size `float`

默认值： `10`

说明：设置文本符号中文字的字号，以像素为单位。

* * *

##### text-ratio `unsigned`

默认值： `0`

说明：设置折行后的文本所占比例。

* * *

##### text-wrap-width `unsigned`

默认值： `0`

说明：设置文本块在多长的时候进行折行，以字符为单位。

* * *

##### text-wrap-before `boolean`

默认值： `false`

说明：控制文本文字的折行动作。如果该值为`false`，那么每一行文本都会比`wrap-width`属性设定的值略长。

* * * 
##### text-wrap-character `string`

默认值：  

说明：使用设置的字符而非空格作为文本标注的折行字符。

* * *

##### text-spacing `unsigned`

默认值： undefined

说明：设置沿线绘制文本符号时每两个文本符号之间的间距。

* * *

##### text-character-spacing `float`

默认值： `0`

说明：设置文本文字的字间距，以像素为单位。

* * *

##### text-line-spacing `unsigned`

默认值： `0`

说明：设置文本文字的行间距。

* * *

##### text-label-position-tolerance `unsigned`

默认值： `0`

说明：设置文本标注相对于其理想位置的偏移量，以像素为单位（目前仅适用于线要素）

* * *

##### text-max-char-angle-delta `float`

默认值： `22.5`

说明：设置文本文字的最大折转角，以十进制角度为单位。这个值会在绘制时被换算成弧度，例如默认的22.5度会按照22.5/math.pi\*180.0公式被换算成0.3925弧度。这个值越大，则被绘制在尖锐转角处的文本符号会越少。

* * *

##### text-fill `color`

默认值： `#000000` _(黑色)_

说明：设置文本文字的颜色。

* * *

##### text-opacity `float`

取值范围：`0`到`1`

默认值： `1` _(不透明)_

说明：设置文本文字的透明度，取值范围为`0`到`1`。

* * *

##### text-halo-fill `color`

默认值： `#FFFFFF` _(白色)_

说明：设置文本文字边缘的光晕颜色。

* * *

##### text-halo-radius `float`

默认值： `0` _(无光晕)_

说明：设置文本文字边缘的光晕大小，以像素为单位。

* * * 
##### text-halo-rasterizer `keyword`

取值范围：`full` `fast`

默认值： `full`

说明：设置用于渲染文字光晕的方法，速度优先还是质量优先。

* * *

##### text-dx `float`

默认值： `0`

说明：设置文本文字的水平偏移量，以像素为单位。正值表示向右偏移。

* * *

##### text-dy `float`

默认值： `0`

说明：设置文本文字的垂直偏移量，以像素为单位。正值表示向下偏移。

* * *

##### text-vertical-alignment `keyword`

取值范围：`top` `middle` `bottom` `auto`

默认值： `auto` _(自动，但受到dy值的影响。当dy\>0时，取bottom；而当dy\<0时，取top)_

说明：设置文本符号相对于点要素座标的位置。

* * * 
##### text-avoid-edges `boolean`

默认值： `false`

说明：设置是否避免将文本标注置于绘制区域（通常为瓦片）的边缘处。

* * *

##### text-min-distance `float`

默认值： undefined

说明：设置文本符号之间的最小间距。

* * *

##### text-min-padding `float`

默认值： undefined

说明：设置文本符号在元瓦片中的最小边距。

* * *

##### text-min-path-length `float`

默认值： `0` _(无论路线长度是多少，都要绘制文本符号)_

说明：如果设置了该值，那么只有在当路线长度大于该值的时候才绘制文本符号。

* * * 
##### text-allow-overlap `boolean`

默认值： `false` _(不允许文本符号相互压盖)_

说明：设置是否显式相互压盖的文本符号。

* * *

##### text-orientation `expression`

默认值： undefined

说明：设置文本旋转。

* * * 
##### text-placement `keyword`

取值范围：`point` `line` `vertex` `interior`

默认值： `point`

说明：设置文本符号在对应几何要素上的放置方式。

* * *

##### text-placement-type `keyword`

取值范围：`dummy` `simple`

默认值： `dummy`

说明：设置文本符号之间相互避让的算法。`simple`表示使用由`text-placements`属性指定的基本算法。而`dummy`则表示不使用该特性。

* * *

##### text-placements `string`

默认值： 

说明：如果`placement-type`属性被设置为`simple`，那么就会依据该属性的值（即形如“POSITIONS, [SIZES]”的字符串）执行文本符号相互避让算法。例如：`text-placements: "E,NE,SE,W,NW,SW";`

* * *

##### text-transform `keyword`

取值范围：`none` `uppercase` `lowercase` `capitalize`

默认值： `none`

说明：设置是否对文本字符进行大小写转换。

* * *

##### text-horizontal-alignment `keyword`

取值范围：`left` `middle` `right` `auto`

默认值： `auto`

说明：设置文本文字的水平对齐方式。

* * *

##### text-align `keyword`

取值范围：`left` `right` `center` `auto`

默认值： `auto` _(默认的自动方式是居中对齐，但如果已经设置了`placement-type`属性，那么就会依据`text-placements`属性的值来对文字进行靠左或靠右对齐)_

说明：设置文本文字的对齐方式。

* * *

##### text-clip `boolean`

默认值： `true` _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为`false`而不采用这个策略。

* * *

##### text-comp-op `keyword`

取值范围：`clear` `src` `dst` `src-over` `dst-over` `src-in` `dst-in` `src-out` `dst-out` `src-atop` `dst-atop` `xor` `plus` `minus` `multiply` `screen` `overlay` `darken` `lighten` `color-dodge` `color-burn` `hard-light` `soft-light` `difference` `exclusion` `contrast` `invert` `invert-rgb` `grain-merge` `grain-extract` `hue` `saturation` `color` `value`

默认值： `src-over` _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

* * * 