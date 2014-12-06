# CartoCSS概述

CartoCSS是一种语法类似CSS的制图样式描述语言。层叠样式表（Cascading Style Sheets，CSS）是一种对网页进行设计的样式语言。如果熟悉CSS的话，那么CartoCSS这种对地图进行样式设计的语言也会看起来不陌生，尽管二者所包含的要素、属性等内容和含义完全不同。

## 符号

higis中的地图渲染引擎提供了一组基本样式，基于这些基本样式可以构造出复杂的地图样式。这些基本样式被称为**符号**，每种符号都有一系列可配置的属性。

higis中目前包含10种符号。每一种符号都可以应用于以下某一类或几类几何要素：

1.	线符号（可用于线要素和面要素）
2.	面符号（可用于面要素）
3.	点符号（可用于点要素）
4.	文本符号（可用于点要素、线要素和面要素）
5.	盾标符号（可用于点要素和线要素）
6.	线模式（可用于线要素和面要素）
7.	面模式（可用于面要素）
8.	栅格符号（可用于栅格）
9.	注记符号（可用于点要素、线要素和面要素）
10.	建筑物符号


需要注意的是，尽管面符号可以用于定制线要素的样式，但往往会出现不可预期的不理想结果，因此不推荐使用。

Multiple symbolizers can be applied to the same layer - some common combinations are line & polygons, point & text, line & markers, and line & line pattern.

对同一个图层可以同时应用多种符号来定制样式。这种用法我们称之为“多符号”。常用的多符号组合包括：线符号加面符号，点符号、文本符号、线符号加注记符号，以及线符号加线模式等。

A symbolizer is not present on the map unless it has a style defined, but once one of its style properties is added to the stylesheet default values will apply to the other properties for that symbolizer unless overridden. For example, the default line symbolizer color is black, so if you assign a line-width to a layer that line will be black unless you also assign a different color.

一种符号只有在明确定义了它的样式之后才能被绘制在地图上。在每种符号的诸多属性中，除了显式赋值的属性以外，其它属性将全部被设置为默认值。例如，线符号中颜色属性的默认值为黑色，所以如果用户显式设置了线宽，那么图层中的线要素就将以用户设置的宽度被绘制成黑色。

## 多符号

A single layer is not limited to one of each symbolizer type. For example, multiple semi-transparent line symbolizers can be assigned to a polygon to achieve a soft glow or shadow effect. Multiple text symbolizers can be assigned to the same point with different offsets to label it with more than one field.

对一个图层来说，它的样式可以不局限于仅使用单一的某种符号来定制。举例来说，为了在多边形的边界上获得一种柔和的光晕或阴影效果，可以定义多个半透明线符号，共同发挥作用达到渲染效果。再举一例，对点要素，可以通过定义多个文本符号将若干个属性字段以不同偏移的形式标注在点要素的周围。

Normally when you assign a style to a layer, the style applies to a default symbolizer that is created. In the following example, the second rule overrides the first one because they both apply to the default symbolizer.

通常，如果对一个图层定义了一种样式，那么这种样式就会应用于一种默认的符号。在下面的例子中，后一个样式规则就会将前一个覆盖，因为二者都应用了相同的默认符号，即线符号。


	#layer {
	  line-color: #C00;
	  line-width: 1;
	}
	
	#layer {
	  line-color: #0AF;
	  line-opacity: 0.5;
	  line-width: 2;
	}

You can explicitly declare any number of new symbolizers for a layer that will be rendered in addition to styles they would otherwise conflict with. New symbolizers are defined using a double colon syntax inspired by pseudo-elements in CSS3:

用户可以通过显式声明的方式为一个图层增加任意数量的**新符号**。由这些新符号所定义的样式之间只要不互相冲突，那么它们都将被用于渲染该图层。为图层定义新符号使用双冒号“::”语法，与CSS3中的伪元素定义类似：


	#layer {
	  /* styles for the default symbolizers */
	}
	
	#layer::newsymbol {
	  /* styles for a new symbolizer named ‘newsymbol’ */
	}

Note that newsymbol is not a special keyword but an arbitrary name chosen by the user. To help keep track of different symbolizers you can name additional symbolizers whatever makes sense for the situation. Some examples: `#road::casing`, `#coastline::glow_inner`, `#building::shadow`.

注意上面例子中的newsymbol不是关键字。用户可以为新符号自定义名字，但是为了便于理解，最好取一些有意义的名字，例如：`#road::casing`, `#coastline::glow_inner`, `#building::shadow`。

Returning to our previous example, declaring the second rule will add a blue glow on top of the red line instead of replacing it:

在上一个例子中，我们可以通过再声明一个新符号来实现一个蓝色光晕效果。而正是通过增加了这个新符号的声明，使得蓝色光晕能够被叠加渲染在之前的红色轮廓线之上，而不是覆盖了前面红色线样式（如图）。

	#layer {
	  line-color: #C00;
	  line-width: 1;
	}
	
	#layer::glow {
	  line-color: #0AF;
	  line-opacity: 0.5;
	  line-width: 4;
	}

![](symbolizer-1.png)

Symbolizers are rendered in the order they are defined, so here the `::glow` (blue line) appears on top of the first style (red line).

在对所定义的符号进行渲染的时候，是按照其在样式脚本中出现的顺序进行的。所以上面例子中的新符号`::glow`（蓝色光晕线）会被绘制在之前定义的红色轮廓线之上。

Named symbolizer styles can still be overridden by further styles that reference the same symbolizer name. In this example, the line color will be green, not green-on-yellow.

**具名符号**样式也同样会有同名覆盖问题，即后定义的具名符号会覆盖之前先定义的同名具名符号的样式设置。在下面的例子中，线的颜色最终将被渲染为绿色（RGB值为#3F6），而不是半透明黄色上叠加一层绿色效果（如图）。

	.border::highlight {
	  line-color: #FF0;
	  line-opacity: 0.5;
	}
	
	.border::highlight {
	  line-color: #3F6;
	}

![](symbolizer-2.png)

