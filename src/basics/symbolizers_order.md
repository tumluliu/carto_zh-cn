### 3.6 符号的渲染顺序

通过CartoCSS配置好样式的地图将按照[Painter’s算法](http://en.wikipedia.org/wiki/Painter's_algorithm)来绘制地图中的各种对象。这些对象的渲染遵循特定的**顺序**，先绘制的对象可能会被后绘制的对象覆盖。

#### 概述

概括的说，地图上的对象要依照以下四个条件所确定的顺序进行绘制：

1. **图层的绘制顺序**：按照从低层到高层的顺序进行绘制，高层图层覆盖低层图层。
2. **从属样式的绘制顺序**：按照从属样式（例如`::glow { ... }`）在同一样式块中出现的顺序自顶向下进行绘制。
3. **从属样式所作用的要素对象的绘制顺序**：按照这些对象在原始数据集中的存储顺序进行绘制。
4. **同一对象上定义的多个样式实例的绘制顺序**：按照样式实例定义的顺序进行绘制。

接下来我们来详细解释这些规则。

#### 顺序vs.优先级

对于线要素和面要素对象来说，越是先被绘制出来的，越有可能在最终的地图上只能露出一部分甚至完全被遮住。因为在绘制堆栈中的高位对象，也就是那些按顺序应该后画的对象很可能会在绘制时把之前先画出来的对象部分或完全覆盖。所以我们说对于这一类要素，处于绘制堆栈中的高位对象具有**较高**的“优先级”或“重要性”。

而对于像文本标注、注记和图标等`allow-overlap`属性默认为`false`的符号，情况就有所不同了。对于用这些符号定义样式的对象，越是先被画出来的，就**越有可能**在最后的地图上可见。其原因是如果后画的对象会遮挡前面已经画出来的对象，那么这些对象就直接被忽略了，根本不会被画出来。所以说在这种情况下绘制堆栈中的高位对象可能会在绘制过程中反而被忽略掉，因而具有**较低**的“优先级”或“重要性”。

总而言之，就是对于线、面要素而言，绘制顺序比较靠后的对象具有更高的优先级，也就是更可能会在最终的地图上可见；而对于文本、注记和图标等要素，却是完全相反，即绘制顺序比较靠后的对象具有更低的优先级，更可能会在最终的地图上不可见。这就是本节讨论的所谓顺序与优先级的关系问题。

#### 图层排序

一幅地图中所包含的若干个图层是按照自底向上的顺序逐层绘制的。在一个典型的地图数据集中，像土地利用、水系这样的基础数据通常位于图层列表的底部。而那些在地图上不允许被遮盖的要素（如标注、图标等）则通常位于列表的顶部。

#### 从属样式排序

在一个图层内部，样式可以进一步通过使用`::`而被拆分为多个**从属样式**块。从属样式也可以被理解为子图层。

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
	

从属样式块将按照其被首次定义的顺序来绘制。因此在上面的例子中，`::outline`部分定义的线样式将先于`::inline`部分被画出来。

需要注意的是，实际上所有样式定义都被包含在某一个从属样式块之中。如果不显式定义一个从属样式块，那么CartoCSS还是会为这些“自由”样式定义生成一个默认的匿名从属样式块。在下面的例子中，`line-width: 2; line-color: white;`这两句样式定义实际上也属于一个从属样式块，一个隐式定义的默认、匿名从属样式块。与前面的不同之处仅仅在于其名字不叫`::inline`而已，所以最后渲染出来的效果和上面的完全一样。

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
	

#### 数据排序

在处理每个从属样式块时，其涉及到的每条要素数据在数据集（无论是在外存还是内存）中的排序对于渲染结果也有影响。在某些矢量数据集中，会为了保证制图渲染效果而专门对要素数据的存储顺序进行优化。

如果用于制图的矢量数据集是用户自己创建的、完全可控的，那么就应该要考虑其中要素数据记录的顺序问题。就拿城市名称标注来说，把用于标注的数据按照某种优先级排个序会对后面的制图有好处。如果这些数据是从关系数据库中查出来的，那么最好在`select`语句中用`ORDER BY`把它们按照某个列（或其它优先级规则）排一下序。

而如果数据来自磁盘文件，那么通常就只能是按照从头到尾的顺序去读，无法实时的调整数据记录的顺序。这种时候可以先把数据文件预处理一下，将其中要素数据记录的顺序调整好之后再把它作为图层加载并制图。

这种调整顺序的预处理可以用`ogr2ogr`在命令行完成。下面的命令就实现了把`cities.shp`文件中的所有要素记录按照`population`字段的值从大到小的顺序重新排列。

	
	ogr2ogr -sql \
	  'select * from cities order by population desc' \
	  cities_ordered.shp cities.shp
	

#### 符号排序

在每个从属样式中，可以为每个要素对象应用多种**符号**进行修饰。就拿面要素来说，它既可以有用面符号修饰的填充样式，又可以有用线符号修饰的边界线样式。在这种情况下，多个样式符号将按照其定义的顺序进行绘制。根据这样的原则，下面例子中的绘制顺序就是先边界线，后填充。

![](https://cloud.githubusercontent.com/assets/126952/3895677/2e921f72-2250-11e4-8643-8271bf00b3e9.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  line-width: 6;
	  polygon-fill: #aec;
	  polygon-opacity: 0.8;
	}
	

而在下面的例子中则正相反，是先画填充，再在它上面画边界线。

![](https://cloud.githubusercontent.com/assets/126952/3895679/2ea0ca40-2250-11e4-883c-a6b0b4d00847.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  polygon-fill: #aec;
	  polygon-opacity: 0.8;
	  line-width: 6;
	}
	

此外，还可以在一个从属样式块中利用具名的**实例**实现同一个样式属性的重定义。与从属样式的命名规则一样，实例也可以取任意的名字。实例的概念我们在3.1节中已经介绍过了。

![](https://cloud.githubusercontent.com/assets/126952/3895678/2e933cc2-2250-11e4-825e-571a633f24cc.png)

_图片来源：[Mapbox](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)_

	
	#layer {
	  bottomline/line-width: 6;
	  middleline/line-width: 4;
	  middleline/line-color: white;
	  topline/line-color: red;
	}
	

需要注意的是，各种排序也是有优先级和顺序的，而符号排序就是所有类型排序中优先级最低的，所以可能会出现一个多边形的边界线绘制在最上层，但却被相邻的多边形盖在下面的情况，这是因为数据排序的优先级高于符号排序。如果想要确保边界线总是被画在填充的下面，就需要另外定义一个从属样式来实现。

#### 图层数据源排序（Layer Source Ordering）

The order in which your layer sources are specified also influences rendering order: data from sources are rendered in order. So if you click “Change source” under “Layers” and you see `you.id123, mapbox.mapbox-terrain-v1`, the layers from `mapbox.mapbox-terrain-v1` will render last, over the layers from `you.id123`. To ensure that your own data renders last, use `mapbox.mapbox-terrain-v1, you.id123`.

图层的数据源的排序也会影响渲染顺序，它们会按照指定的顺序进行绘制。比如图层数据源列表为`you.id123, background-terrain`，那么从`background-terrain`数据中派生出的那些图层将被后绘制，盖在那些`you.id123`数据派生出的图层上面。为了保证用户自己的数据最后再绘制，需要把用户自己的数据集尽量放在图层数据源列表的尾部，例如`background-terrain, you.id123`。

#### 参考文献

1. Mapbox, [Mapbox Studio Symbol Drawing Order](https://www.mapbox.com/mapbox-studio/symbol-drawing-order/)



