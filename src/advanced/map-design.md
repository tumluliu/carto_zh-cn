### 高级地图设计

_译注：[原文地址](https://www.mapbox.com/tilemill/docs/guides/advanced-map-design/)_

This guide will cover some of the more advanced techniques you can employ in TileMill to take your map to the next level. For demonstration, we will continue to work with the 2010 tornado data from the [preparing data with Google Docs guide](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/).

这节中的内容包含了能够让你的地图质量提升一个档次的一些技巧。本节中使用的示例数据是2010年的龙卷风统计数据，关于数据准备可参考[“利用谷歌文档准备数据”](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)。

#### 根据缩放级别调整样式（Styling for Zoom Levels）

Interactive maps give users the ability to change the scale by zooming in or out and we must design them accordingly. Assuming you have [imported your point data](https://www.mapbox.com/tilemill/docs/tutorials/point-data/) and done some initial styling, it is time to start thinking about how your map will look at different **zoom levels**.

交互式地图通过缩放功能提供给用户改变对地图观察尺度的能力。在地图设计的时候，需要针对不同缩放级别考虑不同的样式配置。我们这里假定[有一组点数据已经准备好了](https://www.mapbox.com/tilemill/docs/tutorials/point-data/)，并且配置了一些初始样式。那么现在就考虑一下怎么才能让这张地图在不同的缩放级别具有不同的样式。

Here is the tornado map at **zoom level 4** after sizing the markers based on their intensity (f-scale).

下图是在地图缩放到4级时的龙卷风地图。其中的注记符号根据龙卷风的强度（f-scale）设置了各自的大小。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-1.png)

And here is the same map at **zoom level 7**. Notice that the dots did not scale with the rest of the maps and could be considered too small.

如果把上面的图放大到7级，那么就可以看到那些点不会随着地图放大而放大，因而在这个级别上看起来就太小了（如下图）。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-2.png)

With some simple CartoCSS, you can solve this by **conditioning your styles based on the zoom level**.

只需要用一些简单的CartoCSS，就可以轻松实现让地图在不同缩放级别下展示出不同的样式。

The highlighted CartoCSS below is saying to TileMill, “when the zoom level is 7, apply the following style.” You can do this for as many levels as you wish, and include any kind of styling. This is useful for scaling back the number of dots, icons, and labels as you zoom out, and creating a greater level of detail as you zoom in.

下图中高亮部分的CartoCSS代码的意思就是说：“当缩放级别到达7级时，应用下面这段样式。”用户可以根据自己的需要添加任意数量这样的子样式块。有了这种能力，就可以实现随着地图不断放大也让点、图标和标注跟着一起适当放大，从而展示出更多细节的效果。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-3.png)

The following symbols are allowed in conditional statements: `=`, `!=`, `>`, `>=`, `<`, `<=` You can also group by zoom ranges by setting a beginning and an end, like this:

在条件表达式中可以使用这些比较运算符：`=`，`!=`，`>`，`>=`，`<`，`<=`，还可以定义一个缩放级别范围，就像这样：

	
	[zoom >= 4][zoom <=8] {
	  …styling…
	}
	

**Zoom level conditioning** can also be used to limit the visibility of specific layers to certain zoom ranges, making it possible to “turn them on” or “turn them off” as you zoom in or out. This method is particularly useful for joining multiple geographic levels of data.

缩放级别条件过滤还可以用来控制某些图层在特定的缩放级别上的可见性，从而使这些图层随着地图的放大或缩小被“打开”或“关闭”。这个方法对于包含多个地理级别数据的地图特别有用。

Take the tornado map for example. At the national level it is quite cluttered and hard to decipher individual points where there are clusters. At this zoom level, one option might be to display a scaled dot for each state that represents total number of tornadoes that occurred in that state. Then, as the user zooms in, the state-level layer goes away and the individual tornado points appear.

还是以龙卷风地图为例，在国家级尺度上，那些聚在一起的点会形成一些杂乱的点团而让人难以辨认那里究竟都有些什么。在这个级别，可以为每个州按照各自龙卷风的总数只绘制一个点，而且点的大小可以与龙卷风总数成一定比例。然后随着用户不断放大地图，就不再显示这个国家级尺度的图层，而是可以将每个独立的龙卷风点绘制出来。

