### 关于取值类型的说明

Below is a list of values and an explanation of any expression that can be applied to properties in CartCSS.

这里列出了CartoCSS中所有属性的取值类型及其说明。

#### 颜色型（Color）

CartoCSS accepts a variety of syntaxes for colors - HTML-style hex values, rgb, rgba, hsl, and hsla. It also supports the predefined HTML colors names, like `yellow` and `blue`.

CartoCSS可以使用一系列不同的方法表示颜色：HTML风格的16进制值，RGB值，RGBA值，HSL值或HSLA值都可以，还可以使用HTML预定义颜色名，像`yellow`、`blue`等。

	
	#line {
	 line-color: #ff0;
	 line-color: #ffff00;
	 line-color: rgb(255, 255, 0);
	 line-color: rgba(255, 255, 0, 1);
	 line-color: hsl(100, 50%, 50%);
	 line-color: hsla(100, 50%, 50%, 1);
	 line-color: yellow;
	}
	

Especially of note is the support for hsl, which can be [easier to reason about than rgb()](#)(http://mothereffinghsl.com/). Carto also includes several color functions [borrowed from less](#)(http://lesscss.org/#-color-functions):

这里特别需要强调的是对HSL值的支持，这其实是比RGB值更易用的颜色表达方式（参见[这里](http://mothereffinghsl.com/)）。CartoCSS中还支持几种颜色函数，这是从LESS中借用的概念（参见[这里](http://lesscss.org/#-color-functions)），例子如下：

	
	// 把颜色调亮或调暗
	lighten(#ace, 10%);
	darken(#ace, 10%);
	
	// 饱和度调高或调低
	saturate(#550000, 10%);
	desaturate(#00ff00, 10%);
	
	// 提高或降低颜色的透明度
	fadein(#fafafa, 10%);
	fadeout(#fefefe, 14%);
	
	// 按照一定角度旋转色盘
	spin(#ff00ff, 10);
	
	// 将两种颜色混合
	mix(#fff, #000, 50%);
	

These functions all take arguments which can be color variables, literal colors, or the results of other functions operating on colors.

以上这些函数的参数可以是颜色值，也可以是颜色名，还可以是其它颜色函数。


#### 浮点型（Float）

Float is a fancy way of saying 'number'. In CartoCSS, you specify _just a number_ - unlike CSS, there are no units, but everything is specified in pixels.

浮点数是数值类型的时髦说法。在CartoCSS中，这指的就是一个数值，没有单位，但其实所有的单位都是像素。

	
	#line {
	 line-width: 2;
	}
	

It's also possible to do simple math with number values:
还可以对数值类型做简单运算：

	
	#line {
	 line-width: 4 / 2; // 除
	 line-width: 4 + 2; // 加
	 line-width: 4 - 2; // 减
	 line-width: 4 * 2; // 乘
	 line-width: 4 % 2; // 取余
	}
	

#### 统一资源描述符型（URI）

URI is a fancy way of saying URL. When an argument is a URI, you use the same kind of `url('place.png')` notation that you would with HTML. Quotes around the URL aren't required, but are highly recommended. URIs can be paths to places on your computer, or on the internet.

URI是URL的一种时髦说法（译注：这实在不敢苟同，在http协议和REST架构中，URI和URL都有明确的定义，它们是**不同的**）。当一个属性的值类型是URI时，用户可以像在HTML中使用`url('place.png')`一样的表示方法。URL地址上的引号不是必需的，但最好加上。URI可以指向本地文件系统，也可以是互联网上资源的链接地址。


	
	#markers {
	 marker-file: url('marker.png');
	}
	

#### 字符串型（String）

A string is basically just text. In the case of CartoCSS, you're going to put it in quotes. Strings can be anything, though pay attention to the cases of `text-name` and `shield-name` - they actually will refer to features, which you refer to by putting them in brackets, as seen in the example below.

字符串也就是文本类型。在CartoCSS中，字符串应该有引号包围。字符串可以是任意文本，但在`text-name`和`shield-name`属性中，可以使用中括号包围的数据字段名来表示。例如：

	
	#labels {
	 text-name: "[MY_FIELD]";
	}
	

#### 布尔型（Boolean）

Boolean means yes or no, so it accepts the values `true` or `false`.
布尔类型即是或否，取值为`true`或`false`。

	
	#markers {
	 marker-allow-overlap:true;
	}
	

#### 表达式型（Expressions）

Expressions are statements that can include fields, numbers, and other types in a really flexible way. You have run into expressions before, in the realm of 'fields', where you'd specify `"[FIELD]"`, but expressions allow you to drop the quotes and also do quick addition, division, multiplication, and concatenation from within Carto syntax.

表达式是一种语句，它可以将数据字段、数值以及其它类型灵活的组合起来。前面提到的`"[FIELD]"`形式包含了表达式。实际的表达式可以不用加引号就执行加、减、乘、除、连接等CartoCSS语法支持的操作。

	
	#buildings {
	 building-height: [HEIGHT_FIELD] * 10;
	}
	

#### 数列型（Numbers）
Numbers are comma-separated lists of one or more number in a specific order. They're used in line dash arrays, in which the numbers specify intervals of line, break, and line again.
数列型是逗号分隔的一组有序数值。数列类型在用于配置虚线样式时，其中的数字交替表示的是实线段长度、间隔长度和实线段长度。

	
	#disputedboundary {
	 line-dasharray: 1, 4, 2;
	}
	

#### 百分数型（Percentages）
In Carto, the percentage symbol, `%` universally means `value/100`. It's meant to be used with ratio-related properties, like opacity rules.
在CartoCSS中，百分号`%`表示`值/100`。它可以用于表示比例的属性，例如透明度。

_You should not use percentages as widths, heights, or other properties - unlike CSS, percentages are not relative to cascaded classes or page size, they're, as stated, simply the value divided by one hundred._

_注意，百分数不能用于定义宽度、高度等属性。这一点与CSS不同，因为在CartoCSS中没有CSS中层次化的页面要素和页宽。它们在这里只是除以100以后的值。_

	
	#world {
	 // 这种表达方式与...
	 polygon-opacity: 50%;
	
	 // ...这种方式效果一样
	 polygon-opacity: 0.5;
	}
	

#### 函数型（Functions）

Functions are comma-separated lists of one or more functions. For instance, transforms use the `functions` type to allow for transforms within Carto, which are optionally chainable.
这种类型可以包含一组逗号分隔的函数。例如，各种变换都是用`functions`作为值类型，而且这些函数还可以串接起来（？）。

	
	#point {
	 point-transform: scale(2, 2);
	}
	
