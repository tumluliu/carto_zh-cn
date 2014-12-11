### 文本符号（text）的属性

##### text-name `expression`


默认值： 


Value to use for a text label. Data columns are specified using brackets like [column\_name](#)
设置文本符号上显示的文字。可以通过用中括号括起来的字段名来指定要使用的数据字段，例如[column\_name]。
* * *

##### text-face-name `string`


默认值： undefined


Font name and style to render a label in
设置文本符号所使用的字体。
* * *

##### text-size `float`


默认值： 10


Text size in pixels
设置文本符号中文字的字号，以像素为单位。
* * *

##### text-ratio `unsigned`


默认值： 0


Define the amount of text (of the total) present on successive lines when wrapping occurs
设置折行后的文本所占比例（译注：这解释的好扭曲，到底什么意思，有什么意义？）
* * *

##### text-wrap-width `unsigned`


默认值： 0


Length of a chunk of text in characters before wrapping text
设置文本块在多长的时候进行折行，以字符为单位
* * *

##### text-wrap-before `boolean`


默认值： false


Wrap text before wrap-width is reached. If false, wrapped lines will be a bit longer than wrap-width.
控制文本文字的折行动作。如果该值为false，那么每一行文本都会比wrap-width属性设定的值略长。
* * *

##### text-wrap-character `string`


默认值：  


Use this character instead of a space to wrap long text.
设置盾标文本的折行字符。
* * *

##### text-spacing `unsigned`


默认值： undefined


Distance between repeated text labels on a line (aka. label-spacing)
设置沿线绘制文本符号时每两个文本符号之间的间距。
* * *

##### text-character-spacing `float`


默认值： 0


Horizontal spacing adjustment between characters in pixels
设置文本文字的字间距，以像素为单位。
* * *

##### text-line-spacing `unsigned`


默认值： 0


Vertical spacing adjustment between lines in pixels
设置文本文字的行间距。
* * *

##### text-label-position-tolerance `unsigned`


默认值： 0


Allows the label to be displaced from its ideal position by a number of pixels (only works with placement:line)
设置文本标注相对于其理想位置的偏移量，以像素为单位（目前仅适用于线要素）
* * *

##### text-max-char-angle-delta `float`


默认值： 22.5


The maximum angle change, in degrees, allowed between adjacent characters in a label. This value internally is converted to radians to the default is 22.5\*math.pi&#x2F;180.0. The higher the value the fewer labels will be placed around around sharp corners.
设置文本文字的最大折转角，以十进制角度为单位。这个值会在绘制时被换算成弧度，例如默认的22.5度会按照22.5/math.pi\*180.0公式被换算成0.3925弧度。这个值越大，则被绘制在尖锐转角处的文本符号会越少。
* * *

##### text-fill `color`


默认值： #000000
_(black)_
黑色

Specifies the color for the text
设置文本文字的颜色。
* * *

##### text-opacity `float`


默认值： 1
_(Fully opaque)_
不透明

A number from 0 to 1 specifying the opacity for the text
设置文本文字的透明度，取值范围为0到1。
* * *

##### text-halo-fill `color`


默认值： #FFFFFF
_(white)_
白色

Specifies the color of the halo around the text.
设置文本文字边缘的光晕颜色。
* * *

##### text-halo-radius `float`


默认值： 0
_(no halo)_
无光晕

Specify the radius of the halo in pixels
设置文本文字边缘的光晕大小，以像素为单位。
* * *

##### text-halo-rasterizer `keyword`
`full``fast`

默认值： full


Exposes an alternate text halo rendering method that sacrifices quality for speed.
设置用于渲染文字光晕的方法，速度优先还是质量优先。
* * *

##### text-dx `float`


默认值： 0


Displace text by fixed amount, in pixels, +&#x2F;- along the X axis.  A positive value will shift the text right
设置文本文字的水平偏移量，以像素为单位。正值表示向右偏移。

* * *

##### text-dy `float`


默认值： 0


Displace text by fixed amount, in pixels, +&#x2F;- along the Y axis.  A positive value will shift the text down
设置文本文字的垂直偏移量，以像素为单位。正值表示向下偏移。
* * *

##### text-vertical-alignment `keyword`
`top``middle``bottom``auto`

默认值： auto
_(Default affected by value of dy; &quot;bottom&quot; for dy&gt;0, &quot;top&quot; for dy&lt;0.)_
自动，但受到dy值的影响。当dy\>0时，取bottom；而当dy\<0时，取top

Position of label relative to point position.
设置文本符号相对于点要素座标的位置。
* * *

##### text-avoid-edges `boolean`


默认值： false


Avoid placing labels that intersect with tile boundaries.
设置是否避免将文本标注置于绘制区域（通常为瓦片）的边缘处。
* * *

##### text-min-distance `float`


默认值： undefined


Minimum permitted distance to the next text symbolizer.
设置文本符号之间的最小间距。
* * *

##### text-min-padding `float`


默认值： undefined


Minimum distance a text label will be placed from the edge of a metatile.
设置文本符号在元瓦片中的最小边距。
* * *

##### text-min-path-length `float`


默认值： 0
_(place labels on all paths)_
无论路线长度是多少，都要绘制文本符号

Place labels only on paths longer than this value.
如果设置了该值，那么只有在当路线长度大于该值的时候才绘制文本符号。
* * *

##### text-allow-overlap `boolean`


默认值： false
_(Do not allow text to overlap with other text - overlapping markers will not be shown.)_
不允许文本符号相互压盖

Control whether overlapping text is shown or hidden.
设置是否显式相互压盖的文本符号。
* * *

##### text-orientation `expression`


默认值： undefined


Rotate the text.
设置文本旋转。
* * *

##### text-placement `keyword`
`point``line``vertex``interior`

默认值： point


Control the style of placement of a point versus the geometry it is attached to.
设置文本符号在对应几何要素上的放置方式。
* * *

##### text-placement-type `keyword`
`dummy``simple`

默认值： dummy


Re-position and&#x2F;or re-size text to avoid overlaps. &quot;simple&quot; for basic algorithm (using text-placements string,) &quot;dummy&quot; to turn this feature off.
设置文本符号之间相互避让的算法。simple表示使用由text-placements属性指定的基本算法。而dummy则表示不使用该特性。
* * *

##### text-placements `string`


默认值： 


If &quot;placement-type&quot; is set to &quot;simple&quot;, use this &quot;POSITIONS,[SIZES](#)&quot; string. An example is `text-placements: &quot;E,NE,SE,W,NW,SW&quot;;` 
如果placement-type属性被设置为simple，那么就会依据该属性的值（即形如“POSITIONS, [SIZES]”的字符串）执行文本符号相互避让算法。例如：`text-placements: "E,NE,SE,W,NW,SW";`
* * *

##### text-transform `keyword`
`none``uppercase``lowercase``capitalize`

默认值： none


Transform the case of the characters
设置是否对文本字符进行大小写转换。
* * *

##### text-horizontal-alignment `keyword`
`left``middle``right``auto`

默认值： auto


The text&#x27;s horizontal alignment from its centerpoint
设置文本文字的水平对齐方式。
* * *

##### text-align `keyword`
`left``right``center``auto`

默认值： auto
_(Auto alignment means that text will be centered by default except when using the `placement-type` parameter - in that case either right or left justification will be used automatically depending on where the text could be fit given the `text-placements` directives)_
默认的自动方式是居中对齐，但如果已经设置了`placement-type`属性，那么就会依据`text-placements`属性的值来对文字进行靠左或靠右对齐

Define how text is justified
设置文本文字的对齐方式。
* * *

##### text-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_
几何要素会根据地图的地理范围进行切割

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.
为了提高绘制效率，可以先将矢量要素中所有超出地图边界的部分切掉，再进行绘制。但在某些情况下，为了防止出现绘制错误，也可以通过将该值设为false而不采用这个策略。
* * *

##### text-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_
将当前符号置于其它符号的上一层

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
这也是一个合成操作。它定义了当前的符号应该如何与其相邻图层进行合成。
* * *

