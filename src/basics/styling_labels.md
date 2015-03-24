### 3.5 配置标注样式

#### 简单点标注

在CartoCSS中，文本标注的样式由一系列以`text-`开头的属性来配置。在这些属性中，`text-name`和`text-face-name`是必需的。前者用于指定显示在标注中的文本内容；而后者用于指定显示文本所用的字体。`text-name`属性可以将图层中的属性数据字段提取出来作为标注的文本内容。比如一个图层中有个属性字段叫`name_en`，那么一个简单的文本标注就可以用下面的方式进行样式定义：

![](https://cloud.githubusercontent.com/assets/126952/3881477/0145a420-218e-11e4-8961-23c6d57df53b.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-labels/)_

	
	#place_label {
	  text-name: [name_en];
	  text-face-name: 'Open Sans Condensed Bold';
	}
	

上面例子中标注的颜色和字号都是默认值：黑色，`10`像素。这两个值可以用`text-fill`和`text-size`属性进行调整。

![](https://cloud.githubusercontent.com/assets/126952/3881475/013ef2b0-218e-11e4-8f46-b578843e2092.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-labels/)_

	
	#place_label {
	  text-name: [name_en];
	  text-face-name: 'Open Sans Condensed Bold';
	  text-fill: #036;
	  text-size: 20;
	}
	

为了能让文本标注在地图背景中更为醒目，通常会为标注上的文字增加一圈边线或光晕。光晕的颜色和宽度分别用`text-halo-fill`和`text-halo-radius`属性进行配置。在下面的例子中，我们利用了`fadeout`颜色变换函数得到一个具有`30%`透明度的白色光晕。

![](https://cloud.githubusercontent.com/assets/126952/3881476/014304f4-218e-11e4-9690-792b142c66fd.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-labels/)_

	
	#place_label {
	  text-name: [name_en];
	  text-face-name: 'Open Sans Condensed Bold';
	  text-fill: #036;
	  text-size: 20;
	  text-halo-fill: fadeout(white, 30%);
	  text-halo-radius: 2.5;
	}
	

#### 沿线标注

CartoCSS支持沿线标注，例如沿着道路或河流的走向来绘制文本标注。要实现沿线标注需要将`text-placement`属性值设置为`line`（它的默认值是`point`）。在下面的例子中，除了标注样式以外，还有一些简单的河流线样式。

![](https://cloud.githubusercontent.com/assets/126952/3881773/9f47f6c6-2190-11e4-9d53-f49a687147cb.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-labels/)_

	
	#waterway_label {
	  text-name: [name_en];
	  text-face-name: 'Open Sans Condensed Bold';
	  text-fill: #036;
	  text-size: 20;
	  text-placement: line;
	}
	

对于河流来说，让它的标注相对于河流流向稍作平移会比较美观。这种效果可通过设置`text-dy`属性来实现。它的值以像素为单位，用于指定将标注沿**y**轴方向的偏移量。

还有一个`text-max-char-angle-delta`属性，它用来控制标注文本的最大转弯角度，其默认值是`22.5°`。如果把它的值调小，那么标注就会被绘制在线要素上更为平直的部分，避开尖锐拐角。

![](https://cloud.githubusercontent.com/assets/126952/3881774/9f4851e8-2190-11e4-8c86-cbdba0276f13.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-labels/)_

	
	#waterway_label {
	  text-name: [name_en];
	  text-face-name: 'Open Sans Condensed Bold';
	  text-fill: #036;
	  text-size: 20;
	  text-placement: line;
	  text-dy: 12;
	  text-max-char-angle-delta: 15;
	}
	

#### 添加自定义文字

Labels aren’t limited to pulling text from just one field. You can combine data from many fields as well as arbitrary text to construct your `text-name`. For example you could include a point’s type in parentheses.

用于标注的文字（也就是`text-name`属性的值），不仅可以从空间数据的某一个属性字段中获取，还可以由多个属性字段组合而成，甚至还可以是用户自定义的任意文本。例如，可以在标注文字的后面添加一个写在括号中的兴趣点类型：

![](https://cloud.githubusercontent.com/assets/126952/3882373/597ad9f0-2196-11e4-9cbf-1977422cf312.png)

	
	#poi_label {
	  text-name: [name_en] + ' (' + [type] + ')';
	  text-face-name: 'Open Sans Condensed Bold';
	  text-size: 16;
	}
	

其它一些应用实例：

