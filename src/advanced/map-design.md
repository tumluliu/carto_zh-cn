### 4.1 高级地图设计

在本节中，我们会谈到一些运用CartoCSS制图的高级技巧，它们可以让你的地图看起来更加高端大气。本节示例使用的是美国2010年的龙卷风统计数据。关于制图所需数据的准备，有很多途径，比如[利用谷歌文档](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)来做。

#### 不同的级别，不同的样式

交互式地图通过缩放功能提供给用户改变对地图观察尺度的能力。不同的缩放级别对应着地图的不同比例尺或分辨率。在缩放级数较小时，我们观察者距离观测区域较远，视口中的地理范围较大，对应的地图比例尺较小，分辨率也较低；而当缩放级数较大时则是相反的效果。这种能力是目前数字化地图，特别是基于Web的在线地图服务能够提供的最基本的一种交互能力。缩放级别通常是一组离散的数值，这意味着地图的缩放被限制在一组（目前通常最多为20个左右）的固定级别上，而不是连续的“无级变化”。

在地图设计的时候，可以对同一幅地图在其不同的缩放级别上考虑配置不同的样式。我们这里假定制图所需要的[龙卷风点数据已经准备好了](https://www.mapbox.com/tilemill/docs/tutorials/point-data/)，并且配置了一些初始样式。那么现在就来考虑一下怎样才能让这张地图在不同的级别具有不同的样式。下图是缩放到4级时的龙卷风分布图。其中的龙卷风注记符号根据风力（f-scale属性）大小设置了符号的尺寸。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-1.png)

如果把这张地图放大到7级，那么就可以看到那些点符号并不会随着地图放大而变大，因而在这个级别上看起来就显得太小了（如下图）。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-2.png)

那么我们此时请出CartoCSS。只需寥寥几行代码，就可以轻松实现让地图在不同的级别下展示出不同的样式。

下图中高亮部分的CartoCSS代码的含义是说：“当缩放级别到达7级时，应用下面这段样式。”你可以根据自己的需要添加任意数量这样的样式块。这样一来，就可以让地图上的点、图标和标注随着地图放大而适当变大，从而美观的展示出更多细节。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-3.png)

The following symbols are allowed in conditional statements: `=`, `!=`, `>`, `>=`, `<`, `<=` You can also group by zoom ranges by setting a beginning and an end, like this:

在缩放级别过滤器的条件表达式中可以使用比较运算符：`=`，`!=`，`>`，`>=`，`<`，`<=`，还可以定义一个缩放级别范围，就像这样：

	
	[zoom >= 4][zoom <=8] {
	  …styling…
	}
	

利用缩放级别过滤器还可以控制某些图层在特定级别上的可见性，从而使这些图层随着地图的放大或缩小被“打开”或“关闭”。这个方法对于包含多个地理等级数据（比如多个行政区划等级）的地图的样式配置很有用。

我们还以龙卷风分布图为例。在国家级尺度上，视口部分的地图比例尺很小，因此会出现一些聚在一起的点，它们形成一些杂乱的点团而让人难以辨认那里究竟都有些什么。在这个等级上，可以将每个州按照各自龙卷风的总数统计出来然后只用一个点来表达，并且该点的尺寸与龙卷风总数正相关。而随着用户不断放大地图，从适当的级别开始就不再显示这个国家级尺度的图层，而是可以将原始的独立龙卷风点分别绘制出来。

具体的实现方法是，利用[数据透视表](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)从原始数据集中聚合出州一级的龙卷风统计数据，然后对这个新的数据集[地理编码](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)，赋予其地理位置信息。接下来要做的就是把这个新的点数据集作为图层加入地图，然后就可以继续用缩放级别过滤器为其配置CartoCSS样式了。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-4.png)

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-5.png)

用户还可以使用与缩放级别过滤器相同的语法来从数据集中基于某些字段设置其它的过滤条件，从而筛选出一些满足特定条件的要素记录。下面的样式语句只显示`#tornadoes`图层中俄克拉何马州的数据。其中的`"state"`是该图层属性数据中的一个字段，它包含了各州的缩写。

	
	#tornadoes [state = "OK"] {
	  …styling…
	}
	

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-6.png)

当然，还可以反向选择，例如显示除俄克拉何马以外其它各州的数据：

	
	#tornadoes [state != "OK"] {
	  …styling…
	}
	

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-7.png)

#### 文本标注

为了能够在地图上更直观的展示信息，我们可以为想要在地图上表达的内容加上数字或文字形式的标注。标注的样式非常丰富（详见4.3节），对于本例中的点标注来说，既可以将标注文本填在其对应的圆点当中，也可以不画圆点而只绘制标注。在这张龙卷风分布图上，我们是将发生龙卷风的总次数标记在每个州一级的圆点里面。为了实现这种效果，需要再加几行CartoCSS代码：

