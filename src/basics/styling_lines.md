### 配置线样式（Styling lines）

_译注：[原文地址](https://www.mapbox.com/mapbox-studio/styling-lines/)_

Line styles can be applied to both line and polygon layers. The simplest line styles have just a line-width (in pixels) and a line-color making a single solid line. The default values for these properties are 1 and black respectively if they are not specified.

线样式既可以用于矢量线要素图层，也可以用于矢量面要素图层的样式配置。最简单的线样式可以只包含线宽（以像素为单位）和颜色。这两个属性的默认值分别为1和黑色。下面的例子中展示了对面要素边界进行样式配置的效果。

![](https://cloud.githubusercontent.com/assets/126952/3893043/893b0c40-2237-11e4-83b5-5fef2e1478ba.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#admin[admin_level=2] {
	  line-width: 0.75;
	  line-color: #426;
	}
	

#### 虚线（Dashed lines）

Simple dashed lines can be created with the line-dasharray property. The value of this property is a comma-separated list of pixel widths that will alternatively be applied to dashes and spaces. This style draws a line with 5 pixel dashes separated by 3 pixel spaces:

简单的虚线可以通过使用`line-dasharray`属性实现。这个属性的值是一个数值列表，列表中的元素以逗号分隔，列表中的元素交替表示虚线中短线和间隔的长度，以像素为单位。下面例子中的虚线就是以5个像素的短线和3个像素的间隔进行绘制的。

![](https://cloud.githubusercontent.com/assets/126952/3893044/893cb6ee-2237-11e4-886b-d35dad27acb2.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#admin[admin_level>=3] {
	  line-width: 0.5;
	  line-color: #426;
	  line-dasharray: 5,3;
	}
	

You can make your dash patterns as complex as you want, with the limitation that the dasharray values must all be whole numbers.

虚线的样式还可以被配置成更复杂的模式，只要`line-dasharray`属性值列表中的元素都是整数就行。

![](https://cloud.githubusercontent.com/assets/126952/3893076/d7d35dda-2237-11e4-99ff-7b04d27e44f4.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#admin[admin_level>=3] {
	  line-width: 0.5;
	  line-color: #426;
	  line-dasharray: 10,3,2,3;
	}
	

#### 端点与交汇点（Caps & Joins）

With thicker line widths you’ll notice long points at sharp angles and odd gaps on small polygons.

当把线宽设置成较大的值时，会出现一些不正常的绘制效果，例如在比较尖锐的拐角处会出现一些伸展得很长的拐点（译注：long points是不是这个意思？），还有在很小的多边形上会出现一些奇怪的缝隙，等等（如下图）。

![](https://cloud.githubusercontent.com/assets/126952/3893195/c3bea344-2238-11e4-9da7-a4c46aba4a74.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#countries::bigoutline {
	  line-color: #9ed1dc;
	  line-width: 20;
	}
	

You can adjust the angles with the `line-join` property: `round` or `square` them off (the default is called `miter`). The gaps can be filled by setting line-cap to `round` or `square` (the default is called `butt`).

要解决这个问题，可以通过将`line-join`属性的值调整为`round`或`square`（其默认值为`miter`）。而可以通过将`line-cap`属性设置为`round`或`square`（默认值为`butt`）来填充不必要的缝隙。

![](https://www.mapbox.com/tilemill/assets/pages/styling-lines-5.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#countries::bigoutline {
	  line-color: #9ed1dc;
	  line-width: 20;
	  line-join: round;
	  line-cap: round;
	}
	

For dashed lines, line-caps are applied to each dash and their additional length is not included in the dasharray definition. Notice how the following style creates almost-solid lines despite the dasharray defining a gap of 4 pixels.

对于虚线，`line-cap`会被应用于所有的短线，但多出来的那部分“线头”却不会被算在`line-dasharray`中定义的短线长度中。在下面这个例子中，尽管在`line-dasharray`中定义了4个像素的短线间隔，但由于使用了圆头短线，所以这些间隔几乎都被填满了，所以整体看起来已经很像是一条实线了（译注：其实更像是一串香肠）。

![](https://www.mapbox.com/tilemill/assets/pages/styling-lines-6.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#layer {
	  line-width: 4;
	  line-cap: round;
	  line-dasharray: 10, 4;
	}
	

#### 复合线样式（Compound line styles）

##### 道路（Roads）

For certain types of line styles you will need to style and overlap multiple line styles. For example, a road with casing:

对一些特定类型的线（如道路网），可以用多层相互压盖的方式来定义样式。例如，要实现一个带有边框效果的道路，可以用下面的方式实现（译注：其中`::case`部分定义了道路的边框，实际上是位于下层，颜色为`#d83`的一条宽线；而`::fill`部分则定义了一条覆盖在宽线上面的一条颜色为`#fe3`的窄线。二者共同实现了一条带有边框的道路。）

![](https://cloud.githubusercontent.com/assets/126952/3893352/0cfd24e4-223a-11e4-80ca-be06b2b036e1.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#road[class='motorway'] {
	  ::case {
	    line-width: 5;
	    line-color: #d83;
	  }
	  ::fill {
	    line-width: 2.5;
	    line-color: #fe3;
	  }
	}
	

Dealing with multiple road classes, things get a little more complicated. You can either group your styles by class or group them by attachment. Here we’ve grouped by class (filtering on the `class` field).

对于不同的级别或类型的道路，样式定义也会相应复杂一些。用户可以按照道路的级别或组成样式的从属样式（译注：attachment在这里是指通过叠加覆盖方式组合而成的某类道路样式的各个从属样式，比如例子中的`::case`和`::fill`）对样式进行分组。下面的例子中的样式是按照道路级别进行分组（基于`class`字段设置过滤器）。

![](https://www.mapbox.com/tilemill/assets/pages/styling-lines-8.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#road {
	  [class='motorway'] {
	    ::case {
	      line-width: 5;
	      line-color: #d83;
	    }
	    ::fill {
	      line-width: 2.5;
	      line-color: #fe3;
	    }
	  }
	  [class='main'] {
	    ::case {
	      line-width: 4.5;
	      line-color: #ca8;
	    }
	    ::fill {
	      line-width: 2;
	      line-color: #ffa;
	    }
	  }
	}
	

##### 铁路（Railroads）

A common way of symbolizing railroad lines is with regular hatches on a thin line. This can be done with two line attachments - one thin and solid, the other thick and dashed. The dash should be short with wide spacing.

一种典型的铁路线样式是在一条细实线上画上一系列垂直于细线的小短线。这种效果可以通过用两个相互叠加的从属线样式实现：一条细实线，还有一条短而粗的虚线。

![](https://cloud.githubusercontent.com/assets/126952/3893425/b8d7178e-223a-11e4-813a-f14390ac3bd6.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#road[class='major_rail'] {
	  ::line, ::hatch { line-color: #777; }
	  ::line { line-width:1; }
	  ::hatch {
	    line-width: 4;
	    line-dasharray: 1, 24;
	  }
	}
	

Another common railroad line style is similar, but with a thin dash and a thick outline. Make sure you define the `::dash` after the `::line` so that it appears on top correctly.

另一种典型的铁路线样式与第一种类似，只是虚线部分中的短线更细更长，而实线部分更粗。配置这种样式的时候需要注意，要把`::dash`部分写在`::line`部分的后面，从而保证虚线样式能够盖在实线部分的上面。

![](https://cloud.githubusercontent.com/assets/126952/3893426/b8da3a5e-223a-11e4-824d-24c1fec600a2.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#road[class='major_rail'] {
	  ::line {
	    line-width: 5;
	    line-color: #777;
	  }
	  ::dash {
	    line-color: #fff;
	    line-width: 2.5;
	    line-dasharray: 6, 6;
	  }
	}
	

##### 隧道（Tunnels）

A simple tunnel style can be created by modifying a regular road style and making the background line dashed.

一种简单的隧道样式可以通过把道路样式稍稍修改而成，具体的，就是把作为背景的底层线条由实线改为虚线，即可得到下图中的渲染效果。

![](https://cloud.githubusercontent.com/assets/126952/3893606/73e3eeac-223c-11e4-83dd-8343f8525513.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#road,
	#bridge {
	  ::case {
	    line-width: 8;
	    line-color:#888;
	  }
	  ::fill {
	    line-width: 5;
	    line-color:#fff;
	  }
	}
	#tunnel {
	  ::case {
	    line-width: 8;
	    line-color:#888;
	    line-dasharray: 4, 3;
	  }
	  ::fill {
	    line-width: 5;
	    line-color:#fff;
	  }
	}
	


#### 用图片实现线模式（Line patterns with Images）

Certain types of line pattens are too complex to be easily achieved with regular compound line styles. TileMill allows you to use repeated images alongside or in place of your other line styles. As an example we’ll make a pattern that we’ll use to represent a cliff. To do this you’ll need to work with external graphics software - we’ll be using Inkscape in this example.

有些线型是非常复杂的，复杂到难以通过常规的复合线样式来实现。CartoCSS（译注：这里用CartoCSS而非原文中的TileMill）允许用户对线要素以沿线重复出现同一张图片的方式来绘制（译注：对这种“元图片”，这里称为线型图案）。在下面的例子中，一张用Inkscape绘制出的图片被用于表示悬崖，并且就以它作为线要素样式中的模式。

In Inkscape (or whatever you are using), create a new document. The size should be rather small - the height of the image will be the width of the line pattern and the width of the image will be repeated along the length of the line. Our example is 30x16 pixels.

在Inkscape（或者其它矢量图形编辑软件）中，创建一个新的矢量图形，尺寸不能太大，其高度相当于线宽，宽度就是它沿线重复出现的单位。下面例子中的悬崖图形尺寸为30x16像素。

![](https://cloud.githubusercontent.com/assets/126952/3893643/d05c41fc-223c-11e4-968f-53eb8d2713a8.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

Note how the centerline of the pattern is centered on the image (with empty space at the top) for correct alignment with the line data.

请注意这个图形在设计过程中的对齐方式和上下留白的宽度，只有这样才能保证它画在线上可以对齐。

To use the image from Inkscape, export it as a PNG file. Line patterns just need a single CartoCSS style to be added to your TileMill project:

将这个图片从Inkscape中导出成PNG文件。然后如果要在CartoCSS中使用它的话，只需增加一个包含`line-pattern-file`属性的样式块。

![](https://cloud.githubusercontent.com/assets/126952/3893795/0039a7d8-223e-11e4-92b6-253ccf826af3.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#barrier_line[class='cliff'] {
	  line-pattern-file: url(cliff.png);
	}
	

For some types of patterns, such as the cliff in this example, the direction of the pattern is important. The bottom of line pattern images will be on the right side of lines. The left side of the image will be at the beginning of the line.

对于某些图案，比如上面例子中的悬崖，它的方向非常重要。CartoCSS中的约定是：图案图片的底部会被置于线要素的右侧；而图案图片的左侧会被置于线要素的起始位置。
