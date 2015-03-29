### 4.1 高级地图设计

在本节中，我们会谈到一些运用CartoCSS制图的高级技巧，它们可以让你的地图看起来更加高端大气。本节示例使用的是美国2010年的龙卷风统计数据。关于制图所需数据的准备，有很多途径，比如[利用谷歌文档](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)来做。

#### 不同的级别，不同的样式

交互式地图通过缩放功能提供给用户改变对地图观察尺度的能力。不同的缩放级别对应着地图的不同比例尺或分辨率。在缩放级数较小时，我们观察者距离观测区域较远，视口中的地理范围较大，对应的地图比例尺较小，分辨率也较低；而当缩放级数较大时则是相反的效果。这种能力是目前数字化地图，特别是基于Web的在线地图服务能够提供的最基本的一种交互能力。缩放级别通常是一组离散的数值，这意味着地图的缩放被限制在一组（目前通常最多为20个左右）的固定级别上，而不是连续的“无级变化”。

在地图设计的时候，可以对同一幅地图在其不同的缩放级别上考虑配置不同的样式。我们这里假定制图所需要的[龙卷风点数据已经准备好了](https://www.mapbox.com/tilemill/docs/tutorials/point-data/)，并且配置了一些初始样式。那么现在就来考虑一下怎样才能让这张地图在不同的级别具有不同的样式。下图是缩放到4级时的龙卷风地图。其中的龙卷风注记符号根据风力（f-scale属性）大小设置了符号的尺寸。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-1.png)

如果把这张地图放大到7级，那么就可以看到那些点符号并不会随着地图放大而变大，因而在这个级别上看起来就显得太小了（如下图）。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-2.png)

那么我们此时请出CartoCSS。只需要简单的几行代码，就可以轻松实现让地图在不同的级别下展示出不同的样式。

下图中高亮部分的CartoCSS代码的含义是说：“当缩放级别到达7级时，应用下面这段样式。”你可以根据自己的需要添加任意数量这样的样式块。这样一来，就可以让地图上的点、图标和标注随着地图放大而适当变大，从而美观的展示出更多细节。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-3.png)

The following symbols are allowed in conditional statements: `=`, `!=`, `>`, `>=`, `<`, `<=` You can also group by zoom ranges by setting a beginning and an end, like this:

在缩放级别过滤器的条件表达式中可以使用比较运算符：`=`，`!=`，`>`，`>=`，`<`，`<=`，还可以定义一个缩放级别范围，就像这样：

	
	[zoom >= 4][zoom <=8] {
	  …styling…
	}
	

利用缩放级别过滤器还可以控制某些图层在特定级别上的可见性，从而使这些图层随着地图的放大或缩小被“打开”或“关闭”。这个方法对于包含多个地理等级数据（比如多个行政区划等级）的地图的样式配置很有用。

还是以龙卷风地图为例。在国家级尺度上，视口部分的地图比例尺很小，因此会出现一些聚在一起的点，它们形成一些杂乱的点团而让人难以辨认那里究竟都有些什么。在这个等级上，可以将每个州按照各自龙卷风的总数统计出来然后只用一个点来表达，并且该点的尺寸与龙卷风总数正相关。而随着用户不断放大地图，从适当的级别开始就不再显示这个国家级尺度的图层，而是可以将原始的独立龙卷风点分别绘制出来。

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

In order to deliver information on a map more immediately, sometimes it is useful to label your data with the actual number or feature that is being represented. This can be combined with the dot or just be a label on its own.

为了能够在地图上更快捷的展示信息，为所表达的内容添加数字或文字标注是一种有效的方式。标注可以标在对应的点旁边，也可以不把点绘制出来而只绘制标注。

For our tornado map, we have decided to display the total number of tornadoes inside the state-level dots. To do this we need to add just a few lines to the layer’s CartoCSS:

在这张龙卷风地图上，我们是将发生龙卷风的总次数标记在每个州一级的圆点里面。为了实现这种效果，只需要稍微增加几行CartoCSS代码即可：

