## 2.6 盾标符号（shield）的属性

##### shield-name `expression`

默认值： undefined

说明：设置盾标上显示的标注文字。可以通过用中括号括起来的字段名来指定要使用的数据字段，例如[column\_name]。

* * *

##### shield-file `uri`

默认值： none

说明：设置显示在盾标文本后面的背景图片。

* * *

##### shield-face-name `string`

默认值： 

说明：设置盾标上标注文字的字体与样式。

* * *

##### shield-unlock-image `boolean`

默认值： false _(盾标文字将被置于盾标背景图片的中心位置)_

说明：设置盾标文字与背景图片之间的位置关系。如果不想把盾标文本绘制在背景图的中心，那么就应该将该属性值设置为true。

* * *

##### shield-size `float`

默认值： undefined

说明：设置盾标文字的大小，以像素为单位。

* * *

##### shield-fill `color`

默认值： undefined

说明：设置盾标文字的颜色。

* * * 
##### shield-placement `keyword`

取值范围：`point``line``vertex``interior`

默认值： point

说明：设置盾标的放置方式。point方式是将盾标置于点要素的位置，line方式是将盾标在线要素上沿线绘制多次，vertex方式是将盾标置于多边形的顶点位置，而interior方式则是将盾标置于面要素的内部。

* * *

##### shield-avoid-edges `boolean`

默认值： false

说明：设置是否避免在地图或瓦片的边缘处绘制盾标。

* * *

##### shield-allow-overlap `boolean`

默认值： false _(不允许盾标与其它现有地图要素重叠)_

说明：该属性用于设置在盾标与地图上其它符号出现压盖时，是否显示盾标。

* * * 
##### shield-min-distance `float`

默认值： 0

说明：设置相邻两个盾标符号（可以是相同的盾标，也可以是不同的）之间的最小距离。

* * * 
##### shield-spacing `float`

默认值： 0

说明：设置在同一线要素上多次绘制的盾标之间的间隔。

* * * 
##### shield-min-padding `float`

默认值： 0

说明：设置盾标在瓦片上绘制时的最小边距。

* * * 
##### shield-wrap-width `unsigned`

默认值： 0

说明：设置盾标文本多长的时候需要折行。

* * * 
##### shield-wrap-before `boolean`

默认值： false

说明：控制盾标文本的折行动作。如果该值为false，那么每一行文本都会比wrap-width属性设定的值略长。

* * * 
##### shield-wrap-character `string`

默认值：  

说明：设置盾标文本的折行字符。

* * * 
##### shield-halo-fill `color`

默认值： #FFFFFF _(白色)_

说明：设置盾标文本的光晕颜色。

* * *

##### shield-halo-radius `float`

默认值： 0 _(盾标文本无光晕效果)_

说明：设置盾标文本光晕的大小，单位为像素。

* * * 
##### shield-character-spacing `unsigned`

默认值： 0

说明：设置盾标文字的字间距。该属性目前仅适用于点要素上的盾标。

* * *

##### shield-line-spacing `unsigned`

默认值： undefined

说明：设置盾标文本中的行距，以像素为单位。

* * *

##### shield-text-dx `float`

默认值： 0

说明：设置盾标文本的水平偏移量，以像素为单位。正值表示向右偏移。

* * * 
##### shield-text-dy `float`

默认值： 0

说明：设置盾标文本的垂直偏移量，以像素为单位。正值表示向下偏移。

* * * 
##### shield-dx `float`

默认值： 0

说明：设置盾标本身的水平偏移量，以像素为单位。正值表示向右偏移。

* * * 
##### shield-dy `float`

默认值： 0

说明：设置盾标本身的垂直偏移量，以像素为单位。正值表示向下偏移。

* * * 
##### shield-opacity `float`

默认值： 1

说明：设置盾标背景图片的透明度。

* * * 
##### shield-text-opacity `float`

默认值： 1

说明：设置盾标文本的透明度。

* * * 
##### shield-horizontal-alignment `keyword`

取值范围：`left``middle``right``auto`

默认值： auto

说明：设置盾标相对于其中心点的水平对齐方式。

* * * 
##### shield-vertical-alignment `keyword`

取值范围：`top``middle``bottom``auto`

默认值： middle

说明：设置盾标相对于其中心点的垂直对齐方式。

* * * 
##### shield-placement-type `keyword`

取值范围：`dummy``simple`

默认值： dummy

说明：设置盾标之间相互避让的算法。simple表示使用由shield-placements属性指定的基本算法。而dummy则表示不使用该特性。

* * * 
##### shield-placements `string`

默认值： 

说明：如果shield-placement-type属性被设置为simple，那么就会依据该属性的值（即形如“POSITIONS, [SIZES]”的字符串）执行盾标相互避让算法。例如：`shield-placements: "E,NE,SE,W,NW,SW";`

* * * 
##### shield-text-transform `keyword`

取值范围：`none``uppercase``lowercase``capitalize`

默认值： none

说明：设置盾标文字的大小写方式。

* * * 
##### shield-justify-alignment `keyword`

取值范围：`left``center``right``auto`

默认值： auto

说明：设置盾标文本的对齐方式。

* * * 
##### shield-transform `functions`

默认值： _(无变换)_

说明：设置SVG的变换函数。

* * * 
##### shield-clip `boolean`

默认值： true _(几何要素会根据地图的地理范围进行切割)_

说明：为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。

* * * 
##### shield-comp-op `keyword`

取值范围：`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over _(将当前符号置于其它符号的上一层)_

说明：这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。

* * *