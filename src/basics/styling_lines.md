### 3.3 配置线样式

线样式既可以用于矢量线要素图层，也可以用于矢量面要素图层的样式配置。最简单的线样式可以只包含线宽（以像素为单位）和颜色。这两个属性的默认值分别为1和黑色。下面的例子中展示了对面要素边界进行样式配置的效果。

![](https://cloud.githubusercontent.com/assets/126952/3893043/893b0c40-2237-11e4-83b5-5fef2e1478ba.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#admin[admin_level=2] {
	  line-width: 0.75;
	  line-color: #426;
	}
	

#### 虚线

简单的虚线可以通过使用`line-dasharray`属性实现。这个属性的值是一个数值列表，列表中的元素以逗号分隔，列表中的元素交替表示虚线中短线和间隔的长度，均以像素为单位。下面例子中的虚线就是以5像素长的短线和3像素长的间隔进行绘制的。

![](https://cloud.githubusercontent.com/assets/126952/3893044/893cb6ee-2237-11e4-886b-d35dad27acb2.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#admin[admin_level>=3] {
	  line-width: 0.5;
	  line-color: #426;
	  line-dasharray: 5,3;
	}
	

还可以将虚线配置成更复杂的样子，但要求`line-dasharray`属性值列表中的元素必须都得是整数。

![](https://cloud.githubusercontent.com/assets/126952/3893076/d7d35dda-2237-11e4-99ff-7b04d27e44f4.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#admin[admin_level>=3] {
	  line-width: 0.5;
	  line-color: #426;
	  line-dasharray: 10,3,2,3;
	}
	

#### 端点与交汇点

当把线宽设得比较大时，会出现一些不正常的绘制效果，例如在比较尖锐的拐角处会出现一些伸展得很长的拐点（译注：long points是不是这个意思？），还有在很小的多边形上会出现一些奇怪的缝隙，等等（如下图）。

![](https://cloud.githubusercontent.com/assets/126952/3893195/c3bea344-2238-11e4-9da7-a4c46aba4a74.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#countries::bigoutline {
	  line-color: #9ed1dc;
	  line-width: 20;
	}
	

要解决前一个问题，可以通过将`line-join`属性的值调整为`round`或`square`（其默认值为`miter`）。而对于第二个问题，可以通过将`line-cap`属性设置为`round`或`square`（默认值为`butt`）来填充不必要的缝隙。

![](https://www.mapbox.com/tilemill/assets/pages/styling-lines-5.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#countries::bigoutline {
	  line-color: #9ed1dc;
	  line-width: 20;
	  line-join: round;
	  line-cap: round;
	}
	

对于虚线，`line-cap`会被应用于所有的短线，但多出来的那部分“线头”却不会被算在`line-dasharray`中定义的短线长度中。在下面这个例子中，尽管在`line-dasharray`中定义了4个像素的短线间隔，但由于使用了圆头短线，所以这些间隔几乎都被填满了，所以整体看起来已经很像是一条实线了（译注：其实更像是一串香肠）。

![](https://www.mapbox.com/tilemill/assets/pages/styling-lines-6.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-lines/)_

	
	#layer {
	  line-width: 4;
	  line-cap: round;
	  line-dasharray: 10, 4;
	}
	

#### 复合线样式

##### 道路

For certain types of line styles you will need to style and overlap multiple line styles. For example, a road with casing:

对一些特定类型的线（如道路网），可以利用多个从属样式相互压盖来实现较为复杂的样式。例如，要实现一个带有边框效果的道路，可以用下面的方式实现。其中`::case`部分定义了道路的边框，实际上是位于下层，颜色为`#d83`的一条宽线；而`::fill`部分则定义了一条覆盖在宽线上面的一条颜色为`#fe3`的窄线。这两个从属样式共同实现了一条带有边框的道路。

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
	

对于不同的级别或类型的道路，样式定义也会相应复杂一些。用户可以按照道路的级别或组成样式的从属样式（即例子中的`::case`和`::fill`部分）对样式进行分组。下面的例子中的样式是按照道路级别进行分组（基于`class`字段设置过滤器）。

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
	

##### 铁路

一种典型的铁路线样式是在一条细实线上等间隔的画上一系列垂直于细线的小短线。这种效果可以通过用两个相互叠加的从属线样式实现：一条细实线，还有一条短而宽且间隔较长的虚线。

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
	

另一种典型的铁路线样式与第一种类似，只是要把上层的虚线部分中的短线稍微调细一些和长一些，而把下层的实线部分调得更粗一些以形成铁路线的边框。配置这种样式的时候需要注意，要把`::dash`部分写在`::line`部分的后面，从而保证虚线样式能够盖在实线部分的上面。

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
	

##### 隧道

一种简单的隧道样式可以通过把道路样式稍稍修改而成。具体来说，就是把作为背景的底层线条由实线改为虚线，即可得到下图中的渲染效果。

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
	


#### 用图片作为线型图案

有些线型是非常复杂的，复杂到难以通过常规的复合线样式来实现。CartoCSS允许用户对线要素以沿线重复出现同一张图片的方式来绘制。在下面的例子中，一张用Inkscape绘制出的图片被用于表示悬崖，并且就以它作为线要素样式中的基本图案。

在Inkscape（或者其它矢量图形编辑软件）中，创建一个新的矢量图形，尺寸不能太大，其高度相当于线宽，宽度就是它沿线重复出现的单位。下面例子中的悬崖图形尺寸为30x16像素。

![](https://cloud.githubusercontent.com/assets/126952/3893643/d05c41fc-223c-11e4-968f-53eb8d2713a8.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

请注意这个图形在设计过程中的对齐方式和上下留白的宽度，只有这样才能保证它画在线上可以对齐。将这个图片从Inkscape中导出成PNG文件，然后在CartoCSS中通过增加一个包含`line-pattern-file`属性的样式块来使用它。

![](https://cloud.githubusercontent.com/assets/126952/3893795/0039a7d8-223e-11e4-92b6-253ccf826af3.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-lines/)_

	
	#barrier_line[class='cliff'] {
	  line-pattern-file: url(cliff.png);
	}
	

对于某些图案，比如上面例子中的悬崖，它的方向非常重要。CartoCSS中的约定是：图案图片的底部会被置于线要素（沿线方向）的右侧；而图案图片的左侧会与线要素的起始位置对准。

#### 参考文献

1. Mapbox, [Mapbox Studio Styling Lines](https://www.mapbox.com/mapbox-studio/styling-lines/)