![](https://www.mapbox.com/tilemill/assets/pages/label-styling-1.png)

1. `::label`   
	This creates a new **symbolizer** for your layer. The name ‘label’ here is arbitrary, you can call it whatever you like. The position of the symbolizer in CartoCSS determines the order of its rendering. The first code in a CartoCSS layer is rendered first on the map and will be **below** anything that is rendered after it. Therefore, if you need a layer feature to be on **top**, like we do with the labels, it must come last in the code.  
	它的含义是创建一个从属样式块（译注：原文中是“新符号”，在TileMill的系列文档中，均使用“新符号”这个说法，而在新的Mapbox Studio文档中，都变成attachment，也就是“从属样式块”了，为了保持全文上下一致，我都翻成“从属样式块”）。这里的从属样式块名`label`可以随便起，而从属样式块在整个样式表中的位置则决定了它被渲染的次序。在CartoCSS的样式块中，最先出现的代码会被首先渲染到地图上，因而会被绘制在后面其它渲染的要素的下面。因此，如果需要将某一层渲染到地图的最上层，那么就应该把它定义在样式代码的最后。
2. `text-name`  
	This denotes the field whose text will be displayed.  
	这是指明了哪个数据字段将被作为文本标记中显示的内容。
3. `text-face-name`  
	This sets the **font** for the text label. You can view a list of available system fonts by clicking the** font button (A)** on the lower left.  
	这是用于设置文本标注上文字的字体。
4. `text-allow-overlap`  
	This allows the text and the dots to be displayed together at the same location. By default this option is set to false, which prevents overlapping items.  
	这个设置是允许文本标注和圆点标记重叠显示。这个属性的默认值是`false`，也就是不允许重叠。

That is all you need to get started with **labels**. The same idea applies to placename labels as well. You can further style them with the text- style parameters, changing things like size, color, opacity, placement, and more.

以上这些用来配置一个基本的文本标注已经足够了。把它们用于地名标注也没有任何问题。用户可以通过`text-`系列属性进一步调整诸如字号、颜色、透明度、位置偏移等样式。

#### 使用图像作为图标（Images as Icons）

TileMill supports using **SVG (Scalable Vector Graphic)** images as markers on your map. It is possible that we could use a custom-made tornado icon in place of the circle markers. The first thing you need is the SVG file saved somewhere on your system, preferably in your project folder for the sake of organization (Documents/MapBox/project/project-name/). Then it’s all in the CartoCSS.

在CartoCSS中可以使用SVG图像作为地图上的注记。在这个台风地图的例子中，我们可以使用自定义的图标来代替圆点形状注记。当然首先，我们需要先要有一个保存在系统中的SVG文件。为了组织方便，最好把它放在项目的目录中。

![](https://www.mapbox.com/tilemill/assets/pages/svg-icons-1.png)

1. `point-file`  
	This designates the **path** to the SVG relative to your project folder. In this case the SVG is located in Documents/MapBox/project/2010-tornadoes/icons/.  
	这是指向SVG文件的路径
2. `point-allow-overlap`  
	Like other `-allow-overlap` parameters, this allows the images to be displayed even if they will be on top of each other.  
	就像其它`-allow-overlap`参数一样，这个属性允许表示点符号的图标重叠显示。
3. `point-transform`  
	This is the parameter used to **scale** and **move** the image, among other things. A value of `"scale(1)"` will display the image at its original size, while `"scale(0.5)"` and `"scale(2)"` will display it at 50% and 200% respectively. You can also use this property to move the image vertically and horizontally by using the property `"translate()"`. For example, the value `"translate(20, -40)"` will move the image 20 pixels to the right and 40 pixels up. There are several other properties that you can employ with point-transform. [Learn about them on W3](http://www.w3.org/TR/SVG/coords.html#TransformAttribute).  
	这个参数用来对注记图片进行拉伸与移位。使用`"scale(1)"`将按照原始比例绘制注记图片，而`"scale(0.5)"`和`"scale(2)"`则将分别按照原始图片50%与200%的比例来绘制。除了拉伸，还可以用`"translate()"`来移位，例如，可以用`"translate(20, -40)"`将图片向右平移20像素、向上平移40像素。除此以外，还有一些可用于点变换的属性，可以从[W3上参考更多详细内容](http://www.w3.org/TR/SVG/coords.html#TransformAttribute)。

#### 导出并合成（Exporting for Compositing）

When your map contains multiple levels of data as our tornado map does, it is sometimes wise to export each level separately. This has multiple benefits. Firstly, it compartmentalizes your map so that when updating you may not have to re-export the entire map. Secondly, it gives you greater flexibility when [compositing](https://www.mapbox.com/mapbox-studio/compositing-reference).

当一幅地图与这个龙卷风地图的例子一样包含了多个层次的数据时，把每个层次的数据都分别导出并作为独立的图层是值得推荐的做法。这样做有很多好处，首先，这样可以将地图划分成更多细粒度的部分，从而在地图更新的时候不必把整张图再重新导出一遍（译注：这里“导出”的含义有待斟酌，不知道是否理解正确。我认为是在预处理阶段将原始数据用过滤器拆分成若干个数据子集，而它这里的“导出”似乎未必是这个意思）。第二，这样可以为合成操作提供更多方便。

Thirdly, **interactivity** can only be active on one layer at a time. This means if we want a hover tooltip for the state-level dots and for the individual dots, we cannot export them together.

第三，地图的交互一次只能应用在一个图层上。这就是说，（在龙卷风地图的例子中）如果想要在州一级和原始的独立点数据集上都实现悬停工具条的效果，那么就不能把这些数据集一起导出。（译注：这点说的不清楚，需要梳理）

When exporting individual pieces of your project, a very helpful tool is the ability to **comment-out** specific CartoCSS code. Anything commented-out will remain in your code, but not render on the map. All this entails is placing `/*` before and `*/` after the code you want to comment-out. This is also a way to write comments into the code, hence the name.

当需要将项目中的某些独立部分导出的时候，有个技巧是对特定部分的CartoCSS代码灵活运用注释。被注释掉的部分对应的样式不会被渲染到地图上。CartoCSS中的注释也是使用`/*`和`*/`将被注释的部分代码包围。（译注：最后一句话没有译出，因为感觉含义既有重复，又有不明确的地方）

We have plans to composite this tornado map with an existing world baselayer available from Mapbox, so the first thing to do will be to comment-out the default blue and white world base and the state borders.

在龙卷风地图的例子中，我们准备把龙卷风的分布图与一张现成的世界地图合成。为此，首先应该把蓝白色的世界底图和国界线样式代码注释掉。

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-1.png)

Notice that the code between the `/*` and `*/` is now greyed-out, and the background of the map is **transparent**.

注意图中`/*`和`*/`之间的代码已经变成灰色的，而此时的地图背景已经变成全透明的了。

Now we can create a tooltip for the state-level dots, and export. On the export page, be sure to select only the zoom levels for this particular piece of the map.

现在我们可以为州一级的圆点符号创建悬浮提示效果并导出了。在导出配置页面上，要确定只选择导出那些针对这幅地图特定部分的缩放级别。（译注：关于导出，还是不太清楚具体的含义，所以翻译的不理想）

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-2.png)

Next we create and switch the tooltip to the individual dots layer, and export. Change the zoom levels and filename accordingly.

接下来，我们为每个原始的龙卷风统计点创建悬浮提示效果并导出。在导出时，将缩放级别和文件名做相应的修改。

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-3.png)

这样，我们就得到了两个都具有交互能力的MBTiles数据集，而且还可以将它们合成（参见3.7节），并叠加到一幅在线底图上。最终的地图效果可从[这里](https://a.tiles.mapbox.com/v3/mapbox.map-4qkj96dp.html)看到。用于这幅地图的完整CartoCSS代码如下：

	
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