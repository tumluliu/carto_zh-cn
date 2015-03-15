### 理解元瓦片

_译注：[原文地址](https://www.mapbox.com/tilemill/docs/guides/metatiles/)_

TileMill displays maps in small seamless chunks referred to as tiles. Although displayed individually, TileMill generates groups of images at once in batches before separating them into the final tiles - this improves efficiency in various ways.

在现代Web制图中，地图是由一系列小块无缝组成的。这些小块被称为**瓦片**。尽管瓦片最后是以独立图片的形式出现，但支持CartoCSS的Web制图软件及其底层的渲染引擎会先以组的方式批量处理瓦片，然后再对其拆分得到最终的瓦片。这种处理方式能够从多个方面改善制图效率。

When designing maps in TileMill it is sometimes necessary to understand how metatiles work in order to create your style effectively and work around certain types of issues. Metatile settings play a particularly important role when working with labels, markers, and patterns in your stylesheet.

在使用支持CartoCSS的工具进行制图时，为了能够更高效的配置地图样式，以及规避解决一些制图渲染中的特殊问题，了解一下**元瓦片（metatiles）**的工作机理是很有必要的。元瓦片在正确绘制地图样式中的标注、注记和图案等方面扮演着重要的角色。

#### 元瓦片的结构（Structure of a metatile）

There are two main parts to a metatile: the tiles and the buffer. By default a metatile in TileMill consists of 4 tiles (arranged 2 wide and 2 high) and a buffer of 256 pixels around the tiles.

元瓦片重要包括两个组成部分：瓦片和缓冲区。一个元瓦片在默认情况下由4个（2行2列）个瓦片和一个环绕在这4个瓦片周围、256像素宽的缓冲区组成。

![](https://www.mapbox.com/tilemill/assets/pages/metatile.png)

The area within the buffer is drawn but never displayed - the purpose is to allow labels, markers, and other things that cross over the edge of a metatile to display correctly. Without a buffer you would notice many cut-off labels and icons along the seams between every other tile.

元瓦片的缓冲区部分也会被渲染引擎（译注：也就是Mapnik）绘制，但这部分在最终的地图上不会被显示出来。这样做的目的是为了保证标注、注记等地图要素在跨越元瓦片边界的时候仍然能够被正确绘制。如果没有这个缓冲区，那么就会在每个瓦片的边缘处出现大量被切割的标注、图标等。

#### 调整元瓦片配置（Adjusting metatile settings）

TileMill allows you to configure the number of tiles included in a metatile as well as the width of the buffer.

元瓦片中包含的瓦片数量和缓冲区大小都是可以配置的。

To adjust the number of tiles, open the project settings (wrench icon) and drag the MetaTile size slider to your desired size. The number represents how many tiles high and wide your metatile will be, thus the total number of tiles in each batch will be the square of this value.

以TileMill为例，调整元瓦片中瓦片数量的方法是打开项目设置，然后拖动元瓦片大小滑块到合适的位置。滑块数值的平方代表了瓦片的个数。

Adjusting the width of the buffer is done in CartoCSS. Add a `buffer-size` property to your `Map` object. The value must be a whole number and represents pixel units. Example:

而缓冲区大小的调整则需要在CartoCSS中完成。在`Map`对象中增加一个`buffer-size`属性，它的值（必须是整数）就是缓冲区的像素宽度：

	
	Map {
	  background-color: white;
	  buffer-size: 256;
	}
	

#### 选择合理的缓冲区大小（Choosing your buffer size）

If you are noticing problems with your map such as cut off labels & icons you should try increasing your buffer size. A good starting point for choosing a buffer size is to make it about the width of your widest labels.

如果你发现在地图上出现了标注和图标被切割的情况，那么就应该考虑调整增大缓冲区了。但是应该把宽度设成多少呢？建议先找到地图上最宽的标注，然后从设成它的宽度开始尝试。

#### 选择合理的元瓦片尺寸（Choosing your metatile size）

For most projects it’s reasonable to use the default metatile size (2). This means that tiles will be rendered in 512 px chunks and then broken down into 256 px tiles before being returned to the map view. When one or more adjacent tile requests hit the same metatile the renderer will pause momentarily to process the metatile a single time before slicing and then returning each individual 256 px tile to the map view.

对于大多数情况，默认的元瓦片尺寸（也就是2）都什么没问题。这意味着渲染引擎会将地图内容先渲染到长宽均为512像素的块上，然后再将其切分成4个长宽为256像素的瓦片并返回给地图显示前端。当一个或者更多的相邻瓦片请求正好命中同一个元瓦片时，渲染引擎会在切片之前暂停很短的时间以处理元瓦片，然后再将每个独立的256像素瓦片返回给地图显示前端。

There is one reason why you might want to lower the size to 1, and two main reasons you may want to increase the metatile size above 2 to values like 8 or 16.

有一种可能的原因让你想要将元瓦片的尺寸减小到1；而有两种可能的原因让你想把这个值增大到8或16。下面分别介绍这两种情况。

##### 减小元瓦片尺寸（Going smaller）

There is only one size down from the default metatile size of 2; this effectively disables metatiling. Doing this can help the map view feel slightly more responsive during editing and light browsing because each individual tile will appear as fast as it can, alone, be rendered. If the current part of the map you are viewing has some tiles with lots of data and other tiles with less data, avoiding metatiling will ensure that the tiles with less data will load quicker than adjacent tiles with more data.

要把元瓦片尺寸从默认值2调小，那么只可能是调成1，也就是关闭元瓦片功能。这样做可以让地图在编辑制图样式的过程中响应更快，因为这时每个瓦片都是独立渲染的了。如果地图中某些瓦片对应的数据较多，而另一些瓦片上的数据较少，那么关闭元瓦片功能之后会使包含数据较少的瓦片比包含数据较多的瓦片更快绘制出来。

##### 增大元瓦片尺寸（Going larger）

###### 原因1：减少导出时间（Reason 1: reducing export time）

While disabling metatiling can give a more responsive feel to the map UI, the opposite is true when exporting to MBTiles. Increasing the metatile size can significantly increase overall performance and decrease the overall time it takes to render an entire export job. This is because rendering many tiles in sequence using larger metatiles means doing less overall work.

如果说关闭元瓦片功能可以让制图过程感觉响应速度更快，那么反过来（增大元瓦片尺寸）则可以让导出地图到MBTiles的过程变得更快。增大元瓦片的尺寸可以显著提高地图整体的渲染性能，降低对导出任务的渲染时间。其原因是在穿行渲染大量瓦片时，如果使用尺寸更大的元瓦片，那么就意味着需要更少的整体工作量。（译注：这里的潜台词是：渲染4个小瓦片的时间要大于渲染1个大瓦片的时间。但事实是这样吗？原因何在？极端情况下，把整张地图全画在一个超级大瓦片上会是最快的吗？）

There is no hard rule about how large your metatile should be for the best export performance. It will depend on how much data your project contains, how well it is spatially indexed, and how much memory your machine has.

然而到底使用多大的元瓦片才能获得最佳的导出性能呢？这的确是没有个硬性准则的。这和地图中包含多少数据量、有没有建空间索引、机器的内存有多大等许多因素都有关。

We recommend experimenting by setting up a reduced export job (just a few zoom levels or perhaps a more restricted area) and testing the export completion time as you gradually increase the metatile size to 4, 8, or 16.

我们的建议是先取地图的一小部分（只选几个缩放级别，或者框一小块区域）做做测试，看看把元瓦片的尺寸分别设成4、8或16时导出性能会有什么变化。然后根据实验结果选取最佳的元瓦片大小。

###### 原因2：减少瓦片边缘的切割问题（Reason 2: reducing rendering problems at tile edges）

Larger metatiles mean that things like labels and markers are less likely to be rendered at a tile edge. It also means that labeling algorithms, like the one that can throw out duplicate names (if text-min-distance is set) can work over larger areas of the map and will be more successful at reducing duplicates for a given view.

更大的元瓦片意味着标注、注记等要素被绘制在瓦片边缘的概率会降低。此外，对于一些标注算法（比如在设置了`text-min-distance`属性时控制重复绘制标注的间距），更大的元瓦片可以让它有更大的工作空间，从而得到更好的标注绘制效果。
