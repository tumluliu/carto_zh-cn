### 基本概念

_译注：[原文地址](https://www.mapbox.com/tilemill/docs/manual/carto/)_

#### 符号

CartoCSS所基于的地图渲染引擎Mapnik提供了一组基本样式，基于这些基本样式可以配制出复杂的地图样式。这些基本样式被称为**符号**，每种符号都包含一系列属性。

Mapnik中目前包含以下十种符号。每种符号都可以用于对某一种或几种类型的空间数据进行样式配置：

1.	线符号（可用于线要素和面要素）
2.	面符号（可用于面要素）
3.	点符号（可用于点要素）
4.	文本符号（可用于点要素、线要素和面要素）
5.	盾标符号（可用于点要素和线要素）
6.	线图案（可用于线要素和面要素）
7.	面图案（可用于面要素）
8.	栅格符号（可用于栅格数据）
9.	注记符号（可用于点要素、线要素和面要素）
10.	建筑物符号（译注：通常用于面要素）


_需要注意的是，尽管面符号可以用于定制线要素的样式，但往往会出现不可预期的不理想结果，因此不推荐使用。_

Multiple symbolizers can be applied to the same layer - some common combinations are line & polygons, point & text, line & markers, and line & line pattern.

对同一个图层可以同时应用多种符号来定制样式。这种用法我们称之为**多符号**。常用的多符号组合包括：线符号加面符号，点符号、文本符号、线符号加注记符号，以及线符号加线模式等。

A symbolizer is not present on the map unless it has a style defined, but once one of its style properties is added to the stylesheet default values will apply to the other properties for that symbolizer unless overridden. For example, the default line symbolizer color is black, so if you assign a line-width to a layer that line will be black unless you also assign a different color.

一种符号只有在明确定义了它的样式之后才能被绘制在地图上。在每种符号的诸多属性中，除了显式赋值的属性以外，其它属性将全部被设置为默认值。例如，线符号中颜色属性的默认值为黑色，所以如果用户显式设置了线宽，那么图层中的线要素就将以用户设置的宽度被绘制成黑色。

#### 多符号

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
	

![](https://www.mapbox.com/tilemill/assets/manual/symbolizer-1.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/manual/carto/)_

Symbolizers are rendered in the order they are defined, so here the `::glow` (blue line) appears on top of the first style (red line).

在对所定义的符号进行渲染的时候，是按照其在样式脚本中出现的顺序进行的。所以上面例子中的新符号`::glow`（蓝色光晕线）会被绘制在之前定义的红色轮廓线之上。

Named symbolizer styles can still be overridden by further styles that reference the same symbolizer name. In this example, the line color will be green, not green-on-yellow.

具名的新符号样式也同样会有同名覆盖问题，即后定义的新符号会覆盖之前先定义的同名符号的样式设置。在下面的例子中，线的颜色最终将被渲染为绿色（RGB值为#3F6），而不是半透明黄色上叠加一层绿色效果（如图）。

	
	.border::highlight {
	  line-color: #FF0;
	  line-opacity: 0.5;
	}
	
	.border::highlight {
	  line-color: #3F6;
	}
	

![](https://www.mapbox.com/tilemill/assets/manual/symbolizer-2.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/manual/carto/)_


**需要补充以下部分（来自[carto](https://github.com/mapbox/carto)项目的[README](https://github.com/mapbox/carto/blob/master/README.md)）：**


#### 从属样式与实例（Attachments and Instances）

In CSS, a certain object can only have one instance of a property. A `<div>` has a specific border width and color, rules that match better than others (#id instead of .class) override previous definitions. CartoCSS acts the same way normally for the sake of familiarity and organization, but Mapnik itself is more powerful.

Layers in Mapnik can have multiple [borders](#)(http://trac.mapnik.org/wiki/LineSymbolizer) and multiple copies of other attributes. This ability is useful in drawing line outlines, like in the case of road borders or 'glow' effects around coasts. CartoCSS makes this accessible by allowing attachments to styles:

	
	#world {
	  line-color: #fff;
	  line-width: 3;
	}
	
	#world::outline {
	  line-color: #000;
	  line-width: 6;
	}
	

Attachments are optional.

While attachments allow creating implicit "layers" with the same data, using **instances** allows you to create multiple symbolizers in the same style/layer:

	
	#roads {
	  casing/line-width: 6;
	  casing/line-color: #333;
	  line-width: 4;
	  line-color: #666;
	}
	

This makes Mapnik first draw the line of color #333 with a width of 6, and then immediately afterwards, it draws the same line again with width 4 and color #666. Contrast that to attachments: Mapnik would first draw all casings before proceeding to the actual lines.

#### 变量与表达式（Variables & Expressions）

CartoCSS inherits from its basis in [less.js](#)(http://lesscss.org/) some new features in CSS. One can define variables in stylesheets, and use expressions to modify them.

	
	@mybackground: #2B4D2D;
	
	Map {
	  background-color: @mybackground
	}
	
	#world {
	  polygon-fill: @mybackground + #222;
	  line-color: darken(@mybackground, 10%);
	}
	

#### 嵌套样式（Nested Styles）

CartoCSS also inherits nesting of rules from less.js.

	
	/* Applies to all layers with .land class */
	.land {
	  line-color: #ccc;
	  line-width: 0.5;
	  polygon-fill: #eee;
	  /* Applies to #lakes.land */
	  #lakes {
	    polygon-fill: #000;
	  }
	}
	

This can be a convenient way to group style changes by zoom level:

	
	[zoom > 1] {
	  /* Applies to all layers at zoom > 1 */
	  polygon-gamma: 0.3;
	  #world {
	    polygon-fill: #323;
	  }
	  #lakes {
	    polygon-fill: #144;
	  }
	}
	

#### 字体（FontSets）

By defining multiple fonts in a `text-face-name` definition, you create [FontSets](#)(http://trac.mapnik.org/wiki/FontSet) in CartoCSS. These are useful for supporting multiple character sets and fallback fonts for distributed styles.

	
	/* CartoCSS样式*/
	#world {
	  text-name: "[NAME]";
	  text-size: 11;
	  text-face-name: "Georgia Regular", "Arial Italic";
	}
	

	
	/* 编译后的Mapnik XML样式 */
	<FontSet name="fontset-0">
	  <Font face-name="Georgia Regular"/>
	  <Font face-name="Arial Italic"/>
	</FontSet>
	<Style name="world-text">
	  <Rule>
	    <TextSymbolizer fontset-name="fontset-0"
	      size="11"
	      name="[NAME]"/>
	  </Rule>
	</Style>
	

#### 过滤器（Filters）

CartoCSS supports a variety of filter styles:

Numeric comparisons:

	
	#world[population > 100]
	#world[population < 100]
	#world[population >= 100]
	#world[population <= 100]
	

General comparisons:

	
	#world[population = 100]
	#world[population != 100]
	

String comparisons:

	
	/* a regular expression over name */
	#world[name =~ "A.*"]
	


