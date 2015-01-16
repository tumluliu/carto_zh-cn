### 符号的渲染顺序（Symbol drawing order）

_译注：[原文地址](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

Objects in Mapbox Studio are drawn using a [Painter’s Algorithm](http://en.wikipedia.org/wiki/Painter's_algorithm), meaning everything is drawn in a specific order, and things that are drawn first might be covered by things that are drawn later.

通过CartoCSS配置好样式的地图将按照[Painter’s算法](http://en.wikipedia.org/wiki/Painter's_algorithm)来绘制图中的所有对象。这些对象的渲染是按照特定**顺序**进行的，先绘制的对象可能会被后绘制的对象覆盖。

#### 概述（Overview）

The order in which objects are drawn depends on the following conditions. See the sections that follow for more details.

地图要素依照以下条件所确定的顺序进行绘制。后面的小节中包含更多详细的解释。

1. Layers: “Higher” layers obscure “lower” ones.
2. Style attachments (eg, `::glow { ... }`) within a Stylesheet are applied from top to bottom.
3. Objects within an attachment are drawn in the order in which they are stored in the vector tile.
4. Multiple property instances on the same object (eg `a/line-color: blue; b/line-color: red;`) are drawn in the order they are defined.

1. 图层：位于更高层的图层会覆盖低层图层；
2. 同一样式块中的各个从属样式块（例如`::glow { ... }`）按照从上到下的顺序起作用；
3. 每个子符号所作用的要素对象按照它们在矢量瓦片中的存储顺序进行绘制；
4. 作用于同一对象上的多个样式属性按照属性定义的顺序起作用。

#### 顺序vs.优先级（Order vs. Priority）

For things like lines and areas, objects that are drawn first are less likely to be fully visible. Objects high in the stack might completely obscure other objects, thus you might associate these with a high ‘priority’ or ‘importance’.

对于线要素和面要素对象来说，越是先被绘制出来的，越有可能被后绘制的对象盖住，导致在最终的地图上只能看到这些对象的一部分甚至完全看不到。因为在绘制堆栈中的高位对象（译注：就是指那些按照顺序后绘制的对象）可能会在绘制时把其它对象完全覆盖，所以这些高位对象可以说天然的具有较高的“优先级”或“重要性”。

However for things like text, markers, and icons that have their _allow-overlap_ properties set to false (the default) things work a bit differently. Objects that are drawn first are **more** likely to be visible; instead of letting things sit on top of each other, overlapping objects are simply skipped. Since such objects higher in the stack are less likely to be drawn, you might associate these with a low ‘priority’ or ‘importance’.

而对于像文本标注、注记和图标等_allow-overlap_属性默认为`false`的符号，情况就有所不同了。对于使用这些符号定义样式的对象，越是先被绘制出来的，**越有可能**在最后的地图上可见，因为如果后绘制的对象会对前面已经绘制的对象造成压盖，那么这些对象根本就不会被画出来，而是直接被忽略了。所以说在这种情况下绘制堆栈中的高位对象可能会在绘制过程中反而被忽略掉，因而具有更低的“优先级”或“重要性”。

（译注：用一句简单的话说，就是对于线、面要素而言，绘制顺序比较靠后的对象具有更高的优先级，也就是更可能会在最终的地图上可见；而对于文本、注记和图标等要素，却是完全相反，即绘制顺序比较靠后的对象具有更低的优先级，更可能会在最终的地图上不可见。这也是本节讨论的所谓顺序与优先级的关系问题。）

#### 图层排序（Layer Ordering）

Layers are rendered in order starting at the bottom of the layers list moving up. If you look at the layers in the Mapbox Streets vector tile source you can see that the basic parts of the map (eg. landuse areas, water) are in layers at the bottom of the list. The things that shouldn’t be covered up by anything else (eg. labels, icons) are in layers at the top of the list.

图层是按照自底向上的顺序逐层绘制的。在一个典型的地图数据集中，像土地利用、水系这样的基础数据通常位于图层列表的底部。而那些在地图上不允许被遮盖的要素（如标注、图标等）则通常位于列表的顶部。

#### 从属样式排序（Attachment Ordering）

Within a layer, styles can be broken up into ‘attachments’ with the `::` syntax. Think of attachments like sub-layers.

在一个图层内部，样式可以进一步通过使用`::`而被拆分为多个“从属样式块”。子符号可以被理解为子图层。

![](https://cloud.githubusercontent.com/assets/126952/3895676/2e8e4686-2250-11e4-8655-7d4498470238.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  ::outline {
	    line-width: 6;
	    line-color: black;
	  }
	  ::inline {
	    line-width: 2;
	    line-color: white;
	  }
	}
	

Attachments are drawn in the order they are first defined, so in the example above the `::outline` lines will be drawn below the `::inline` lines.

从属样式块将按照其被首次定义的顺序来绘制。在上面的例子中，`::outline`部分定义的线样式将先于`::inline`部分被绘制。

Note that all styles are nested inside attachments. If you don’t explicitly define one, a default attachment still exists. Thus the following style produces the same result as the one above.

需要注意的是，实际上所有样式都是被定义在从属样式块之中的。如果不显式定义一个子符号，那么CartoCSS还是会为一系列样式属性生成一个默认的从属样式块。所以下面的样式代码能够产生与前面完全一样的渲染结果。（译注：注意第一句话中的nested不是“嵌套”的意思，而是单纯的“被包裹在…里面”的含义。具体而言，在下面的例子中，`line-width: 2; line-color: white;`两句话实际上也在一个从属样式块中，它是一个隐式定义的默认从属样式块，只是名字不叫`::inline`而已，所以最后渲染出来的效果和上面的完全一样。）

![](https://cloud.githubusercontent.com/assets/126952/3895676/2e8e4686-2250-11e4-8655-7d4498470238.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  ::outline {
	    line-width: 6;
	    line-color: black;
	  }
	  line-width: 2;
	  line-color: white;
	}
	

#### 数据排序（Data Ordering）

Within each attachment, the order that your data is stored/retrieved in is also significant. The ordering of objects in the Mapbox Streets vector tiles have been optimized for the most common rendering requirements.

在处理每个从属样式块时，其涉及到的每条要素记录的存储和检索后的顺序对于渲染结果也是很有影响的。在某些矢量数据集中，会为了保证制图渲染效果而专门对要素记录的存储顺序进行优化。

If you are creating a custom vector tile source this is something you will have to consider. When styling city labels, for example, it’s good to ensure that the order of your data makes sense for label prioritization. For data coming from an SQL database you should `ORDER BY` a population column or some other prioritization field in the select statement.

如果用户要创建一个自定义的矢量数据源，那么就必须要考虑数据记录的顺序问题。就拿城市名称标注来说，让用于标注的数据按照某种优先级保持一个良好的顺序是很有好处的。如果这些数据是从关系数据库中查出来的，那么最好在`select`语句中用`ORDER BY`把它们按照某个特定的列或优先级规则排个序。

Data coming from files are read from the beginning of the file to the end and cannot be re-ordered on-the-fly by TileMill. You’ll want to pre-process such files to make sure the ordering makes sense.

如果是从文件中读取数据，那么顺序通常只能是从文件的开头读到结尾，无法实时的调整数据记录的顺序。这种时候可以通过预处理来把数据记录的顺序调整得更合理。

You can do this from the terminal with ogr2ogr. This example rearranges all the objects in cities.shp based on the population field in descending order (highest population first).

这种调整顺序的预处理可以用`ogr2ogr`在命令行完成。下面的命令就实现了把`cities.shp`文件中的要素记录按照`population`字段的值从大到小的顺序重新排列。

	
	ogr2ogr -sql \
	  'select * from cities order by population desc' \
	  cities_ordered.shp cities.shp
	

#### 符号排序（Symbolizer Ordering）

Each object in each attachment may have multiple _symbolizers_ applied to it. That is, a polygon might have both a fill and an outline. In this case, the styles are drawn in the same order they are defined.

在每个从属样式块中，可以为每个要素对象应用多种_符号_进行修饰。就拿面要素来说，它既有用面符号修饰的填充样式，又有用线符号修饰的边界线样式。在这种情况下，多个样式符号将按照其定义的顺序进行绘制。

In this style, the outline will be drawn below the fill:

下面的例子中的绘制顺序是先画边界线，再在它上面画填充。

![](https://cloud.githubusercontent.com/assets/126952/3895677/2e921f72-2250-11e4-8643-8271bf00b3e9.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  line-width: 6;
	  polygon-fill: #aec;
	  polygon-opacity: 0.8;
	}
	

In this style, the line is drawn on top of the fill:

而在下面的例子中则相反，是先画填充，再在它上面画边界线。

![](https://cloud.githubusercontent.com/assets/126952/3895679/2ea0ca40-2250-11e4-883c-a6b0b4d00847.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  polygon-fill: #aec;
	  polygon-opacity: 0.8;
	  line-width: 6;
	}
	

It’s also possible to create multiple symbols of the same type within an attachment using named _instances_. Like attachments, their names are arbitrary.

此外，还可以在一个从属样式块中利用具名_实例_实现同一个样式属性的重定义。与从属样式的命名规则一样，实例也可以取任意的名字。（译注：一个“多重样式”的翻译就已经够让人蛋疼了，这又来了个“实例”，我去。。。）

![](https://cloud.githubusercontent.com/assets/126952/3895678/2e933cc2-2250-11e4-825e-571a633f24cc.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  bottomline/line-width: 6;
	  middleline/line-width: 4;
	  middleline/line-color: white;
	  topline/line-color: red;
	}
	

Note that symbolizer ordering happens after all other types of ordering - so an outline might be on top of one polygon but beneath a neighboring polygon. If you want to ensure lines are always below fills, use separate attachments.

需要注意的是，符号排序发生在所有类型排序的最后，所以可能会出现一个多边形的边界线绘制在最上层，但却被相邻的多边形盖在下面的情况（译注：这因为数据排序的优先级高于符号排序）。如果想要确保边界线总是在填充样式之下，就需要使用独立的从属样式块来实现。

#### 图层数据源排序（Layer Source Ordering）

The order in which your layer sources are specified also influences rendering order: data from sources are rendered in order. So if you click “Change source” under “Layers” and you see `you.id123, mapbox.mapbox-terrain-v1`, the layers from `mapbox.mapbox-terrain-v1` will render last, over the layers from `you.id123`. To ensure that your own data renders last, use `mapbox.mapbox-terrain-v1, you.id123`.

图层的数据源的排序也会影响渲染顺序，它们会按照指定的顺序进行绘制。比如图层数据源列表为`you.id123, background-terrain`，那么从`background-terrain`数据中派生出的那些图层将被后绘制，盖在那些`you.id123`数据派生出的图层上面。为了保证用户自己的数据最后再绘制，需要把用户自己的数据集尽量放在图层数据源列表的尾部，例如`background-terrain, you.id123`。



