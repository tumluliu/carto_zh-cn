## 文本符号（text）的属性

##### text-name `expression`


默认值： 


Value to use for a text label. Data columns are specified using brackets like [column\_name](#)
* * *

##### text-face-name `string`


默认值： undefined


Font name and style to render a label in
* * *

##### text-size `float`


默认值： 10


Text size in pixels
* * *

##### text-ratio `unsigned`


默认值： 0


Define the amount of text (of the total) present on successive lines when wrapping occurs
* * *

##### text-wrap-width `unsigned`


默认值： 0


Length of a chunk of text in characters before wrapping text
* * *

##### text-wrap-before `boolean`


默认值： false


Wrap text before wrap-width is reached. If false, wrapped lines will be a bit longer than wrap-width.
* * *

##### text-wrap-character `string`


默认值：  


Use this character instead of a space to wrap long text.
* * *

##### text-spacing `unsigned`


默认值： undefined


Distance between repeated text labels on a line (aka. label-spacing)
* * *

##### text-character-spacing `float`


默认值： 0


Horizontal spacing adjustment between characters in pixels
* * *

##### text-line-spacing `unsigned`


默认值： 0


Vertical spacing adjustment between lines in pixels
* * *

##### text-label-position-tolerance `unsigned`


默认值： 0


Allows the label to be displaced from its ideal position by a number of pixels (only works with placement:line)
* * *

##### text-max-char-angle-delta `float`


默认值： 22.5


The maximum angle change, in degrees, allowed between adjacent characters in a label. This value internally is converted to radians to the default is 22.5*math.pi&#x2F;180.0. The higher the value the fewer labels will be placed around around sharp corners.
* * *

##### text-fill `color`


默认值： #000000


Specifies the color for the text
* * *

##### text-opacity `float`


默认值： 1
_(Fully opaque)_

A number from 0 to 1 specifying the opacity for the text
* * *

##### text-halo-fill `color`


默认值： #FFFFFF
_(white)_

Specifies the color of the halo around the text.
* * *

##### text-halo-radius `float`


默认值： 0
_(no halo)_

Specify the radius of the halo in pixels
* * *

##### text-halo-rasterizer `keyword`
`full``fast`

默认值： full


Exposes an alternate text halo rendering method that sacrifices quality for speed.
* * *

##### text-dx `float`


默认值： 0


Displace text by fixed amount, in pixels, +&#x2F;- along the X axis.  A positive value will shift the text right
* * *

##### text-dy `float`


默认值： 0


Displace text by fixed amount, in pixels, +&#x2F;- along the Y axis.  A positive value will shift the text down
* * *

##### text-vertical-alignment `keyword`
`top``middle``bottom``auto`

默认值： auto
_(Default affected by value of dy; &quot;bottom&quot; for dy&gt;0, &quot;top&quot; for dy&lt;0.)_

Position of label relative to point position.
* * *

##### text-avoid-edges `boolean`


默认值： false


Avoid placing labels that intersect with tile boundaries.
* * *

##### text-min-distance `float`


默认值： undefined


Minimum permitted distance to the next text symbolizer.
* * *

##### text-min-padding `float`


默认值： undefined


Minimum distance a text label will be placed from the edge of a metatile.
* * *

##### text-min-path-length `float`


默认值： 0
_(place labels on all paths)_

Place labels only on paths longer than this value.
* * *

##### text-allow-overlap `boolean`


默认值： false
_(Do not allow text to overlap with other text - overlapping markers will not be shown.)_

Control whether overlapping text is shown or hidden.
* * *

##### text-orientation `expression`


默认值： undefined


Rotate the text.
* * *

##### text-placement `keyword`
`point``line``vertex``interior`

默认值： point


Control the style of placement of a point versus the geometry it is attached to.
* * *

##### text-placement-type `keyword`
`dummy``simple`

默认值： dummy


Re-position and&#x2F;or re-size text to avoid overlaps. &quot;simple&quot; for basic algorithm (using text-placements string,) &quot;dummy&quot; to turn this feature off.
* * *

##### text-placements `string`


默认值： 


If &quot;placement-type&quot; is set to &quot;simple&quot;, use this &quot;POSITIONS,[SIZES](#)&quot; string. An example is `text-placements: &quot;E,NE,SE,W,NW,SW&quot;;` 
* * *

##### text-transform `keyword`
`none``uppercase``lowercase``capitalize`

默认值： none


Transform the case of the characters
* * *

##### text-horizontal-alignment `keyword`
`left``middle``right``auto`

默认值： auto


The text&#x27;s horizontal alignment from its centerpoint
* * *

##### text-align `keyword`
`left``right``center``auto`

默认值： auto
_(Auto alignment means that text will be centered by default except when using the `placement-type` parameter - in that case either right or left justification will be used automatically depending on where the text could be fit given the `text-placements` directives)_

Define how text is justified
* * *

##### text-clip `boolean`


默认值： true
_(geometry will be clipped to map bounds before rendering)_

geometries are clipped to map bounds by default for best rendering performance. In some cases users may wish to disable this to avoid rendering artifacts.
* * *

##### text-comp-op `keyword`
`clear``src``dst``src-over``dst-over``src-in``dst-in``src-out``dst-out``src-atop``dst-atop``xor``plus``minus``multiply``screen``overlay``darken``lighten``color-dodge``color-burn``hard-light``soft-light``difference``exclusion``contrast``invert``invert-rgb``grain-merge``grain-extract``hue``saturation``color``value`

默认值： src-over
_(add the current symbolizer on top of other symbolizer)_

Composite operation. This defines how this symbolizer should behave relative to symbolizers atop or below it.
* * *