With this particular data we were able to aggregate to the state level using a [pivot table](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/) and then [geocoded](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/) the state points. Add this newly created data to the project as a new layer. Then simply add zoom level conditions after the layer names in your CartoCSS code, and continue to style normally.

基于这个特定的数据集可以利用[数据透视表](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)聚合出州一级的点数据集，并且对这些点进行[地理编码](https://www.mapbox.com/tilemill/docs/tutorials/google-docs/)。下面要做的就是把这个新生成的数据集作为图层加入，然后就可以继续用缩放级别过滤器为其配置CartoCSS样式了。

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-4.png)

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-5.png)

You can also use this same syntax to single-out features on the map where a field in your data **meets a certain criteria**. For example, the code below will only show points on the #tornado layer for Oklahoma. “state” is the name of a field in the tornado data that contains state abbreviations.

用户还可以使用与缩放级别过滤器相同的语法来从数据集中基于某些字段设置条件过滤器，从而挑选出一些特定的要素记录。下面的样式语句只显示`#tornadoes`图层中的俄克拉何马州的点数据。其中`"state"`是该图层中的一个包含了各州缩写字母的字段。

	
	#tornadoes [state = "OK"] {
	  …styling…
	}
	

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-6.png)

The reverse is also possible. Show states that are not Oklahoma:

当然，还可以反向选择，例如显示除俄克拉何马以外其它所有州的数据：

	
	#tornadoes [state != "OK"] {
	  …styling…
	}
	

![](https://www.mapbox.com/tilemill/assets/pages/zoom-styling-7.png)

#### 文本标注（Text Labels）

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



![](https://www.mapbox.com/tilemill/assets/pages/svg-icons-1.png)

1. `point-file`  
	This designates the **path** to the SVG relative to your project folder. In this case the SVG is located in Documents/MapBox/project/2010-tornadoes/icons/.
2. `point-allow-overlap`  
	Like other `-allow-overlap` parameters, this allows the images to be displayed even if they will be on top of each other.
3. `point-transform`  
	This is the parameter used to **scale** and **move** the image, among other things. A value of `"scale(1)"` will display the image at its original size, while `"scale(0.5)"` and `"scale(2)"` will display it at 50% and 200% respectively. You can also use this property to move the image vertically and horizontally by using the property `"translate()"`. For example, the value `"translate(20, -40)"` will move the image 20 pixels to the right and 40 pixels up. There are several other properties that you can employ with point-transform. [Learn about them on W3](http://www.w3.org/TR/SVG/coords.html#TransformAttribute).

#### Exporting for Compositing

When your map contains multiple levels of data as our tornado map does, it is sometimes wise to export each level separately. This has multiple benefits. Firstly, it compartmentalizes your map so that when updating you may not have to re-export the entire map. Secondly, it gives you greater flexibility when [compositing](https://www.mapbox.com/mapbox-studio/compositing-reference).

Thirdly, **interactivity** can only be active on one layer at a time. This means if we want a hover tooltip for the state-level dots and for the individual dots, we cannot export them together.

When exporting individual pieces of your project, a very helpful tool is the ability to **comment-out** specific CartoCSS code. Anything commented-out will remain in your code, but not render on the map. All this entails is placing `/*` before and `*/` after the code you want to comment-out. This is also a way to write comments into the code, hence the name.

We have plans to composite this tornado map with an existing world baselayer available fromMapbox, so the first thing to do will be to comment-out the default blue and white world base and the state borders.

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-1.png)

Notice that the code between the `/*` and `*/` is now greyed-out, and the background of the map is **transparent**.

Now we can create a tooltip for the state-level dots, and export. On the export page, be sure to select only the zoom levels for this particular piece of the map.

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-2.png)

Next we create and switch the tooltip to the individual dots layer, and export. Change the zoom levels and filename accordingly.

![](https://www.mapbox.com/tilemill/assets/pages/exporting-in-pieces-3.png)

We now have two MBTiles with their own interactivity that we can [composite](https://www.mapbox.com/mapbox-studio/compositing-reference) together with a slick base map.

Here is the final map: \<iframe width='600' height='400' frameBorder='0' src='https://a.tiles.mapbox.com/v3/mapbox.map-4qkj96dp.html#4/40/-98'\> \</iframe\>

And the final project CartoCSS code for reference:

	
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
	