![](https://www.mapbox.com/tilemill/assets/pages/label-styling-1.png)

1. `::label`   
	从前面第三章的基本概念介绍中我们已经了解了这个用双冒号`::`开头的标记定义了一个从属样式块（attachment），这里的从属样式名`label`是任意的，并不是关键字。而从属样式在整个样式表中的定义位置则决定了它被绘制的次序。在CartoCSS的样式块中，遵循“先定义，先绘制”的原则。而先绘制的要素会被后绘制的要素压在下面。因此，如果想把某一层绘制到地图的最上层，那么就应该把它的样式定义代码写在最后。
2. `text-name`  
	指明作为文本标注内容的属性数据字段。
3. `text-face-name`  
	设置标注中文字的字体。
4. `text-allow-overlap`  
	这个属性的作用是允许文本标注和圆点标记重叠显示。注意它的默认值是`false`，也就是不允许重叠，我们这里把它改成了`true`。

用上面这些样式属性来配置基本的文本标注已经足够了。用同样的方法还可以配置地名标注。当然，如果需要，那么你还可以通过`text-`系列属性进一步调整标注的其它样式，例如字号、颜色、透明度、位置偏移等。

#### 使用自定义图标

CartoCSS支持将外部的SVG格式图片作为地图上的注记图标。在本节的例子中，我们可以使用自定义的SVG图片来代替圆点形状注记。当然首先，我们需要先要有一个保存在系统中能够被制图项目引用到的SVG文件。为了方便管理，最好把它就放在项目的目录中。

![](https://www.mapbox.com/tilemill/assets/pages/svg-icons-1.png)

1. `point-file`  
	明确SVG文件的路径。
2. `point-allow-overlap`  
	跟其它`-allow-overlap`参数一样，这个属性的作用也是允许点符号图标重叠显示。
3. `point-transform`  
	这个参数用来对注记图标进行拉伸与移位。使用`"scale(1)"`将按照原始比例绘制图标，而`"scale(0.5)"`和`"scale(2)"`则将分别按照原始图片尺寸的`50%`与`200%`来绘制。除了拉伸，还可以用`"translate()"`来移位，例如，可以用`"translate(20, -40)"`将图片向右平移`20`像素、向上平移`40`像素。除此以外，`point-transform`的其它属性可参考[W3上的相关文档](http://www.w3.org/TR/SVG/coords.html#TransformAttribute)。

#### 导出与合成

在完成制图样式的配置后，最后可以将地图以瓦片数据集（MBTiles）的形式导出。导出的瓦片数据集中包含了按照所配置的样式渲染后的地图以及图例、浮动工具等内容。导出的瓦片数据集是一种可移植的外存文件格式，可以被直接用于很多瓦片地图服务。需要强调的是，导出到瓦片数据集这个功能本身并不是CartoCSS的一部分，但在实际的制图工作流程中是重要的一环。试想一下，当你用CartoCSS设计好了一幅地图，然后怎么才能让其他人看呢？总不能每次都打开制图工具吧。所以，最自然的做法就是把制好的图保存成一种可移植、可发布的格式，然后拷贝或上传到一个支持瓦片地图服务的服务器上，这样才能让别人能够在线观赏你制作的精美地图。因此，导出这个功能通常是支持CartoCSS的制图工具提供的一项基本功能。

如果你的地图包含多个等级的数据，就像本例中的龙卷风分布图一样，那么我们建议把不同等级的数据分别单独导出。这样做的好处有三点：首先，它将地图划分成多个可以独立更新的部分，因而在其中某个部分发生变化（比如更新了样式）的时候无需将整张图全部重新导出；第二，按照不同等级拆分开的独立数据集可以更加灵活的进行合成操作（参见3.7节）；第三，地图的交互效果（参见4.4节）一次只能应用在一个图层上，还以龙卷风分布图为例，比如说我想在州和原始点两个级别上都提供浮动工具条的交互效果，那么就不能把这两个级别的瓦片数据集打包导出，而必须要分开才行。

当需要将制图项目中的某一部分单独导出的时候，有个技巧是对其它部分的CartoCSS代码灵活运用注释。被注释掉的样式不会被渲染。CartoCSS中的注释也是使用`/*`和`*/`将被注释的部分代码包围。

前面既然谈到了“灵活的合成操作”，那么我们现在就来把本节的示例中的龙卷风分布图与一张全球底图合成。为此，我们先把原来项目中的蓝白色全球底图与国界线样式代码注释掉。注意图中`/*`和`*/`之间的代码已经变成灰色的，而此时的地图背景变成了全透明。

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-1.png)

现在我们为州一级的圆点符号创建浮动工具条（参见4.4节），然后将其导出。在配置导出参数时，要根据实际需要和地图本身的特点（比如地理覆盖范围、最高分辨率等），想好要导出地图中的哪些级别。如果选择的级别范围不合理，那么可能会造成导出的数据量过大（对那些本来没有那么大地理覆盖范围的图选择了较多的小比例尺级别）等问题。

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-2.png)

接下来，我们为原始的龙卷风统计点创建浮动工具条，然后将其导出。在配置导出参数时，将缩放级别范围和导出的数据文件名做相应修改。

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-3.png)