- 多语言标注：`[name] + '(' + [name_en] + ')'`
- 多级行政区划：`[city] + ', ' + [province]`
- 计量单位：`[elevation] + 'm'`
- 特殊的[unicode字符](http://copypastecharacter.com/symbols)：`'⚑ ' + [embassy_name] or '⚓ ' + [harbour_name]`

标注的内容还可以不从属性数据的字段中提取，而是使用任意用户自定义的文本。但由于向后兼容的原因，这样的自定义文本必须要套上两层引号：

	
	#poi_label[maki='park'] {
	  text-name: "'Park'";
	  text-face-name: 'Open Sans Regular';
	}
	

如果标注文本中包含引号，那么需要使用反斜杠来转义。例如，如果要标注文字**City’s “Best” Coffee**，那么要这样写：

	
	text-name: "'City\'s \"Best\" Coffee'";
	

#### 多行标注

You can wrap long labels onto multiple lines with the `text-wrap-width` property which specifies at what pixel width labels should start wrapping. By default the first word that crosses the wrap-width threshold will not wrap - to change this you can set `text-wrap-before` to `true`.

对于包含较长文本的标注，可以通过设置`text-wrap-width`属性来将其折行显示。这个属性用于指定标注文本从什么位置（以像素为单位）开始折行。对于正好位于设定的折行宽度处的第一个字，默认是不会被折到下一行显示。如果要修改这个默认行为，可以将`text-wrap-before`属性设置为`true`。

![](https://cloud.githubusercontent.com/assets/126952/3882901/a1ccfc06-219b-11e4-8545-4fd89239e144.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/styling-labels/)_

	
	#poi_label {
	  text-name: [name];
	  text-face-name: 'Open Sans Condensed Bold';
	  text-size: 16;
	  text-wrap-width: 150;
	  text-wrap-before: true;
	}
	

不过，需要注意多行本文暂不支持沿线标注的情况。

用户还可以自行指定一个特定的折行点，比如`\n`。

	
	#poi_label {
	  text-name: [name] + '\n' + [type];
	  text-face-name: 'Open Sans Condensed Bold';
	  text-size: 16;
	}
	

（译注：以下部分为原TileMill文档中的内容，但在新的Mapbox Studio文档中已被删除，但本书编译者认为这部分值得保留。）

用户还可以利用`text-wrap-character`属性来指定一个折行字符（默认是空格）。这个特性在对一些具有特定结构的数据集进行文本标注时特别有用。例如在之前的多字段组合标注例子中，我们可以用下划线`_`作为多个字段的连接符，然后将折行字符就设成下划线`_`，之后再把折行宽度`text-wrap-width`设得很小，这样就能够保证不同的字段总是能分别显示在独立的行中。

![](https://www.mapbox.com/tilemill/assets/pages/styling-labels-7.png)

_图片来源：[Mapbox](https://www.mapbox.com/tilemill/docs/guides/styling-labels/)_

	
	#cities {
	  text-name: [NAME] + '_' + [ADM1NAME];
	  text-face-name: 'Droid Sans Regular';
	  text-size: 20;
	  text-wrap-width: 1;
	  text-wrap-character: '_';
	}
	

#### 将标注层独立出来

If you are applying label styles to layers that also have line or polygon styles you might notice some unexpected overlapping where the labels aren’t necessarily on top.

如果在一个已经配置了线样式或面样式的图层上也同时配置标注样式，那么往往会出现一些标注和几何要素相互压盖等异常的渲染效果，无法保证标注能出现在最上层。

对于这个问题，在地图样式比较简单的时候，可以通过将地理要素的样式与标注样式分别定义在不同的从属样式中来解决：

	
	#layer {
	  ::shape {
	    polygon-fill: #ace;
	    line-color: #68a;
	  }
	  ::label {
	    text-name: [name];
	    text-face-name: 'Arial Regular';
	  }
	}
	

而对于样式比较复杂的大多数情况，则最好是为标注专门创建一个独立于地理要素的图层，然后分别定义它们的样式。（可参考TileMill中的示例制图项目Open Streets DC）

这时，专门用于标注的图层与其对应的地理要素图层实际指向的是同一个地理数据集，但是要把标注图层置于地理要素图层之上，以保证标注不会被地理要素压盖。关于图层和地图对象的绘制顺序问题，请参考下一节“符号的渲染顺序”。

#### 参考文献

1. Mapbox, [Mapbox Studio Styling Labels](https://www.mapbox.com/mapbox-studio/styling-labels/)