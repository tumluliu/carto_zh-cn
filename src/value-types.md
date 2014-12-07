## 2.14 关于取值类型的说明

这里列出了higis制图语言中所有属性的取值类型及其说明。

### 颜色型（Color）

higis制图语言可以使用一系列不同的方法表示颜色：HTML风格的16进制值，RGB值，RGBA值，HSL值或HSLA值都可以，还可以使用HTML预定义颜色名，像`yellow`、`blue`等。

	
	#line {
	 line-color: #ff0;
	 line-color: #ffff00;
	 line-color: rgb(255, 255, 0);
	 line-color: rgba(255, 255, 0, 1);
	 line-color: hsl(100, 50%, 50%);
	 line-color: hsla(100, 50%, 50%, 1);
	 line-color: yellow;
	}
	

这里特别需要强调的是对HSL值的支持，这其实是比RGB值更易用的颜色表达方式（参见[这里](http://mothereffinghsl.com/)）。higis制图语言中还支持几种颜色函数，这是从LESS中借用的概念（参见[这里](http://lesscss.org/#-color-functions)），例子如下：

	
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
	

以上这些函数的参数可以是颜色值，也可以是颜色名，还可以是其它颜色函数。

### 浮点型（Float）

浮点数是数值类型的时髦说法。在higis制图语言中，这指的就是一个数值，没有单位，但其实所有的单位都是像素。

	
	#line {
	 line-width: 2;
	}
	

还可以对数值类型做简单运算：

	
	#line {
	 line-width: 4 / 2; // 除
	 line-width: 4 + 2; // 加
	 line-width: 4 - 2; // 减
	 line-width: 4 * 2; // 乘
	 line-width: 4 % 2; // 取余
	}
	

### 统一资源描述符型（URI）

当一个属性的值类型是URI时，用户可以像在HTML中使用`url('place.png')`一样的表示方法。URL地址上的引号不是必需的，但最好加上。URI可以指向本地文件系统，也可以是互联网上资源的链接地址。

	
	#markers {
	 marker-file: url('marker.png');
	}
	

### 字符串型（String）

字符串也就是文本类型。在higis制图语言中，字符串应该有引号包围。字符串可以是任意文本，但在`text-name`和`shield-name`属性中，可以使用中括号包围的数据字段名来表示。例如：

	
	#labels {
	 text-name: "[MY_FIELD]";
	}
	

### 布尔型（Boolean）

布尔类型即是或否，取值为`true`或`false`。

	
	#markers {
	 marker-allow-overlap:true;
	}
	

### 表达式型（Expressions）

表达式是一种语句，它可以将数据字段、数值以及其它类型灵活的组合起来。前面提到的`"[FIELD]"`形式包含了表达式。实际的表达式可以不用加引号就执行加、减、乘、除、连接等higis制图语言语法支持的操作。

	
	#buildings {
	 building-height: [HEIGHT_FIELD] * 10;
	}
	

### 数列型（Numbers）

数列型是逗号分隔的一组有序数值。数列类型在用于配置虚线样式时，其中的数字交替表示的是实线段长度、间隔长度和实线段长度。

	
	#disputedboundary {
	 line-dasharray: 1, 4, 2;
	}
	

### 百分数型（Percentages）

在higis制图语言中，百分号`%`表示`值/100`。它可以用于表示比例的属性，例如透明度。

_注意，百分数不能用于定义宽度、高度等属性。这一点与CSS不同，因为在higis制图语言中没有CSS中层次化的页面要素和页宽。它们在这里只是除以100以后的值。_

	
	#world {
	 // 这种表达方式与...
	 polygon-opacity: 50%;
	
	 // ...这种方式效果一样
	 polygon-opacity: 0.5;
	}
	

### 函数型（Functions）

这种类型可以包含一组逗号分隔的函数。例如，各种变换都是用`functions`作为值类型，而且这些函数还可以串接起来。

	
	#point {
	 point-transform: scale(2, 2);
	}
	