这样，我们就得到了两个都具有交互能力（浮动工具条）的瓦片数据集，而且还可以将它们合成（参见3.7节），并叠加到一幅在线全球底图上。最终的地图效果可从[这里](https://a.tiles.mapbox.com/v3/mapbox.map-4qkj96dp.html)看到。用于这幅地图的完整CartoCSS代码如下：

	
	Map {
	  background-color: #b8dee6;
	}
	
	#countries {
	  ::outline {
	    line-color: #85c5d3;
	    line-width: 2;
	    line-join: round;
	  }
	  [GEOUNIT != &quot;United States of America&quot;]{polygon-fill: #fff;}
	}
	
	/*Individual tornado points*/
	#tornadoes [zoom &gt; 5]{
	  marker-width:6;
	  marker-fill:#f45;
	  marker-line-color:#813;
	  marker-allow-overlap:true;
	  marker-line-width:0.5;
	  [zoom = 6]{
	    [fscale=0]{marker-width:2.5;}
	    [fscale=1]{marker-width:4;}
	    [fscale=2]{marker-width:5.5;}
	    [fscale=3]{marker-width:7;}
	    [fscale=4]{marker-width:9;}
	    [fscale=5]{marker-width:12;}
	  }
	  [zoom = 7]{
	    [fscale=0]{marker-width:4;}
	    [fscale=1]{marker-width:6;}
	    [fscale=2]{marker-width:8;}
	    [fscale=3]{marker-width:11;}
	    [fscale=4]{marker-width:14;}
	    [fscale=5]{marker-width:18;}
	  }
	  [zoom = 8]{
	    [fscale=0]{marker-width:6;}
	    [fscale=1]{marker-width:9;}
	    [fscale=2]{marker-width:12;}
	    [fscale=3]{marker-width:16;}
	    [fscale=4]{marker-width:22;}
	    [fscale=5]{marker-width:30;}
	  }
	}
	
	/*State-level dots and labels*/
	#tornadoes-state-level [zoom &lt;= 5] {
	  marker-width:6;
	  marker-fill:#f45;
	  marker-line-color:#813;
	  marker-line-opacity:0;
	  marker-allow-overlap:true;
	  [zoom = 3]{
	    [tornadoes &lt; 10]{marker-width:6;}
	    [tornadoes &gt;= 10][tornadoes &lt; 25]{marker-width:10;}
	    [tornadoes &gt;= 25][tornadoes &lt; 50]{marker-width:16;}
	    [tornadoes &gt;= 50][tornadoes &lt; 100]{marker-width:24;}
	    [tornadoes &gt;= 100]{marker-width:16;}
	  }
	  [zoom = 4]{
	    [tornadoes &lt; 10]{marker-width:7;}
	    [tornadoes &gt;= 10][tornadoes &lt; 25]{marker-width:12;}
	    [tornadoes &gt;= 25][tornadoes &lt; 50]{marker-width:20;}
	    [tornadoes &gt;= 50][tornadoes &lt; 100]{marker-width:32;}
	    [tornadoes &gt;= 100]{marker-width:44;}
	  }
	  [zoom = 5]{
	    [tornadoes &lt; 10]{marker-width:10;}
	    [tornadoes &gt;= 10][tornadoes &lt; 25]{marker-width:18;}
	    [tornadoes &gt;= 25][tornadoes &lt; 50]{marker-width:28;}
	    [tornadoes &gt;= 50][tornadoes &lt; 100]{marker-width:44;}
	    [tornadoes &gt;= 100]{marker-width:68;}
	  }
	  ::labels {
	    text-name:&quot;[tornadoes]&quot;;
	    text-face-name:&quot;Arial Bold&quot;;
	    text-allow-overlap:true;
	    [zoom = 3]{
	      [tornadoes &lt; 25]{text-opacity:0;}
	    }
	    [zoom = 4]{
	      [tornadoes &lt; 10]{text-opacity:0;}
	      [tornadoes &gt;= 10][tornadoes &lt; 25]{text-size:8;}
	      [tornadoes &gt;= 25][tornadoes &lt; 50]{text-size:10;}
	      [tornadoes &gt;= 50][tornadoes &lt; 100]{text-size:11.5;}
	      [tornadoes &gt;= 100]{text-size:13;}
	    }
	    [zoom = 5]{
	      [tornadoes &lt; 10]{text-size:8;}
	      [tornadoes &gt;= 10][tornadoes &lt; 25]{text-size:10;}
	      [tornadoes &gt;= 25][tornadoes &lt; 50]{text-size:11.5;}
	      [tornadoes &gt;= 50][tornadoes &lt; 100]{text-size:13;}
	      [tornadoes &gt;= 100]{text-size:16;}
	    }
	  }
	}
	
	/* State borders */
	#states {
	  line-color:#ccc;
	  line-width:0.5;
	  polygon-opacity:1;
	  polygon-fill:#fff;
	}
	

#### 参考文献

1. Mapbox, [Advanced Map Design](https://www.mapbox.com/tilemill/docs/guides/advanced-map-design/)