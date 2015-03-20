### 3.1 基本概念

#### 符号

CartoCSS所基于的地图渲染引擎Mapnik提供了一组基本样式，基于这些基本样式可以配制出复杂的地图样式。这些基本样式被称为**符号**，每种符号都包含一系列属性。

Mapnik中目前包含以下十种符号。每种符号都可以用于对某一种或几种类型的空间数据进行样式配置：

1. 线符号（可用于线要素和面要素）
2. 面符号（可用于面要素）
3. 点符号（可用于点要素）
4. 文本符号（可用于点要素、线要素和面要素）
5. 盾标符号（可用于点要素和线要素）
6. 线图案（可用于线要素和面要素）
7. 面图案（可用于面要素）
8. 栅格符号（可用于栅格数据）
9. 注记符号（可用于点要素、线要素和面要素）
10. 建筑物符号（译注：通常用于面要素）	

_需要注意的是，尽管面符号可以用于定制线要素的样式，但往往会出现不可预期的不理想结果，因此不推荐使用。_

对同一个图层可以同时应用多种符号来定制样式。这种用法我们称之为**多符号**。常用的多符号组合包括：线符号加面符号，点符号、文本符号、线符号加注记符号，以及线符号加线图案等。

一种符号只有在明确定义了它的样式之后才能被绘制在地图上。在每种符号的诸多属性中，除了显式赋值的属性以外，其它属性将全部被赋予默认值。例如，线符号中颜色属性的默认值为黑色，所以如果用户只显式设置了线宽，那么图层中的线要素就将以用户设置的宽度被绘制成黑色线条。

#### 多符号

对一个图层来说，它的样式可以不局限于仅使用单一的某种符号来定制。举例来说，为了在多边形的边界上获得一种柔和的光晕或阴影效果，可以定义多个半透明线符号，共同发挥作用达到渲染效果。再举一例，对点要素，可以通过定义多个文本符号将若干个属性字段以不同偏移的形式散布标注在点要素的周围。

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
	

用户可以通过显式声明的方式为一个图层增加任意数量的新符号。由这些新符号所定义的样式之间只要不互相冲突，那么它们都将被用于渲染该图层。为图层定义新符号使用双冒号`::`语法，与CSS3中的伪元素定义类似。这里所谓“新符号”也被称为“从属样式”（下文详细介绍）：

	
	#layer {
	  /* 定义默认符号的样式 */
	}
	
	#layer::newsymbol {
	  /* 定义名为'newsymbol'的新符号的样式 */
	}
	

注意上面例子中的`newsymbol`并不是CartoCSS关键字。用户可以为新符号自定义名字，但是为了便于理解，最好取一些有意义的名字，例如：`#road::casing`, `#coastline::glow_inner`, `#building::shadow`。

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

在对所定义的符号进行渲染的时候，是按照其在样式脚本中出现的顺序进行的。所以上面例子中的新符号`::glow`（蓝色光晕线）会被绘制在之前定义的红色轮廓线之上。

具名的新符号样式也同样会有同名覆盖问题，即后定义的新符号会覆盖之前先定义的同名符号的样式设置。在下面的例子中，线的颜色最终将被渲染为绿色（RGB值为`#3F6`），而不是半透明黄色上叠加一层绿色效果（如图）。

	
	.border::highlight {
	  line-color: #FF0;
	  line-opacity: 0.5;
	}
	
	.border::highlight {
	  line-color: #3F6;
	}
	

![](https://www.mapbox.com/tilemill/assets/manual/symbolizer-2.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/manual/carto/)_

#### 从属样式与实例

在CSS中，对象的属性只能有一个明确定义的值。比如一个`<div>`，它具有明确定义的边界宽度和颜色。CSS中对象属性的定义满足更精确匹配原则（例如`#id`的匹配度比`.class`要高），匹配度较高的属性定义会覆盖匹配度较低的。为了尽量与CSS保持相似，CartoCSS也秉承了相同的规则。但实际上，作为底层渲染引擎的Mapnik具有更强大的能力，支持更丰富的特性。

Mapnik中的图层可以有很多个[边界](https://github.com/mapnik/mapnik/wiki/LineSymbolizer)等属性。这种能力在绘制带有边框的线条（例如道路线上的边框，海岸线上的光晕效果等）时非常有用。在CartoCSS中，这种能力通过定义**从属样式**（attachments）来实现。当然，从属样式不是必需的。

	
	#world {
	  line-color: #fff;
	  line-width: 3;
	}
	
	#world::outline {
	  line-color: #000;
	  line-width: 6;
	}
	

从属样式实际上是从同一个数据集上创建出了隐式图层。与从属样式对应的是**实例**。实例的作用是在同一个图层样式块中创建多个符号。

	
	#roads {
	  casing/line-width: 6;
	  casing/line-color: #333;
	  line-width: 4;
	  line-color: #666;
	}
	

上面这种实例定义在Mapnik中的对应行为是先将道路线按照颜色`#333`宽度`6`进行绘制，然后立即用颜色`#666`和宽度`4`把同一条道路线再绘一次。而如果是从属样式，Mapnik就会先用边框的样式将整个道路图层绘制一遍，然后再按照实际线样式绘制道路。

#### 变量与表达式

CartoCSS的设计受到了[less.js](http://lesscss.org/)的很多启发，支持CSS中的一些新特性。用户可以在样式表中定义变量，还可以用表达式修改变量。

	
	@mybackground: #2B4D2D;
	
	Map {
	  background-color: @mybackground
	}
	
	#world {
	  polygon-fill: @mybackground + #222;
	  line-color: darken(@mybackground, 10%);
	}
	

#### 嵌套样式

CartoCSS也像less.js一样支持嵌套规则。

	
	/* 以下样式块将应用在所有class为.land的图层上 */
	.land {
	  line-color: #ccc;
	  line-width: 0.5;
	  polygon-fill: #eee;
	  /* 应用在#lakes.land图层上 */
	  #lakes {
	    polygon-fill: #000;
	  }
	}
	

这可以方便的将样式根据缩放级别进行分组：

	
	[zoom > 1] {
	  /* 以下样式块在zoom大于1时将应用在所有图层上 */
	  polygon-gamma: 0.3;
	  #world {
	    polygon-fill: #323;
	  }
	  #lakes {
	    polygon-fill: #144;
	  }
	}
	

#### 字体集

通过`text-face-name`属性可以对地图中使用的字体集进行设置，这一设置在底层对应的是Mapnik中的[FontSet](https://github.com/mapnik/mapnik/wiki/FontSet)。它的作用是通过定义多个字体来保证当地图中包含多种语言的文字时（比如有英文和中文），如果用一种字体渲染不出来某些字符，那么就会尝试用其它字体进行渲染。

	
	/* CartoCSS样式 */
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
	

#### 过滤器

CartoCSS支持多种过滤器类型，其中有数值类型的过滤器：

	
	#world[population > 100]
	#world[population < 100]
	#world[population >= 100]
	#world[population <= 100]
	

有常规类型的过滤器（等于，不等于运算）：

	
	#world[population = 100]
	#world[population != 100]
	

还有字符串类型的过滤器：

	
	/* 在name字段上应用正则表达式进行匹配 */
	#world[name =~ "A.*"]
	

#### 参考文献

1. Mapbox, [CartoCSS](https://www.mapbox.com/tilemill/docs/manual/carto/)
2. carto Project Readme, [carto README.md](https://github.com/mapbox/carto/blob/master/README.md)
